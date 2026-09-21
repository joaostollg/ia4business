# Regras

> Regras que uma rotina segue para decidir sozinha. Cada uma tem sete campos:
> Nome, Gatilho (quando olha), Fonte (onde está o dado), Condição (o que precisa
> ser verdade, com número), Ação (o que acontece), Quem recebe (pessoa e canal) e
> Se não disparar (o que fica registrado quando está tudo bem). O log de execução
> de cada regra fica em [automacoes.md](automacoes.md).

## Regra 0 — Fonte indisponível (trava geral, vale para todas as regras)

> Esta regra vem **antes** de todas as outras. Enquanto ela não passar, nenhuma
> outra regra roda. Existe porque uma análise feita sobre dado que não chegou é
> pior que análise nenhuma: ela parece certa.

- **Nome:** Fonte indisponível
- **Gatilho:** Sempre, como primeiro passo de qualquer execução — automática ou
  sob pedido — antes de avaliar a condição de qualquer outra regra.
- **Fonte:** As duas fontes do projeto:
  1. **FakeERP** — `POST /auth/login` e `GET /report/{ano}/{mês}`, conforme
     [fake-erp.md](fake-erp.md).
  2. **Arquivo local** — `dados/amostra.csv`.
- **Condição** (dispara se **qualquer uma** for verdadeira):
  - O FakeERP não responde: erro de rede, tempo esgotado, ou HTTP que não seja
    200 — inclusive **502** (servidor fora do ar), 500, 503, ou 401/403 que não
    se resolva com um novo login.
  - O arquivo `dados/amostra.csv` **não existe** na pasta `dados/`, está vazio
    ou não pode ser lido.
- **Ação:** Responder exatamente **"fonte indisponível"** e **parar a análise ali**.
  Não seguir para a condição de nenhuma outra regra, não escrever alarme no
  Notion, não gerar nem regenerar painel.
  **Proibido em qualquer hipótese:** estimar, deduzir, arredondar, reaproveitar
  número de execução anterior, usar valor de arquivo `_OLD`, completar com
  memória do que "costuma ser" ou apresentar qualquer número como se tivesse
  vindo da fonte. **Não inventar absolutamente nada.** Dado que não chegou não
  vira número — vira "fonte indisponível".
- **Quem recebe:** João, na resposta da própria execução. Além disso, grava uma
  linha em `automacoes.md` com data/hora, qual fonte falhou e o erro observado
  (ex.: "HTTP 502 no login"), para ficar a prova de que a regra rodou e parou.
  Nada é escrito no Notion — página de alarme é para problema de negócio, e
  fonte fora do ar é problema de infraestrutura.
- **Se não disparar** (fonte respondeu e o arquivo existe): não registra nada
  por si só; segue normalmente para a Regra 1 e a Regra 2, que fazem o próprio
  registro.

### Atenção — o que esta regra NÃO cobre

Fonte **indisponível** é diferente de fonte **vazia**. Se o FakeERP responder
HTTP 200 com `count: 0`, a fonte está disponível e funcionando: ela respondeu, e
a resposta é "não há pedido neste mês". A Regra 0 **não** dispara nesse caso — a
execução segue para a Regra 1, que hoje trata esse R$ 0,00 como receita abaixo
da meta. Esse é o ponto cego registrado como pendência em
[automacoes.md](automacoes.md) e continua em aberto.

## Regra 1 — Meta de vendas do mês abaixo do esperado

- **Nome:** Meta de vendas do mês abaixo do esperado
- **Gatilho:** Tempo. Todo dia às 8h, **a partir do dia 20 do mês**. Antes do dia
  20 a regra não checa — receita baixa no início do mês é normal, não é sinal de
  nada (evita alarme falso todo começo de mês). Também pode ser rodada sob pedido
  direto ("confere a meta de vendas do mês").
- **Fonte:** FakeERP, relatório de pedidos do mês atual —
  `GET /report/{ano}/{mês}`, acesso documentado em [fake-erp.md](fake-erp.md).
- **Condição:** Receita paga do mês atual **menor que R$ 3.000,00**.
  Receita paga = soma do campo `total` apenas dos pedidos com `status == "PAID"`.
  Pedidos `CANCELLED` e `PENDING` não entram na conta (ver a pegadinha documentada
  em `fake-erp.md`).
- **Ação:** Escrever um alarme na página **Alertas** do Notion, com: o valor da
  receita paga apurada, a meta (R$ 3.000,00), quanto falta para bater a meta, e o
  mês/ano de referência. Título do alarme: "⚠️ Vendas abaixo da meta — `mês/ano`".
- **Quem recebe:** João, na página Alertas do Notion.
- **Se não disparar** (receita paga ≥ R$ 3.000,00): não escreve nada no Notion.
  Grava uma linha em `automacoes.md` com data/hora da checagem, mês verificado,
  receita paga apurada e "meta OK".

### Como testar manualmente

"Mês atual" depende do calendário real no momento em que a regra roda. Para o
teste de hoje, foram usados dois meses concretos do FakeERP com dado real, um de
cada lado da condição:

| Mês testado | Receita paga (só PAID) | < R$ 3.000? | Resultado esperado |
|---|---|---|---|
| Janeiro/2026 | R$ 1.430,00 (pedidos 1001 e 1002; 1003 cancelado e 1004 pendente ficam de fora) | Sim | **Dispara** → alarme no Notion |
| Fevereiro/2026 | R$ 3.149,90 (pedidos 1005, 1006 e 1007, todos pagos) | Não | **Fica calada** → registra em `automacoes.md` |

### Dado ao vivo — setembro/2026 (mês atual em 14/09/2026)

Checado direto na API em 14/09/2026: `count: 0`, receita paga R$ 0,00 — mês
ainda sem pedido lançado. Hoje é dia 14, **antes do dia 20** — pelo gatilho
corrigido, a rotina automática ainda nem chegaria a checar. Simulação manual
"como se já fosse dia ≥20, com o dado de hoje": R$ 0,00 < R$ 3.000 → **dispararia**.
Isso é esperado (mês zerado é abaixo da meta de verdade se persistir até o fim);
o que evitamos foi o alarme falso dos primeiros dias do mês.

## Regra 2 — Queda de receita paga em relação ao mês anterior

- **Nome:** Queda de receita paga em relação ao mês anterior
- **Gatilho:** Tempo. Todo dia **1** de cada mês, às 8h — o mês anterior já
  fechou por completo, então dá pra comparar mês fechado contra mês fechado
  (evita comparar mês parcial com mês inteiro, que sempre pareceria queda).
- **Fonte:** FakeERP, relatório do mês que acabou de fechar e do mês
  imediatamente anterior a ele — `GET /report/{ano}/{mês}` duas vezes, acesso
  documentado em [fake-erp.md](fake-erp.md).
- **Condição:** Receita paga do mês que fechou é **mais de 20% menor** que a
  receita paga do mês anterior a ele. Receita paga = mesma regra da Regra 1
  (soma do `total` só dos pedidos `PAID`).
- **Ação:** Escrever um alarme na página **Alertas** do Notion, com: receita
  paga do mês que fechou, receita paga do mês anterior, a queda em % e os
  pedidos `PAID` considerados em cada um dos dois meses. Título: "📉 Queda de
  receita — `mês/ano` vs `mês anterior`".
- **Quem recebe:** João, na página Alertas do Notion.
- **Se não disparar** (queda ≤ 20%, ou alta): não escreve nada no Notion.
  Grava uma linha em `automacoes.md` com os dois meses comparados, a receita
  paga de cada um, a variação em % e "sem queda relevante".

### Como testar manualmente

| Mês fechado | Receita paga | Mês anterior | Receita paga | Variação | Resultado esperado |
|---|---|---|---|---|---|
| Março/2026 | R$ 800,00 (só o pedido 1009 — 1008 pendente e 1010 cancelado ficam de fora) | Fevereiro/2026 | R$ 3.149,90 | **-74,6%** | **Dispara** → alarme no Notion |
| Fevereiro/2026 | R$ 3.149,90 | Janeiro/2026 | R$ 1.430,00 | +120,3% (alta, não queda) | **Fica calada** → registra em `automacoes.md` |

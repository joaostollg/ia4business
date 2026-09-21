# Regras

> Regras que uma rotina segue para decidir sozinha. Cada uma tem sete campos:
> Nome, Gatilho (quando olha), Fonte (onde está o dado), Condição (o que precisa
> ser verdade, com número), Ação (o que acontece), Quem recebe (pessoa e canal) e
> Se não disparar (o que fica registrado quando está tudo bem). O log de execução
> de cada regra fica em [automacoes.md](automacoes.md).

**O negócio por trás destas regras:** boutique de assessoria a provedores de
internet (ISPs) regionais do interior de SP, ligada ao Grupo Sun — vende
serviços recorrentes (contábil, jurídico, marketing) como porta de entrada e
assessoria de M&A como objetivo final da relação. O cliente pagante é o **dono
do ISP**, lado vendedor. Contexto completo em
[contexto/negocio.md](contexto/negocio.md) e
[contexto/cliente.md](contexto/cliente.md).

| Regra | Sobre o quê | Fonte |
|---|---|---|
| **0** | Trava geral — fonte indisponível | as duas abaixo |
| **1** | Meta de vendas do mês | FakeERP (**base de treino**) |
| **2** | Queda de receita mês a mês | FakeERP (**base de treino**) |
| **3** | Cliente recorrente sem entrega no mês | operação real da boutique |

As Regras 1 e 2 rodam sobre o FakeERP, o ERP de treino das Aulas 10–12 — é
prática de "consumir uma API e decidir em cima do dado", não a operação da
boutique. Quando houver um ERP/CRM de verdade, a mecânica das duas se aplica
igual, trocando "pedido pago" por "retainer recebido". A **Regra 3** é a que
mede o negócio real.

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

### Contagem de linhas — obrigatória em toda leitura de arquivo

Sempre que uma regra ler `dados/amostra.csv` (ou qualquer outro arquivo de
dado), a resposta **tem que dizer quantas linhas foram lidas, quantas foram
ignoradas e por quê** — mesmo quando nenhuma foi ignorada ("li 12 linhas,
ignorei 0"). Linha ignorada sem aviso é número errado com cara de número certo.

Conta como linha a ignorar, e cada uma precisa ser nomeada no aviso:

| O que apareceu | O que fazer |
|---|---|
| Linha vazia ou incompleta | Não conta como pedido. Avisar qual. |
| Valor negativo em `value`, `discount` ou `total` | Não somar. Avisar qual pedido e qual campo. |
| Data fora do formato `aaaa-mm-ddThh:mm:ss` | Não chutar o mês. Avisar qual pedido e qual formato veio. |
| `status` diferente de `PAID`/`CANCELLED`/`PENDING` | Não classificar por conta própria. Avisar. |

**Nunca criar período que não existe.** Se a data de um pedido não puder ser
lida, ele fica de fora da contagem e é reportado — jamais vira um mês novo na
tabela. Um período com nome estranho no relatório é sinal de data mal lida, não
de mês novo no negócio.

**Linha que some não é linha ignorada — e é mais perigosa.** Contar as linhas
que o leitor entregou não basta: uma linha em branco no meio do arquivo é
descartada pelo próprio leitor de CSV antes de a contagem acontecer, então ela
não aparece nem como "ignorada". Por isso, toda leitura precisa **conferir o
número de linhas físicas do arquivo contra o número de registros processados** e
avisar se não baterem. Confirmar também que os `order_id` não têm buraco na
sequência. Descoberto no teste de 21/09/2026, ver [testes.md](testes.md).

**Se mais de 20% das linhas forem ignoradas**, tratar como fonte corrompida:
aplicar a Regra 0 ("fonte indisponível") e parar, em vez de entregar um
relatório construído sobre o que sobrou.

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

## Regra 3 — Cliente recorrente sem entrega registrada no mês

> As Regras 1 e 2 rodam sobre o FakeERP, que é **base de treino** (pedidos de
> uma loja), não sobre a operação da boutique. Esta regra existe porque o risco
> que realmente derruba este negócio não é receita de pedido: é cliente que
> paga o retainer e não vê entrega. `contexto/cliente.md` registra isso como o
> motivo nº 1 de churn — "falta de resultado/entrega percebida" — e
> `contexto/negocio.md` diz que a receita previsível vem justamente desse
> retainer mensal.

- **Nome:** Cliente recorrente sem entrega registrada no mês
- **Gatilho:** Tempo. Todo dia **25** de cada mês, às 8h. O dia 25 é de
  propósito: sobram cinco dias úteis para produzir e mostrar alguma entrega
  antes de o mês fechar. Alarme no dia 30 só serviria para constatar o dano.
- **Fonte:** `dados/clientes.md` — um registro por cliente com: nome do ISP,
  serviços recorrentes contratados, valor do retainer e **data da última
  entrega registrada**.
  ⚠️ **Esse arquivo ainda não existe.** A boutique está em estágio de
  ideia/validação, sem cliente pagante fechado (`contexto/negocio.md`). Até o
  primeiro contrato assinado, esta regra cai na **Regra 0** e responde "fonte
  indisponível" — em voz alta, registrada no `automacoes.md`. Ela não finge
  que está tudo bem, e não fica calada.
- **Condição:** Cliente com contrato recorrente ativo e **nenhuma entrega
  registrada nos últimos 30 dias** contados a partir da data da checagem.
- **Ação:** Escrever um alarme na página **Alertas** do Notion com: nome do
  cliente, serviços contratados, quantos dias desde a última entrega e qual foi
  a última entrega registrada. Título: "🔻 Cliente sem entrega há N dias —
  `nome do ISP`".
- **Quem recebe:** João, na página Alertas do Notion.
- **Se não disparar** (todo cliente ativo teve entrega nos últimos 30 dias):
  não escreve nada no Notion. Grava uma linha em `automacoes.md` com a data da
  checagem, quantos clientes foram verificados e "todos com entrega no período".

### Por que esta regra e não outra

O cliente da boutique é um fundador mais velho que decide sozinho, cuja
concorrência **é a inércia** — ele não está comparando a boutique com outra
assessoria, está comparando com não fazer nada (`contexto/cliente.md`). Um mês
sem entrega visível não gera reclamação: gera silêncio, e o silêncio vira não
renovação. É o mesmo padrão dos três cenários de falha testados em
[testes.md](testes.md) — o problema não avisa, ele só aparece depois.

Por isso a regra mede **entrega registrada**, não satisfação declarada. Satisfação
o cliente não reclama até ir embora; entrega é verificável hoje, pela boutique,
sem depender de o dono do ISP falar alguma coisa.


## Prova de vida — silêncio não é sinal de OK

> Regra sobre as regras. Descoberta no teste do cenário 3 em 21/09/2026, quando
> se constatou que a rotina da Regra 1 não deixou rastro nos dias 20 e 21/09,
> embora devesse ter rodado nos dois. Ninguém percebeu, porque uma automação que
> não roda produz exatamente o mesmo silêncio de uma automação que rodou e não
> achou problema.

- **Toda execução deixa linha**, dispare ou não. Isso já está em cada regra, mas
  aqui vira obrigação verificável: `automacoes.md` é o comprovante de vida da
  automação, não o arquivo de alarmes.
- **Ausência de linha é falha**, não é "estava tudo bem". Se uma regra deveria
  ter rodado num dia e não existe linha daquele dia, o estado correto a assumir
  é **"não sei"** — nunca "sem problema". Registrar como falha de execução.
- **Ao checar qualquer regra sob pedido**, conferir antes se as execuções
  automáticas anteriores deixaram rastro. Se faltar dia, dizer isso junto com o
  resultado, mesmo que o resultado do dia esteja bom.
- **Regra sem agendamento não é regra automática** — é documentação de intenção.
  Enquanto uma regra depender de alguém lembrar de pedir, o `automacoes.md`
  precisa dizer isso com todas as letras, na linha dela.

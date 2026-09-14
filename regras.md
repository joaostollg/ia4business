# Regras

> Regras que uma rotina segue para decidir sozinha. Cada uma tem sete campos:
> Nome, Gatilho (quando olha), Fonte (onde está o dado), Condição (o que precisa
> ser verdade, com número), Ação (o que acontece), Quem recebe (pessoa e canal) e
> Se não disparar (o que fica registrado quando está tudo bem). O log de execução
> de cada regra fica em [automacoes.md](automacoes.md).

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
| Fevereiro/2026 | R$ 3.149,90 (pedidos 1005, 1006 e 1007, todos pagos) | Não | **Fica calada** → registra em `historico.md` |

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

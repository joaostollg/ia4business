# Automações

> Registro de execução das regras de [regras.md](regras.md) e de como o
> [painel.html](painel.html) é atualizado. É aqui que fica a prova de que uma
> regra rodou e ficou calada porque estava tudo bem (não só as vezes em que ela
> dispara) — e o que manter, consertar ou matar quando alguma parar de valer a
> pena.

## Regras ligadas

- Arquivo das regras: `regras.md` (2 regras: *Meta de vendas do mês abaixo do
  esperado* e *Queda de receita paga em relação ao mês anterior*)
- Como roda: rotina agendada na nuvem (`claude.ai/code/routines`), diariamente
  às 8h (horário de Brasília), dias 20–31 — cobre a Regra 1. A Regra 2 (checagem
  no dia 1) ainda roda à mão, sob pedido; falta um segundo agendamento para o
  dia 1 de cada mês.
- Rotina: **Meta de vendas do mês — FakeERP** —
  `https://claude.ai/code/routines/trig_013tK1Kz1xxJ5jNKoHZcJL69`

## Execuções

| Data/hora | Regra | Mês(es) verificado(s) | Receita paga | Disparou? | O que aconteceu |
|---|---|---|---|---|---|
| 14/09/2026 | Meta de vendas do mês abaixo do esperado | Janeiro/2026 | R$ 1.430,00 | **Sim** | Alarme escrito na página [Alertas](https://app.notion.com/p/3db58fad60ea81b2b581c8729ab12725) do Notion |
| 14/09/2026 | Meta de vendas do mês abaixo do esperado | Fevereiro/2026 | R$ 3.149,90 | Não | Meta OK — nada escrito no Notion, só este registro |
| 14/09/2026 | Meta de vendas do mês abaixo do esperado | Setembro/2026 (mês atual, dado ao vivo) | R$ 0,00 | Sim (simulação) | Mês ainda sem pedido lançado. Gatilho real só age a partir do dia 20 — hoje é dia 14, então isto não é um disparo de produção, é a condição testada com o dado real de hoje. Registrado no Notion como "(simulação)" |
| 14/09/2026 | Queda de receita paga em relação ao mês anterior | Março/2026 vs Fevereiro/2026 | R$ 800,00 vs R$ 3.149,90 (-74,6%) | **Sim** | Alarme escrito na página [Alertas](https://app.notion.com/p/3db58fad60ea81b2b581c8729ab12725) do Notion |
| 14/09/2026 | Queda de receita paga em relação ao mês anterior | Fevereiro/2026 vs Janeiro/2026 | R$ 3.149,90 vs R$ 1.430,00 (+120,3%) | Não | Sem queda relevante — nada escrito no Notion, só este registro |

## Painel

- **Arquivo:** [`painel.html`](painel.html) — v1, gerado em 14/09/2026.
- **Fontes:** [`dados/fonte.md`](dados/fonte.md) (regra de cálculo dos 3
  números) + [`dados/amostra.csv`](dados/amostra.csv) (dado bruto). Os dados
  ficam embutidos dentro do próprio `painel.html` — abre com duplo clique,
  sem internet e sem servidor.
- **Como é atualizado hoje:** manualmente. Quando `dados/amostra.csv` mudar
  (nova exportação do FakeERP), pedir para regenerar com o prompt:
  > "Lê o dados/fonte.md e o dados/amostra.csv desta pasta. Gera um arquivo
  > painel.html na raiz do projeto mostrando os 3 números declarados no
  > fonte.md... usa exatamente as definições de cálculo do fonte.md e não
  > inventa outra; embute os dados dentro do próprio painel.html."
- **Períodos disponíveis hoje:** Janeiro, Fevereiro, Março e Julho de 2026 —
  os únicos meses com pedido no FakeERP (mesma lista de `fake-erp.md`).
  Padrão exibido: Janeiro/2026 (bate com os valores de conferência de
  `dados/fonte.md`: 2 pedidos pagos, R$ 1.430,00, R$ 715,00 — conferido).
- **Pendência:** nenhuma rotina regenera o painel sozinha ainda — é sempre
  sob pedido.

## Pendências

- Agendar a Regra 2 (dia 1 de cada mês, 8h) como segunda rotina — hoje só a
  Regra 1 está agendada.
- Reconferir setembro/2026 a partir do dia 20, quando o gatilho da Regra 1
  realmente entra em ação.

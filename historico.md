# Histórico

> Registro de execução das regras de [regras.md](regras.md). Equivalente ao que a
> Aula 11 chama de `automacoes.md` — nome escolhido pelo João. É aqui que fica a
> prova de que uma regra rodou e ficou calada porque estava tudo bem (não só as
> vezes em que ela dispara).

## Regras ligadas

- Arquivo das regras: `regras.md` (1 regra: *Meta de vendas do mês abaixo do esperado*)
- Como roda hoje: à mão, no Claude Code, sob pedido ("confere a meta de vendas do mês")
- Produção prevista: rotina diária às 8h (ainda não agendada — ver pendências)

## Execuções

| Data/hora | Regra | Mês verificado | Receita paga | Disparou? | O que aconteceu |
|---|---|---|---|---|---|
| 14/09/2026 | Meta de vendas do mês abaixo do esperado | Janeiro/2026 | R$ 1.430,00 | **Sim** | Alarme escrito na página [Alertas](https://app.notion.com/p/3db58fad60ea81b2b581c8729ab12725) do Notion |
| 14/09/2026 | Meta de vendas do mês abaixo do esperado | Fevereiro/2026 | R$ 3.149,90 | Não | Meta OK — nada escrito no Notion, só este registro |
| 14/09/2026 | Meta de vendas do mês abaixo do esperado | Setembro/2026 (mês atual, dado ao vivo) | R$ 0,00 | Sim (simulação) | Mês ainda sem pedido lançado. Gatilho real só age a partir do dia 20 — hoje é dia 14, então isto não é um disparo de produção, é a condição testada com o dado real de hoje. Registrado no Notion como "(simulação)" |

## Pendências

- Agendar a rotina diária das 8h (`claude.ai/code/routines` ou equivalente) — ainda não configurada, precisa de confirmação antes de criar.
- ~~A API do FakeERP estava fora do ar~~ — voltou ao normal em 14/09/2026, reconferida com sucesso (relatório de setembro/2026 ao vivo).
- Reconferir setembro/2026 a partir do dia 20, quando o gatilho da regra realmente entra em ação.

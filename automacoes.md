# Automações

> Registro de execução das regras de [regras.md](regras.md) e de como o
> [painel.html](painel.html) é atualizado. É aqui que fica a prova de que uma
> regra rodou e ficou calada porque estava tudo bem (não só as vezes em que ela
> dispara) — e o que manter, consertar ou matar quando alguma parar de valer a
> pena.

## Regras ligadas

- Arquivo das regras: `regras.md` (3 regras: *Fonte indisponível* — trava geral
  que roda antes das outras duas —, *Meta de vendas do mês abaixo do esperado* e
  *Queda de receita paga em relação ao mês anterior*)
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
| 21/09/2026 08:57 | Meta de vendas do mês abaixo do esperado | Setembro/2026 (mês atual) | — não apurada | **Não checou** | Primeira execução com o gatilho real valendo (dia 21 ≥ 20). FakeERP fora do ar: `POST /auth/login` e `GET /v3/api-docs` devolveram HTTP 502 (Cloudflare, origem caída) em 3 tentativas. Sem token não há como chamar `GET /report/2026/9`. Nada escrito no Notion — a condição não chegou a ser avaliada. **Refazer quando a API voltar.** |
| 21/09/2026 09:00 | Meta de vendas do mês abaixo do esperado | Setembro/2026 (mês atual, dado ao vivo) | R$ 0,00 | **Sim** | API voltou (queda durou ~3 min). `GET /report/2026/9` devolveu `count: 0`, nenhum pedido no mês → receita paga R$ 0,00 < R$ 3.000. Primeira execução com o gatilho real valendo (dia 21). Alarme escrito na página [Alertas](https://app.notion.com/p/3db58fad60ea81b2b581c8729ab12725) do Notion, **com ressalva explícita de base vazia**: setembro não é um dos meses com dado na base de treino (só jan/fev/mar/jul de 2026), então o R$ 0,00 é ausência de dado, não queda de vendas real. |

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
- **Ponto cego da Regra 1 — mês vazio vs. mês ruim (aberto em 21/09/2026).** A
  regra só pergunta "receita paga < R$ 3.000?" e por isso não distingue duas
  situações muito diferentes que dão o mesmo resultado: (a) o mês existe, o ERP
  está sendo alimentado e mesmo assim entrou pouco dinheiro — alarme legítimo;
  (b) o mês não tem nenhum pedido lançado — aí o R$ 0,00 é notícia sobre a base,
  não sobre as vendas. Foi o que aconteceu com setembro/2026 em 21/09.
  *Correção proposta (não aplicada):* se o relatório vier com `count == 0`, a
  regra não escreve alarme de vendas; registra "mês sem pedido lançado" e, se
  for o caso, avisa que a fonte pode estar parada. Dois problemas diferentes,
  dois avisos diferentes. **Decisão do João em 21/09: anotar agora, mexer na
  regra depois.**
- Reconferir setembro/2026 quando houver pedido lançado no mês — a checagem de
  21/09/2026 rodou com a base zerada.

# Automações

> Registro de execução das regras de [regras.md](regras.md) e de como o
> [painel.html](painel.html) é atualizado. É aqui que fica a prova de que uma
> regra rodou e ficou calada porque estava tudo bem (não só as vezes em que ela
> dispara) — e o que manter, consertar ou matar quando alguma parar de valer a
> pena.

## Regras ligadas

- Arquivo das regras: `regras.md` (4 regras + 1 seção de disciplina):
  *Regra 0 — Fonte indisponível* (trava geral, roda antes de todas),
  *Regra 1 — Meta de vendas do mês abaixo do esperado*,
  *Regra 2 — Queda de receita paga em relação ao mês anterior*,
  *Regra 3 — Cliente recorrente sem entrega registrada no mês* (a única sobre a
  operação real da boutique; as Regras 1 e 2 rodam sobre o FakeERP, base de
  treino), e a seção *Prova de vida — silêncio não é sinal de OK*.
- **Como roda hoje: à mão, sob pedido. Nenhuma regra tem agendamento ativo.**
  Esta linha foi corrigida em 21/09/2026 — antes dizia que a Regra 1 rodava por
  rotina agendada na nuvem, diariamente às 8h nos dias 20–31. O teste do
  cenário 3 (ver [testes.md](testes.md)) mostrou que isso não se sustenta:
  20/09 e 21/09 foram os dois primeiros dias da janela declarada e **não existe
  registro de execução de nenhum dos dois**, nem aqui nem na página Alertas do
  Notion. O agendador consultável nesta máquina não lista nenhuma tarefa.
- Rotina que constava como agendada (**Meta de vendas do mês — FakeERP**,
  `claude.ai/code/routines/trig_013tK1Kz1xxJ5jNKoHZcJL69`): mantida aqui como
  registro do que foi declarado, **não como prova de que está no ar**. Até que
  um agendamento seja confirmado e deixe rastro neste arquivo, tratar toda
  regra deste repositório como execução manual.

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
| 21/09/2026 09:30 | **Teste de falha — cenário 1** (fonte fora do ar) | `dados/amostra.csv` ausente + FakeERP com HTTP 502 | não apurada | n/a | Teste de propósito, ver [testes.md](testes.md). Achado: o `painel.html` continuou abrindo normal, mostrando R$ 1.430,00 e citando no subtítulo um arquivo que não existia mais. Conserto: Regra 0. |
| 21/09/2026 09:40 | **Teste de falha — cenário 2** (dado inesperado) | 3 de 12 linhas do `amostra.csv` avariadas | Fev: R$ 2.050,10 (verdade: R$ 3.149,90) | n/a | Relatório saiu completo e sem aviso, com um período inventado ("19/03/2") e Fevereiro 35% menor. **A decisão da Regra 1 virou ao contrário** — mês que bateu a meta viraria alarme. Conserto: contagem de linhas lidas/ignoradas + conferência de buraco na sequência de `order_id`. Fonte restaurada ao original após o teste. |
| 21/09/2026 09:50 | **Teste de falha — cenário 3** (condição que nunca dispara) | Regras 1 e 2, dias 20 e 21/09 | n/a | n/a | Nenhum registro de execução nos dois primeiros dias da janela declarada. Silêncio da automação era indistinguível de "tudo bem". Conserto: seção "Prova de vida" em `regras.md` + correção da declaração de agendamento acima. |

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
- **Agendar de verdade as Regras 1, 2 e 3 (aberto em 21/09/2026).** Nenhuma tem
  agendamento ativo. Enquanto isso não existir, a operação depende de alguém
  lembrar de pedir — e, pela seção "Prova de vida" do `regras.md`, cada dia sem
  linha neste arquivo conta como falha de execução, não como "sem problema".
- **`painel.html` sem carimbo de validade (aberto em 21/09/2026).** O painel traz
  os dados embutidos e continua abrindo normalmente mesmo com a fonte ausente,
  como o cenário 1 de `testes.md` demonstrou. A Regra 0 protege quem pede análise
  nova, mas não protege quem abre o HTML com dois cliques. Falta o painel exibir
  de forma visível a data do dado que está mostrando.
- **`dados/clientes.md` não existe.** É a fonte da Regra 3, e só passa a existir
  quando houver o primeiro contrato recorrente assinado. Até lá, a Regra 3 cai na
  Regra 0 e responde "fonte indisponível" — em voz alta, não em silêncio.

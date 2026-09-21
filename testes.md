# Testes de falha

Testado em 21/09/2026.

> Os três cenários que derrubam qualquer automação, rodados de propósito contra
> a operação deste repositório: as regras de [regras.md](regras.md), a fonte
> [dados/fonte.md](dados/fonte.md) + `dados/amostra.csv`, e o
> [painel.html](painel.html). O que está escrito aqui aconteceu — nenhum
> cenário foi descrito sem ter sido executado. Dois deles falharam de um jeito
> que eu não previa.

---

## Cenário 1 · A fonte saiu do ar

**O que eu testei:** duas coisas, uma de cada fonte do projeto.

1. **A API caiu sozinha, sem eu pedir.** Às 08:57 de 21/09/2026, ao rodar a
   checagem de meta a pedido, o FakeERP devolveu **HTTP 502** no `POST
   /auth/login` e também no `/v3/api-docs`, em três tentativas seguidas. Não foi
   teste encenado: o servidor caiu de verdade, e voltou ~3 minutos depois.
2. **Escondi o arquivo local.** `dados/amostra.csv` foi renomeado para
   `dados/amostra_OLD.csv`, e rodei o que gera os 3 números da fonte canônica.

**O que aconteceu:**

No caso da API, a análise parou e reportou o erro — mas por decisão minha na
hora, não porque alguma regra obrigasse. Não havia nada escrito dizendo o que
fazer, então o comportamento certo dependeu de sorte.

No caso do arquivo, o resultado foi pior e é o achado deste cenário: **o
`painel.html` continuou abrindo normalmente**. Ele traz os dados embutidos dentro
do próprio arquivo, então mostrou Janeiro/2026 com 2 pedidos pagos, **R$ 1.430,00
em verde** e ticket médio de R$ 715,00 — enquanto o subtítulo da página dizia, em
letras claras, *"calculados a partir de `dados/amostra.csv`"*, um arquivo que
**não existia mais**. O rodapé repetia a mesma origem falsa. O único indício de
que algo estava velho era a data "14/09/2026" no fim da página, onde ninguém
olha.

Nenhum erro, nenhum aviso, nenhuma célula vazia. Um painel com cara de saudável
citando uma fonte inexistente.

**O que eu consertei:** criei a **Regra 0 — Fonte indisponível** em
[regras.md](regras.md), que roda antes de todas as outras. Se a API não
responder (qualquer HTTP diferente de 200, incluindo o 502 observado) ou se
`dados/amostra.csv` não existir, estiver vazio ou ilegível, a resposta é
**"fonte indisponível"** e a análise para ali — sem avaliar outras regras, sem
escrever no Notion, sem gerar painel. A regra proíbe nominalmente os atalhos
tentadores: estimar, arredondar, reaproveitar número de execução anterior e
**usar arquivo `_OLD`**.

**Antes e depois, com a fonte ausente:**

| | Resposta |
|---|---|
| **Antes** | Painel abre normal: "2 pedidos pagos · R$ 1.430,00 · ticket médio R$ 715,00", citando `dados/amostra.csv` como origem |
| **Depois** | `FONTE INDISPONÍVEL: não encontrei dados/amostra.csv. Análise interrompida. Não vou estimar nada.` |

**Pendência que este teste deixou:** o `painel.html` continua com os dados
embutidos e sem carimbo de validade. A Regra 0 protege quem pede análise nova,
mas não protege quem abre o arquivo HTML com dois cliques. Falta o painel
declarar a data do dado que está mostrando, de forma visível.

---

## Cenário 2 · Chegou dado inesperado

**O que eu testei:** devolvi o `dados/amostra.csv` ao lugar e estraguei 3 das 12
linhas de pedido, uma de cada tipo:

| Linha | Avaria |
|---|---|
| Pedido 1005 | apagada — virou linha em branco |
| Pedido 1006 | `total` negativo: `-199.90` em vez de `199.90` |
| Pedido 1009 | data em `19/03/2026 19:55` em vez de `2026-03-19T19:55:00` |

Em seguida rodei o cálculo dos 3 números de [dados/fonte.md](dados/fonte.md) sem
nenhuma checagem — do jeito que uma rotina faz quando ninguém mandou conferir.

**O que aconteceu:** saiu um relatório completo, formatado, sem um único aviso.

| Período | Verdade | Relatório entregue |
|---|---|---|
| Janeiro/2026 | 2 pedidos · R$ 1.430,00 | 2 · R$ 1.430,00 ✓ |
| Fevereiro/2026 | 3 pedidos · R$ 3.149,90 | 2 · **R$ 2.050,10** |
| Março/2026 | 1 pedido · R$ 800,00 | **sumiu da tabela** |
| `19/03/2` | não existe | **1 · R$ 800,00** |
| Julho/2026 | 2 pedidos · R$ 4.300,00 | 2 · R$ 4.300,00 ✓ |

Três estragos distintos, todos silenciosos:

1. **Fevereiro perdeu R$ 1.099,80 (−35%)** — o pedido 1005 sumiu junto com a
   linha, e o 1006 entrou subtraindo em vez de somar.
2. **Março desapareceu** e no lugar nasceu um período chamado **"19/03/2"** —
   a data quebrada foi fatiada como se fosse ISO, e a rotina **inventou um mês**
   em vez de reclamar.
3. **A decisão da Regra 1 virou ao contrário.** A Regra 1 dispara abaixo de
   R$ 3.000. Fevereiro de verdade fechou em R$ 3.149,90 e **não** deveria
   disparar; com o dado sujo deu R$ 2.050,10 e **dispararia**. Uma linha em
   branco e um sinal trocado transformam um mês que bateu a meta num alarme de
   "vendas abaixo da meta" na página do Notion.

**O que eu consertei:** em duas etapas — a primeira não foi suficiente.

Acrescentei à Regra 0 a obrigação de **dizer quantas linhas leu, quantas ignorou
e por quê**, com a tabela dos tipos de avaria e a proibição de criar período que
não existe. Rodei de novo. Resultado: o valor negativo e a data quebrada foram
detectados e nomeados, e o período fantasma "19/03/2" sumiu do relatório.

**Mas a linha em branco passou batido.** O aviso dizia "li 11 linhas" — e o
arquivo tem 12 pedidos. O leitor de CSV descarta linha vazia *antes* de a
contagem acontecer, então o pedido 1005 continuava sumindo de fevereiro sem
ninguém avisar. A instrução "diga quantas linhas leu" não protege contra a linha
que nunca chegou a ser lida.

Fechei o buraco com uma segunda instrução: **conferir o número de linhas físicas
do arquivo contra o número de registros processados, e checar buraco na sequência
de `order_id`**. Aí o pedido 1005 é encontrado justamente pela ausência dele.

**Antes e depois, com as mesmas 3 linhas estragadas:**

| | Resposta |
|---|---|
| **Antes** | Tabela completa com Fevereiro = R$ 2.050,10 e um período "19/03/2". Nenhum aviso. |
| **Depois** | `BURACO na sequencia de order_id: [1005]` · `pedido 1006: valor negativo` · `pedido 1009: data fora do formato` → **mais de 20% dos pedidos não puderam ser lidos → FONTE INDISPONÍVEL. Análise interrompida. Nenhum número entregue.** |

*Precisão honesta:* a checagem contou 4 pedidos não contabilizados de 13, porque
a quebra de linha no fim do arquivo foi contada como linha em branco. O número
real é 3 de 12 (25%). Os dois passam do limite de 20%, então a conclusão não
muda — mas a contagem tem esse erro de ±1 e está registrado aqui em vez de
escondido.

---

## Cenário 3 · A condição nunca dispara

**O que eu testei:** perguntei qual das minhas regras nunca vai disparar, e fui
conferir no agendador em vez de confiar no que estava escrito.

**O que aconteceu:** achei coisa pior do que uma condição que não dispara.

O [automacoes.md](automacoes.md) declarava que a Regra 1 roda por uma rotina
agendada, **todo dia às 8h, dias 20 a 31**, e que toda checagem deixa uma linha
registrada mesmo quando está tudo bem. Ontem foi dia 20 e hoje é dia 21 — os
dois primeiros dias da janela.

**Não existe registro de execução de 20/09 nem de 21/09.** Nem no
`automacoes.md`, nem na página Alertas do Notion. A única checagem que aconteceu
hoje foi a que eu pedi à mão, às 08:55. O agendador que consigo consultar nesta
máquina não lista nenhuma tarefa cadastrada.

E a Regra 2 nunca teve agendamento nenhum — isso já estava registrado como
pendência no próprio `automacoes.md`, mas registrado como tarefa a fazer, não
como "esta regra não está no ar".

O ponto é o da aula, e aqui ele apareceu inteiro: **eu não tinha como distinguir
"a automação rodou e estava tudo bem" de "a automação não rodou"**. As duas
produzem exatamente o mesmo silêncio. Achei que a operação estava monitorada
desde 14/09. Não estava.

*Ressalva:* o agendador que enxergo nesta sessão pode não ser a mesma superfície
da rotina na nuvem linkada no `automacoes.md`. Mas a ausência de rastro nos dias
20 e 21 é fato verificável nos arquivos, e não depende dessa dúvida.

**O que eu consertei:** criei a seção **"Prova de vida — silêncio não é sinal de
OK"** em [regras.md](regras.md), com quatro obrigações:

- toda execução deixa linha, dispare ou não — `automacoes.md` é comprovante de
  vida, não arquivo de alarmes;
- **ausência de linha é falha, não é "estava tudo bem"** — o estado a assumir é
  "não sei";
- ao checar qualquer regra sob pedido, conferir antes se as execuções
  automáticas anteriores deixaram rastro, e dizer se faltou dia;
- regra sem agendamento não é regra automática, é documentação de intenção, e o
  `automacoes.md` tem que dizer isso na linha dela.

**Antes e depois:**

| | Resposta |
|---|---|
| **Antes** | Nenhum alarme desde 14/09 → parecia operação saudável e monitorada |
| **Depois** | A falta de linha nos dias 20 e 21 é lida como falha de execução e reportada junto com o resultado da checagem do dia |

**Pendência que este teste deixou:** a correção é de instrução, não de
infraestrutura. As Regras 1, 2 e 3 continuam **sem agendamento ativo** — hoje
rodam porque alguém pede. Agendar de verdade é o próximo passo, e está registrado
em [automacoes.md](automacoes.md).

---

## O que os três cenários têm em comum

Nos três, a operação **continuou respondendo com aparência normal**: um painel
verde citando arquivo inexistente, uma tabela completa com um mês inventado, e um
silêncio que parecia saúde. Em nenhum deles houve mensagem de erro, e em nenhum
deles eu teria percebido sozinho.

Os três consertos são frases em [regras.md](regras.md), não código. A capacidade
da ferramenta não mudou — mudou a obrigação de avisar.

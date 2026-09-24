# Pronto para escalar?

Avaliado em 24/09/2026 (Aula 15).

> O portão de cinco perguntas aplicado à operação deste repositório: as regras
> de [regras.md](regras.md), o log de [automacoes.md](automacoes.md), os testes
> de [testes.md](testes.md), a fonte [dados/fonte.md](dados/fonte.md) e a
> métrica de [problema.md](problema.md). Critério: evidência é arquivo e data.
> Onde não há arquivo que sustente, está escrito "sem evidência".

**Ressalva que vale para o documento inteiro:** boa parte da operação avaliada
roda sobre **dado de treino**. As Regras 1 e 2 olham o FakeERP (loja fictícia
das Aulas 10–12), não a boutique. A Regra 3 é a única sobre o negócio real, e a
fonte dela (`dados/clientes.md`) ainda não existe, porque a boutique não tem
cliente pagante (`contexto/negocio.md`). Um "sim" sobre a operação de treino
prova a mecânica, não o negócio.

---

## 1. Roda sem mim?

**Resposta:** não

**Evidência:** [automacoes.md](automacoes.md), seção "Regras ligadas", corrigida
em 21/09/2026: *"Como roda hoje: à mão, sob pedido. Nenhuma regra tem
agendamento ativo."* Todas as linhas da tabela de execuções (14/09 e 21/09)
foram rodadas a pedido. O cenário 3 de [testes.md](testes.md) confirma: não
existe registro de execução em 20/09 nem em 21/09, os dois primeiros dias da
janela em que a Regra 1 deveria ter rodado sozinha.

**Se não: o que falta:** agendar as Regras 1, 2 e 3 e ter ao menos uma linha
no `automacoes.md` gravada por uma execução que ninguém pediu. Já registrado
como pendência em `automacoes.md`.

## 2. Quando falha, avisa?

**Resposta:** em parte

**Evidência:** [testes.md](testes.md), 21/09/2026. Quebrei a operação de
propósito e ela acusou:
- **Cenário 1** (fonte fora do ar): depois da Regra 0, a resposta com
  `dados/amostra.csv` ausente é `FONTE INDISPONÍVEL ... Não vou estimar nada.`
- **Cenário 2** (dado inesperado): com 3 de 12 linhas avariadas, a resposta
  aponta `BURACO na sequencia de order_id: [1005]`, o valor negativo e a data
  quebrada, e para a análise.

**Se não: o que falta:**
- O aviso só existe **quando alguém roda**. Como nada roda sozinho (pergunta 1),
  uma falha entre duas execuções manuais não avisa ninguém.
- O `painel.html` continua abrindo com dado velho e sem data visível (pendência
  do cenário 1, aberta em `automacoes.md`).
- Mês sem nenhum pedido ainda vira alarme falso de "vendas abaixo da meta"
  (ponto cego da Regra 1, aberto em 21/09 em `automacoes.md`, correção proposta
  e não aplicada).
- O conserto do cenário 3 (seção "Prova de vida" de `regras.md`) é instrução
  escrita, ainda não testada contra uma falha nova.

## 3. Alguém lê a saída?

**Resposta:** não, sem evidência

**Evidência:** o campo "Quem recebe" de cada regra em [regras.md](regras.md)
nomeia uma pessoa: João, na página Alertas do Notion. É só isso. **Não existe
registro do que foi feito depois de nenhum alarme.** Os alarmes de 14/09
(janeiro/2026 e março/2026) e de 21/09 (setembro/2026) não têm ação anotada em
lugar nenhum do repositório. E não teriam: são sobre a loja de treino, e não há
decisão real a tomar em cima deles.

**Se não: o que falta:** para cada regra, escrever o que o João faz nos 10
minutos seguintes ao alarme e registrar ao menos uma vez que fez. Só faz
sentido de verdade quando a Regra 3 tiver cliente real para alarmar.

## 4. Mede alguma coisa?

**Resposta:** em parte

**Evidência:** a tabela Métrica / Alvo / Como confiro existe em
[problema.md](problema.md), seção "Métrica", definida em 21/09/2026: sete
métricas, todas com número e prazo.

**Se não: o que falta:** **nenhuma medição foi feita ainda.** O primeiro prazo
da tabela é 31/10/2026 (baseline do funil manual). Os números efetivamente
medidos no repositório (R$ 1.430,00, R$ 3.149,90, R$ 800,00) são da receita do
FakeERP e não pertencem à tabela de métricas da boutique. Vira "sim" com a
primeira linha da tabela medida e datada.

## 5. O cliente foi ouvido?

**Resposta:** não, sem evidência

**Evidência:** não há registro de conversa com ninguém que use ou usaria o que
a boutique faz.
- [contexto/negocio.md](contexto/negocio.md): "ainda sem clientes pagantes
  fechados".
- [contexto/cliente.md](contexto/cliente.md): perfil do comprador, dores e
  concorrência marcados como hipótese "ainda não testada com uma base real de
  clientes".
- [problema.md](problema.md), seção 10: a entrevista com os sócios da boutique
  consta como "tarefa pendente", e é o gargalo comum de seis seções.
- `radar/comentarios.md` e `radar/classificacao.md` não contam: são comentários
  públicos no perfil do BTG no Instagram, e não de quem usa o Radar ou os
  serviços da boutique.

**Se não: o que falta:** ver "A decisão".

---

## A decisão

**Não abre.**

Nenhuma das cinco perguntas tem um "sim" completo. As perguntas 1 a 4 mostram
que a mecânica existe e foi testada, mas sobre uma operação de treino e
dependendo de alguém apertar o botão. A pergunta 5 mostra algo mais sério:
ainda não se sabe se a operação resolve um problema que alguém de fato tem.
Escalar agora multiplicaria regras sobre uma loja fictícia e hipóteses de
cliente não testadas.

**O UM item que precisa virar "sim" primeiro: pergunta 5, o cliente foi
ouvido.**

Por que ele e não a pergunta 1, que é mais rápida: agendar as regras deixaria
a operação de treino rodando sozinha, mas não mudaria nada no negócio real. A
pergunta 5 destrava as outras: sem cliente não existe `dados/clientes.md`, sem
ele a Regra 3 não roda, e sem a Regra 3 não há saída real para alguém ler
(pergunta 3) nem métrica real para medir (pergunta 4).

- **O que fazer:** conversar com 3 pessoas que usam ou usariam o que a boutique
  faz. Sugestão de composição: 2 sócios da boutique/Grupo Sun (os usuários do
  Radar, o que também entrega a métrica de baseline do `problema.md`) e 1 dono
  de ISP do interior de SP (o cliente pagante dos serviços recorrentes).
- **O que registrar:** para cada conversa, data, quem é (papel, não dado
  pessoal), o que a pessoa disse que contradiz uma hipótese do
  `contexto/cliente.md` ou do `problema.md`, e **o que foi mudado no
  repositório por causa disso**. Não é pesquisa de satisfação, é descobrir o
  que foi entendido errado.
- **Prazo:** até a Aula 17, quinta-feira, 01/10/2026.
- **Como confiro:** 3 conversas registradas com data e ao menos uma mudança
  feita em `contexto/` ou `problema.md` que cite a conversa como motivo.

---

## Sexta pergunta: se eu dobrar o volume amanhã, o que quebra primeiro?

**A execução manual.** Hoje toda regra roda porque o João lembra de pedir. Com
o dobro de regras, de meses ou de clientes (quando a Regra 3 existir), o
primeiro sintoma seria dia sem linha no `automacoes.md`. Pela seção "Prova de
vida" de `regras.md`, cada dia sem linha conta como falha de execução. Em
seguida viria o `dados/clientes.md`, que também é mantido à mão: mais clientes
significam mais entregas para registrar, e entrega esquecida de registrar vira
alarme falso de "cliente sem entrega".

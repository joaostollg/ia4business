# ia4business — repositório de trabalho da boutique (Grupo Sun / ISPs)

Este repositório é a operação de uma **boutique de assessoria a provedores de
internet (ISPs) regionais do interior de São Paulo**, ligada ao Grupo Sun
Investimentos. A boutique vende serviços profissionais recorrentes — contábil,
tributário, jurídico e marketing — como porta de entrada, e assessoria de M&A
como objetivo final da relação; o cliente pagante é sempre o **dono do ISP**, o
lado vendedor. Aqui vivem as regras que a operação segue sozinha, a fonte
canônica dos números, o registro do que rodou e os documentos de análise do
projeto **Radar de Provedores**.

> Feito na disciplina **AI for Business** (Link School of Business). As Regras 1
> e 2 rodam sobre o FakeERP, um ERP de treino — a Regra 3 é a que mede o negócio
> real. Ver [regras.md](regras.md).

## Por onde começar

Na ordem em que alguém que nunca viu este repositório deveria ler:

| # | Arquivo | O que é |
|---|---|---|
| 1 | [contexto/negocio.md](contexto/negocio.md) | O que a boutique vende, para quem, como ganha dinheiro e o que ela **não** deve fazer por enquanto. Base de qualquer decisão de escopo. |
| 2 | [contexto/cliente.md](contexto/cliente.md) | Quem compra: fundador mais velho, decide sozinho. As três camadas de medo dele, e por que a concorrência real é a inércia. |
| 3 | [problema.md](problema.md) | O problema que o Radar de Provedores ataca, com lente de investidor. Contém a seção **## Métrica** — cada alvo com número e prazo. |
| 4 | [regras.md](regras.md) | As 4 regras que a operação segue sozinha, cada uma com gatilho, fonte, condição, ação e quem recebe. |
| 5 | [testes.md](testes.md) | Os 3 cenários de falha rodados de propósito em 21/09/2026, com o que quebrou e o que foi consertado. |
| 6 | [automacoes.md](automacoes.md) | O log: prova de que cada regra rodou — inclusive quando ficou calada porque estava tudo bem. |
| 7 | [painel.html](painel.html) | Dashboard de arquivo único com os 3 números do negócio de treino. Abre com dois cliques, offline. |

## Todos os arquivos

### Raiz

| Arquivo | O que é e para que serve |
|---|---|
| [README.md](README.md) | Este índice. O que faz o repositório sobreviver à memória de quem o escreveu. |
| [regras.md](regras.md) | As regras de decisão automática. Regra 0 (fonte indisponível, trava geral), Regra 1 (meta de vendas do mês), Regra 2 (queda de receita mês a mês), Regra 3 (cliente recorrente sem entrega) e a seção "Prova de vida". |
| [testes.md](testes.md) | Os três cenários que derrubam qualquer automação — fonte fora do ar, dado inesperado e condição que nunca dispara — testados de verdade, com antes e depois. |
| [automacoes.md](automacoes.md) | Log de execução das regras e de como o painel é atualizado. Registra também o que ficou pendente. |
| [problema.md](problema.md) | Diagnóstico do Radar de Provedores com lente de investidor: problema reescrito e hierarquizado, mercado, modelo de operação, métricas e dois panoramas de M&A em telecom. É o documento mais longo do repositório. |
| [painel.html](painel.html) | Painel de arquivo único gerado a partir de `dados/fonte.md` + `dados/amostra.csv`. Dados embutidos: abre sem internet e sem servidor. |
| [fake-erp.md](fake-erp.md) | O manual da porta do FakeERP: como autenticar, qual endpoint puxa o relatório do mês, quais meses têm dado e a pegadinha de somar pedidos cancelados como se fossem venda. |
| [prompt-ma-telecom-v1.md](prompt-ma-telecom-v1.md) | Primeira versão do pedido que gerou as seções de M&A do `problema.md`. Guardado para comparar com as versões seguintes. |
| [prompt-ma-telecom-v2.md](prompt-ma-telecom-v2.md) | O mesmo pedido reescrito no formato tarefa / formato / amostra / limite. |
| [prompt-ma-telecom-v3.md](prompt-ma-telecom-v3.md) | Versão vigente do prompt, refinada depois de uma crítica. **Ainda não foi executada** — é só o texto, para uso futuro. |
| [CLAUDE.md](CLAUDE.md) | Instruções que o Claude Code lê no início de toda sessão nesta pasta: o que ler antes de decidir, o que pode fazer sozinho e o que nunca deve commitar. |
| [SKILLS.md](SKILLS.md) | Tabela das skills instaladas, com o comando de cada uma e quando usar. |
| [tech-radar-provedores-spec.pdf](tech-radar-provedores-spec.pdf) | Spec técnico original do Radar de Provedores. É a fonte que o `problema.md` critica. |
| `.gitignore` | Lista do que não entra no Git: o dossiê pessoal e a configuração local do Claude Code. |

### `contexto/` — quem somos e para quem trabalhamos

| Arquivo | O que é e para que serve |
|---|---|
| [contexto/negocio.md](contexto/negocio.md) | O negócio: o que vende, modelo de receita, porte de ISP atendido, riscos assumidos e pontos ainda em aberto. |
| [contexto/cliente.md](contexto/cliente.md) | O cliente: quem compra, do que reclama, o maior medo em três camadas e o que causa churn. |
| [contexto/sobremim.md](contexto/sobremim.md) | Quem é o João e como ele prefere trabalhar. Cópia local para o repositório ser autocontido. |

### `dados/` — de onde vêm os números

| Arquivo | O que é e para que serve |
|---|---|
| [dados/fonte.md](dados/fonte.md) | A fonte canônica: onde o dado mora e a regra de cálculo dos 3 números (pedidos pagos, receita paga, ticket médio). Se uma conta divergir daqui, a conta está errada. |
| `dados/amostra.csv` | O dado bruto — 12 pedidos exportados do FakeERP. É o arquivo que alimenta o `painel.html` e o que foi quebrado de propósito nos testes. |

### `radar/` — coleta e classificação (Aula 13)

| Arquivo | O que é e para que serve |
|---|---|
| [radar/comentarios.md](radar/comentarios.md) | 31 comentários coletados do Instagram do @btgpactual em 17–18/09/2026. Matéria-prima bruta. |
| [radar/classificacao.md](radar/classificacao.md) | Os mesmos 31 comentários classificados por intenção, com o que ficou ambíguo marcado como não classificado em vez de forçado. |
| [radar/termometro-farialima.html](radar/termometro-farialima.html) | "Termômetro da Faria Lima" — página única com o diagnóstico de sentimento por trás da classificação. |

### `.claude/` — configuração da ferramenta

| Arquivo | O que é e para que serve |
|---|---|
| `.claude/skills/fecha-conversa/` | Skill que transforma notas cruas de reunião em ata estruturada e propõe o que atualizar em `contexto/`. |
| `.claude/skills/gera-briefing/` | Skill que monta o briefing antes de uma conversa importante, lendo `contexto/negocio.md` e `contexto/cliente.md`. |
| `.claude/skills/revisa-repo/` | Skill que passa o pente-fino no repositório antes de uma entrega: afirmação vaga, número sem fonte, dado desatualizado. |
| `.claude/skills/internal-comms/` | Skill de comunicação interna instalada do repositório oficial da Anthropic, com exemplos de formato. |
| `.claude/settings.local.json` | Configuração local da ferramenta. **Fora do Git** (`.gitignore`). |

### Fora do Git

| Arquivo | Por quê |
|---|---|
| `Hire_Capital_Dossie.pdf` | Documento pessoal/familiar. Listado no `.gitignore` e nunca commitado, mesmo em `git add .`. |

## Duas coisas que este README não resolve

1. **`prompt-ma-telecom-v3.md` nunca foi executado.** O arquivo existe e está
   pronto, mas nenhuma das seções do `problema.md` veio dele — vieram da v2. Não
   dá para saber, lendo o repositório, o que a v3 produziria.
2. **Nenhuma regra tem agendamento ativo.** As Regras 1, 2 e 3 estão escritas e
   funcionam quando alguém pede, mas hoje não rodam sozinhas — ver o cenário 3
   de [testes.md](testes.md) e as pendências de [automacoes.md](automacoes.md).

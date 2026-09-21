# CLAUDE.md — Repositório da boutique (Grupo Sun / ISPs)

> Lido automaticamente no início de toda sessão do Claude Code nesta pasta.
> Escopo: trabalhos da boutique de assessoria a provedores regionais (ISPs) do
> interior de SP. O projeto **Radar de Provedores** é a frente principal hoje,
> mas o repositório abriga outros trabalhos (M&A, panoramas, materiais comerciais).

## Leitura obrigatória no início da sessão

Antes de qualquer decisão de produto, negócio, escopo ou discurso, ler:

- `contexto/negocio.md` — o que a boutique vende, modelo de receita, cliente
  pagante (dono do ISP = lado vendedor), limites e pontos em aberto.
- `contexto/cliente.md` — perfil do comprador, as três camadas de medo,
  concorrência = "a inércia", o que causa churn.
- `contexto/sobremim.md` — quem é o João e como ele prefere trabalhar.

Pontos marcados "em aberto" nesses arquivos **não são decididos** — perguntar
antes de assumir resposta.

## Como trabalhar comigo (resumo — íntegra em `contexto/sobremim.md`)

- **Explicar jargão técnico antes de usar.** João é iniciante em
  programação/dados e básico em IA/LLMs. Não assumir conhecimento prévio de
  termos (API, scraping, token, RAG, git etc.); não infantilizar o raciocínio
  de negócio.
- **Perguntar antes de agir** em decisões de rumo. Depois de validado, pode
  executar sem reconfirmar cada micro-passo.
- **Enquete de múltipla escolha é o formato preferido** para alinhar decisões.
- **Tom:** informal e direto na conversa; formal e profissional em
  entregáveis/documentos finais.
- **Evitar:** resposta genérica de tutorial (pensar no caso específico) e
  complicar o que é simples.
- **Idioma:** português.

## Autonomia neste repositório

Pode fazer sem confirmar cada passo:
- Criar e editar arquivos em `contexto/` e rascunhos/documentos de trabalho.

Sempre confirmar antes:
- **Decisões estratégicas** (escopo, priorização, mudança de rumo) — trazer em
  enquete de múltipla escolha.
- **Qualquer coisa que "publique" ou saia do computador:** `git push`,
  `git commit` que o João não pediu, envio de conteúdo para serviços externos.

Nunca fazer:
- **Commitar arquivos pessoais/familiares** (ex: `Hire_Capital_Dossie.pdf`),
  mesmo se pedirem "commitar tudo". Conferir o `.gitignore` antes de qualquer
  `git add`.

## Mapa do repositório (abrir sob demanda)

- `README.md` — índice do repositório: o que é cada arquivo e cada pasta, na
  ordem em que alguém leria pela primeira vez. **Manter atualizado** quando
  arquivo novo entrar ou sair.
- `testes.md` — os 3 cenários de falha (fonte fora do ar, dado inesperado,
  condição que nunca dispara) rodados de propósito em 21/09/2026, com o que
  quebrou e a frase que consertou cada um.
- `problema.md` — diagnóstico do Radar com lente de
  investidor; reforça/critica a seção de "Problema" do spec. Contém a seção
  `## Métrica` (alvo com número e prazo).
- `tech-radar-provedores-spec.pdf` — spec técnico original do Radar de Provedores.
- `prompt-ma-telecom-v1.md` / `v2.md` / `v3.md` — iterações do prompt de
  análise de M&A em telecom (v3 é a versão vigente, ainda não executada).
- `SKILLS.md` — índice das skills instaladas (globais e deste projeto).
- `fake-erp.md` — contexto de acesso à API do FakeERP (ERP de treino das
  Aulas 10–12): autenticação, endpoints, erros comuns.
- `regras.md` — as 4 regras de automação (gatilho, fonte, condição, ação, quem
  recebe, se não disparar), mais a seção "Prova de vida". Regra 0 é trava geral
  de fonte indisponível; Regras 1 e 2 rodam sobre o FakeERP (treino); Regra 3 é
  a única sobre a operação real da boutique.
- `automacoes.md` — log de execução das regras + como o `painel.html` é
  atualizado.
- `dados/fonte.md` — fonte canônica do FakeERP: os 3 números do negócio de
  treino, com a regra de cálculo de cada um.
- `dados/amostra.csv` — dado bruto (pedidos do FakeERP) que alimenta o painel.
- `painel.html` — dashboard de arquivo único, gerado a partir de
  `dados/fonte.md` + `dados/amostra.csv`, abre offline.
- `Hire_Capital_Dossie.pdf` — pessoal, fora do git (`.gitignore`).

## Regras herdadas do projeto Radar de Provedores

- Cliente pagante = dono do ISP (vendedor). Não desenhar soluções pensando no
  comprador/consolidador.
- Ao sugerir escopo/funcionalidades, **priorizar por padrão a especialização em
  recorrência bem entregue** — o medo de "fazer tudo mal feito" é restrição de
  negócio real.
- Coleta de dados: nada de scraping atrás de login, listas vazadas, dados
  sensíveis ou contato pessoal privado do sócio. Abordagem sempre comercial,
  ligada à atividade da empresa-alvo, base no legítimo interesse (LGPD Art. 7º, IX).

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

- `problema-radar-provedores-analise.md` — diagnóstico do Radar com lente de
  investidor; reforça/critica a seção de "Problema" do spec.
- `tech 1.pdf` — spec técnico original do Radar de Provedores.
- `prompt V1.md` / `V2.md` / `V3.md` — iterações do prompt de análise de M&A
  em telecom (V3 é a versão vigente, ainda não executada).
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

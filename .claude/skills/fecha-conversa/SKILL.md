---
name: fecha-conversa
description: Transforma notas cruas de uma reunião ou call em ata estruturada, propõe a atualização da pasta contexto/ com o que mudou e prepara o commit. Use quando o João colar anotações de reunião ou disser "fecha essa conversa", "transforma isso em ata", "acabei a call com...". Toda ação sai com dono e prazo; decisões e pontos em aberto ficam separados. Não commita sozinho.
---

# fecha-conversa

Notas soltas de uma conversa viram um registro que serve daqui a seis meses: o que foi decidido, quem faz o quê até quando, e o que mudou no entendimento do negócio.

## Passo 1 — Receber as notas

O João cola as anotações (ou aponta um arquivo). Se não estiver claro, perguntar:

1. **Data** da conversa.
2. **Quem participou.**
3. **Assunto principal** em uma frase.

## Passo 2 — Escrever a ata

Formato:

```
# Ata — <assunto> (<data>)

**Participantes:** <nomes>
**Contexto:** <1 linha: por que essa conversa aconteceu>

## Decisões
- <decisão tomada, objetiva>

## Ações
| O quê | Dono | Prazo |
|---|---|---|
| <ação> | <nome> | <data> |

## Pontos em aberto
- <o que ficou sem resolver e precisa voltar>

## Próxima conversa
<data / gatilho, ou "não definida">
```

**Régua das ações:** toda ação tem **dono** e **prazo**. Se a nota não disser, escrever `❓ sem dono` / `❓ sem prazo` e listar isso nos pontos em aberto — **nunca inventar** um nome ou uma data.

## Passo 3 — Propor atualização do contexto/

Comparar o que foi dito com `contexto/negocio.md` e `contexto/cliente.md`:

- Uma hipótese marcada como "a validar" foi confirmada ou derrubada na conversa? → propor editar a seção correspondente.
- Surgiu um limite novo, um ponto em aberto novo, uma mudança de rumo? → propor onde entra.
- **Mostrar o trecho antes e depois. Não aplicar a edição sem o "ok" do João** (regra do CLAUDE.md: mudança de rumo se confirma antes).

Se nada no contexto/ muda, dizer isso explicitamente.

## Passo 4 — Preparar o commit

- Salvar a ata em `atas/AAAA-MM-DD-<assunto-em-kebab-case>.md`.
- Conferir o `.gitignore` antes de qualquer `git add` (nunca versionar `Hire_Capital_Dossie.pdf` nem outro arquivo pessoal).
- Montar a mensagem de commit sugerida: `Ata: <assunto> (<data>)` — e, se o contexto/ mudou, um segundo commit `Atualiza contexto/ apos conversa <data>`.
- **Mostrar o que vai entrar e esperar o João mandar commitar.** Esta skill não roda `git commit` nem `git push` por conta própria.

## O que NÃO fazer

- Não inventar dono, prazo ou decisão que não está nas notas.
- Não editar `contexto/` sem aprovação.
- Não commitar nem dar push sozinho.

---
name: revisa-repo
description: Revisa este repositório antes de qualquer entrega — material que vai para cliente, sócio do Grupo Sun, professor, ou antes de um git push de conteúdo que sai do computador. Use quando o João pedir "revisa o repo", "confere antes de mandar", "isso está pronto pra entregar?", "passa o pente-fino". Aponta afirmação vaga, número sem fonte, dado desatualizado, hipótese tratada como fato, arquivo fora do padrão de nome e inconsistência com a pasta contexto/. Não corrige sozinho — lista os problemas do mais grave ao menor.
---

# revisa-repo

Revisão crítica do repositório antes de entregar qualquer coisa. O revisor é um leitor cético que não conhece a boutique e vai cobrar cada afirmação.

## Quando rodar

- Antes de mandar um documento para um dono de ISP, para o Grupo Sun ou para um professor.
- Antes de qualquer `git push` de material novo.
- Quando o João pedir revisão explícita.

## O que revisar — a régua

Percorrer todos os `.md` e materiais de trabalho do repositório (não os arquivos de `contexto/` em si, mas conferir tudo **contra** eles) e sinalizar:

1. **Afirmação vaga sem número, prazo ou fonte.**
   - "muitos provedores", "o mercado está aquecido", "grande parte" → exigir número ou faixa.
   - Toda estatística de mercado/M&A precisa de **fonte + ano** ao lado. Sem isso, é opinião.

2. **Hipótese tratada como fato.**
   - O `contexto/` marca várias coisas como "hipótese a validar" (faixa de porte ~3.000–30.000 acessos, perfil do comprador, concorrência = inércia). Se o material afirma isso como verdade estabelecida, sinalizar.
   - Estágio da boutique é **ideia/validando, sem cliente pagante** — qualquer texto que soe como "já atendemos" ou "nossos clientes" está errado.

3. **Inconsistência com `contexto/negocio.md` e `contexto/cliente.md`.**
   - Material que desenha solução pensando no **comprador/consolidador** em vez do **dono do ISP (vendedor)**.
   - Material que sugere atender ISP fora da faixa pequeno/médio.
   - Discurso de M&A como primeira oferta (a porta de entrada é **recorrência bem entregue**).
   - Discurso comercial que trata a objeção do dono só como "desconhecimento técnico de M&A" e ignora as **3 camadas de medo** (perder controle / desconfiança / "e agora?" pós-venda).
   - Texto que posiciona a boutique contra outra empresa — a concorrência é **a inércia**, não um concorrente.
   - Gestão patrimonial pós-venda tratada como "extra" e não como parte formal da oferta.

4. **Ponto "em aberto" do `contexto/` sendo tratado como decidido.**
   - Restrições legais (advocacia, CVM), validação da faixa de porte, etc. Se o material fecha uma questão que o `contexto/` deixou aberta de propósito, sinalizar.

5. **Limites de coleta de dados (LGPD).**
   - Qualquer menção a scraping atrás de login, listas vazadas, dados sensíveis ou contato pessoal privado do sócio → sinalizar como violação da regra do projeto.

6. **Arquivo fora do padrão de nome.**
   - Padrão: minúsculas, palavras ligadas por hífen (kebab-case), **sem espaço**, sem acento, versão como `-v2` no fim.
   - Exemplos atuais fora do padrão: `tech 1 .pdf` (espaço, número solto), `prompt V1.md` / `prompt V2.md` / `prompt V3.md` (espaço + maiúscula). Sugerir: `tech-radar-provedores-spec.pdf`, `prompt-ma-telecom-v3.md`.

7. **Documentos-âncora desatualizados.**
   - `CLAUDE.md`, `SKILLS.md` e `contexto/` refletem a realidade atual do repositório? Arquivo citado que não existe mais, seção que contradiz o estado de hoje → sinalizar.

## Como reportar

Lista única, **do mais grave ao menor**. Para cada item:

```
### [N] <título curto do problema>  — gravidade: alta / média / baixa
**Onde:** <arquivo>:<linha ou seção>
**O quê:** <o que está errado, citando o trecho>
**Correção sugerida:** <o que fazer>
```

Fechar com um resumo: quantos itens por gravidade, e a pergunta "quer que eu aplique alguma correção?".

## O que NÃO fazer

- Não corrigir nada sem o João pedir — esta skill **aponta**, não edita.
- Não commitar nada.
- Não inventar fonte para um número que está sem fonte — o correto é sinalizar que falta.
- Não revisar `Hire_Capital_Dossie.pdf` nem sugerir versioná-lo (é pessoal, fica fora do Git).

# Radar de Provedores — Diagnóstico de Investidor + Problema Reescrito

> Análise feita sobre o documento `tech 1.pdf` ("Radar de Provedores — Ferramenta de IA para prospecção e qualificação de ISPs regionais", Grupo Sun Investimentos). Foco: entender e fortalecer 100% a seção de **Problema**, com a lente de quem vai decidir se aporta capital/tempo nisso.

---

## 1. Diagnóstico geral (o que um investidor vê primeiro)

**Pontos fortes do documento atual:**
- A Seção 3 ("Onde está a IA") é rara: você separa honestamente o que é *engenharia de dados* do que é *IA de verdade*. A maioria dos trabalhos finge que tudo é IA. Isso constrói credibilidade.
- Camadas de dados (Anatel + Receita Federal antes de "raspar a internet inteira") mostram disciplina — é uma escolha defensável tanto tecnicamente quanto legalmente (Seção 7, LGPD).
- A tabela de ICP com selo de confiança (`Certeza` / `Provável` / `Chute`) é ótima higiene epistêmica — poucos trabalhos admitem "isso é um chute a calibrar".

**O problema estrutural que um investidor vai pegar em 30 segundos:**
A Seção 2 ("Problema que resolve") e a Seção 3 ("Onde está a IA") **se contradizem**. A Seção 2 lista 4 problemas como se tivessem o mesmo peso — fragmentação, volume, qualificação subjetiva, timing. Mas a Seção 3 já confessa: "a coleta e o cruzamento são, em boa parte, engenharia de dados." Ou seja: **metade dos problemas listados na Seção 2 não são o problema que a IA resolve — são o pré-requisito para chegar no problema que a IA resolve.**

Isso importa porque é a primeira pergunta que qualquer investidor/banca vai fazer: *"por que isso não é só um CRM com filtro de CNAE?"* — e hoje o documento não tem uma resposta direta, porque a seção de problema não está hierarquizada por dificuldade/defensabilidade.

---

## 2. As perguntas que você precisa conseguir responder de cabeça

Antes de reescrever qualquer slide, teste se você responde isso sem gaguejar:

1. **Quantifica a dor**: hoje, quantas horas por mês um sócio/analista da boutique gasta prospectando manualmente? De cada 10 abordagens, quantas viram reunião? Quantas viram mandato?
2. **Quantifica o universo**: segundo a Anatel (SCM), quantas prestadoras de pequeno/médio porte existem hoje no interior de SP? (Isso é um dado público que dá pra puxar agora, mesmo sem entrevistar ninguém.)
3. **Por que ninguém resolveu isso ainda?** Existem bases comerciais no Brasil — Econodata, Speedio, Cortex Intelligence, Data Stone — que já cruzam CNPJ + CNAE + porte para o país inteiro. O que elas fazem que o Radar não precisa refazer, e o que elas **não** fazem que é exatamente o seu fosso?
4. **É remédio ou vitamina?** Sem a ferramenta, a boutique deixa de fechar quantos mandatos por ano — ou só demora mais para fechar o mesmo número?
5. **O timing é mesmo o gargalo?** O próprio documento menciona (Seção 1) que provedores "têm receio de vender por desconhecerem M&A". Isso sugere que parte do problema não é *achar quem já está maduro*, é *educar quem ainda não sabe que está maduro*. Isso muda o produto — hoje o ICP não captura essa dimensão.

Se qualquer uma dessas não tem resposta, é onde vale gastar a próxima semana — não em código, em levantamento de dado/entrevista.

---

## 3. Seção "Problema" — reescrita

*(pronta para substituir a Seção 2 do documento original; mantém o estilo e os selos de confiança)*

### 2.1 O problema raiz, em uma frase

> Entre centenas de provedores regionais de internet no interior de SP, a boutique não consegue saber — sem meses de trabalho manual e relacional — **quais poucas dezenas** têm ao mesmo tempo o perfil estratégico certo *e* uma janela real de disposição para vender ou buscar assessoria agora. O resultado: tempo de sócio é gasto em conversas de baixa conversão, enquanto candidatas com timing melhor passam despercebidas.

### 2.2 Os quatro problemas, ordenados por dificuldade real — não por ordem de pipeline

O documento original lista os problemas na ordem em que aparecem no pipeline de dados. Isso esconde qual deles é o fosso competitivo. Ordenando por defensabilidade:

| # | Problema | É resolvido por | Confiança |
|---|----------|------------------|-----------|
| 1 | **Fragmentação & Volume** — quem é provedor, de que porte, onde atua, está espalhado entre Anatel/Receita/web, sem chave comum; são centenas de candidatas, inviável revisar na mão | Engenharia de dados (não é IA, não é exclusivo do projeto — bases comerciais como Econodata/Speedio já fazem isso para o Brasil inteiro) | Certeza |
| 2 | **Qualificação subjetiva** — mesmo com a lista completa, decidir quem vale abordagem depende de sinais qualitativos (maturidade digital, estrutura societária, tom de marketing) que hoje só existem no feeling de quem tem repertório | IA (LLM-como-juiz) — genuinamente difícil de terceirizar ou comprar pronto | Provável |
| 3 | **Timing / propensão à venda** — o dado mais valioso ("este dono está numa janela de abertura para vender ou buscar ajuda agora") nunca vem pronto em nenhuma base; exige inferência sobre texto livre (notícias, mudança societária, pressão competitiva) | IA (NLP/detecção de sinais) — o problema mais difícil e o de maior valor por acerto | Provável |
| 4 | **Educação vs. descoberta** *(novo — hoje perdido dentro da Seção 1 "Contexto")* — parte dos candidatos qualificados não sabe que está pronta para vender/buscar ajuda, por desconhecer M&A e gestão patrimonial. Isso não é o mesmo problema que "achar quem já decidiu vender" — é um segundo eixo (educar vs. converter) que muda a abordagem comercial e deveria aparecer como critério explícito no ICP, não ficar implícito no contexto | Combinação de IA (identificar o padrão) + processo comercial (não é só produto) | Chute — a validar |

**Por que essa reordenação importa:** o problema #1 é o que justifica a *arquitetura* do produto (por que usar Anatel + Receita, por que não raspar tudo). Os problemas #2, #3 e #4 são o que justifica a *existência* do produto — são eles que respondem "por que isso não é só um banco de dados com filtro". Um investidor/banca vai te cobrar exatamente essa distinção; hoje o documento não a faz de forma explícita.

### 2.3 Por que ninguém resolveu isso ainda (a pergunta que todo investidor faz)

Bases genéricas de prospecção B2B otimizam para cobertura nacional e cadastro — não para julgamento fino de "está maduro para vender" dentro de um nicho vertical específico (ISPs regionais). Não existe incentivo econômico para uma empresa horizontal construir uma rubrica de qualificação tão específica para um universo pequeno (algumas centenas de empresas). É exatamente o tipo de nicho onde uma ferramenta vertical e sob medida vale mais do que uma ferramenta genérica comprada de prateleira — e onde IA (que barateia construir "julgamento customizado" em escala) muda a equação de custo que antes inviabilizava isso.

### 2.4 O que ainda falta para "entender o problema 100%" — hipóteses a validar

Marcado como `[Chute — validar]` no espírito do restante do documento:

- **Tamanho real do universo**: puxar o dado público da Anatel (SCM) hoje mesmo para o interior de SP e contar quantas prestadoras caem na faixa "3.000–30.000 acessos". Isso é factível sem esperar nada — é o número que sustenta toda a tese de volume.
- **Custo do processo manual hoje**: entrevistar 1–2 sócios da boutique — quantas horas/mês em prospecção, taxa de conversão abordagem→reunião→mandato.
- **Benchmark de concorrência indireta**: testar Econodata/Speedio/Cortex Intelligence com o mesmo filtro (CNAE de telecom + porte) e documentar explicitamente o que elas entregam vs. o que faltaria para qualificação — isso vira munição direta para a seção de diferenciação.
- **Validar a hipótese de "educação vs. conversão"**: nas primeiras conversas reais com donos de ISP, registrar se a objeção predominante é "não sei o que é M&A" (educação) ou "não tenho vontade/motivo pra vender" (timing) — são problemas diferentes e pedem produto/abordagem diferentes.

---

## 4. Próximo passo

Essa seção de problema agora tem hierarquia e uma resposta pronta para "por que isso não é só um CRM com filtro". Quando você tiver pelo menos o número real de ISPs via Anatel e uma estimativa de horas gastas hoje, dá para evoluir isso para as próximas seções do business plan (mercado, modelo de operação, métricas de sucesso) — é só avisar.

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

## 4. Seção "Mercado" — primeiro rascunho (24/08/2026)

> Objetivo desta seção: responder "que tamanho tem esse mercado" para quem vai decidir se aporta capital/tempo. Tentamos primeiro puxar o dado direto da Anatel (acessos SCM por prestadora e por município); não foi possível — ver 4.3. O que segue é a melhor estimativa possível com dado público indireto, com os cálculos expostos para poder ser refeita ou corrigida depois.

### 4.1 O que é dado direto (Certeza)

| Métrica | Número | Fonte / data |
|---|---|---|
| Provedores SCM com outorga formalizada no Brasil | **18.805** | Anatel, jan/2026 |
| Provedores ainda com autorização em análise | 420 | mesma fonte |
| Provedores excluídos do cadastro por não regularizar até o prazo | 5.085 | Anatel, 29/10/2025 |
| Atos de outorga publicados em 2025 (10x o ritmo de 2024) | 6.401 | Anatel/imprensa do setor, 2025 |

**Contexto por trás desse número:** a Anatel passou 2024–2025 exigindo que todo provedor de banda larga fixa (mesmo os pequenos, antes dispensados) tivesse outorga formal — prazo final 29/10/2025. Isso é uma coincidência favorável: o universo de "18.805 prestadoras" hoje é, em boa parte, exatamente o público-alvo do Radar (a nova exigência pegou sobretudo os pequenos/regionais que antes operavam sem outorga formal).

### 4.2 Estimativa para o interior de SP (Chute — método exposto)

Não existe, em nenhuma fonte pública testada, uma quebra desse total por estado ou por município. Para estimar a fatia do interior de SP, usamos proporção de municípios como proxy (na ausência de melhor dado):

- Brasil: **5.569 municípios** (IBGE, 2024)
- Estado de SP: **645 municípios**, dos quais **39** compõem a Região Metropolitana de SP (RMSP, inclui a capital)
- Interior de SP (excluindo capital e RMSP): **606 municípios** → **10,9%** dos municípios do Brasil

Aplicando essa proporção ao universo nacional:

> 18.805 × 10,9% ≈ **2.050 prestadoras SCM candidatas no interior de SP** (todas as faixas de porte, sem filtro de ICP)

**Por que isso é só um chute, não um número confiável:**
- Assume que provedores se distribuem uniformemente por município — mas SP é mais desenvolvido/conectado que a média do país, o que pode significar *mais* concorrência de grandes teles por município (menos espaço pra regional) ou *mais* provedores locais historicamente consolidados (mais fragmentação). As duas hipóteses puxam em direções opostas e não temos como saber qual pesa mais sem o dado real.
- Não filtra por porte. O número de ~2.050 inclui desde um provedor de 200 acessos até um de 200.000 — a faixa "doce" de 3.000–30.000 acessos (também um chute, ver [ICP no documento original](tech%201%20.pdf)) é uma fatia menor e desconhecida desse total.

### 4.3 O que tentamos e não deu certo (documentar para não repetir)

- **Painel oficial da Anatel** (`informacoes.anatel.gov.br/paineis/acessos/banda-larga-fixa`) tem o dado por prestadora/município, mas só existe dentro de um dashboard interativo (Qlik Sense) — não é um CSV baixável, e a automação de navegador não conseguiu carregar o dashboard (trava na tela de cookies, o motor de dados retorna erro 401).
- **Portal de Dados Abertos (dados.gov.br)** — o catálogo da Anatel lá não inclui esse dataset especificamente.
- **Base dos Dados (basedosdados.org)** — tem o dataset, mas só acessível via BigQuery/SQL, não é download direto.
- Alternativa viável não testada ainda: **acessar o painel manualmente** (você, no navegador, com login se precisar) e aplicar os filtros de município/porte na interface — o dashboard provavelmente funciona para uso humano normal, o obstáculo foi só a automação.

### 4.4 Para fechar essa seção com número real (não chute)

Duas rotas, não excludentes:
1. **Abrir o painel da Anatel manualmente** e filtrar por UF=SP, excluindo capital/RMSP, exportando a tabela por prestadora/porte — dá o número real de uma vez.
2. **Comparar com uma fonte comercial** (Econodata, Speedio) — que já cruzam CNPJ+CNAE+porte — como checagem cruzada da estimativa de ~2.050.

---

## 5. Seção "Modelo de Operação" — primeiro rascunho

> Objetivo: descrever como o Radar se encaixa no dia a dia da boutique — de lista ranqueada até mandato fechado. Diferente da seção de Mercado, aqui não há levantamento externo pendente: é desenho de processo, apoiado no que já está registrado em [negocio.md](contexto/negocio.md) e [cliente.md](contexto/cliente.md).

### 5.1 Onde o Radar entra no funil

| Etapa | O que acontece | Quem decide | Confiança |
|---|---|---|---|
| 1. Pipeline roda | Coleta → enriquecimento → cruzamento → qualificação (seção 6 do doc original) gera a lista ranqueada com nota 0–100 e justificativa | Automático | Certeza (já desenhado) |
| 2. Sócio/analista revisa | Lê a justificativa de cada nota alta — a nota não decide sozinha quem é abordado | Sócio/analista da boutique | Provável |
| 3. Classifica por eixo "educação vs. timing" | Candidata pronta pra conversa direta (timing) vs. candidata que precisa de conteúdo/relacionamento antes (educação) — ver problema #4 na seção 2.2 | Sócio/analista, com apoio da nota de IA | Chute — o critério existe na tese, ainda não no produto |
| 4. Abordagem comercial | **Sempre pela porta de entrada dos serviços recorrentes** (contábil/jurídico/marketing) — nunca abrindo com M&A, mesmo quando o score de timing é alto (ver risco registrado em negocio.md: não assumir muitos serviços, construir confiança primeiro) | Sócio/analista | Certeza (é decisão de negócio já tomada) |
| 5. Relação recorrente amadurece | Entrega de serviço com qualidade visível — é o que sustenta a relação (ver cliente.md: falta de entrega percebida = churn) | Time da boutique | Certeza (registrado em cliente.md) |
| 6. Conversa de M&A | Só quando o dono sinaliza maturidade — não é gatilho automático do Radar | Sócio | Certeza (decisão de negócio já tomada) |

**Ponto central:** o Radar prioriza *quem* abordar e ajuda a decidir *com que discurso* (educação vs. conversão), mas não pula a estratégia comercial já definida — a nota alta de "timing de venda" não vira abordagem de M&A direta, vira prioridade na fila de prospecção dos serviços recorrentes.

### 5.2 Cadência

- Dado da Anatel tem **defasagem mensal** (já registrado na seção 8 do documento original, "Limitações a assumir") — não faz sentido rodar o pipeline com frequência maior que isso.
- **Proposta (Chute — validar com uso real):** pipeline completo roda **mensalmente**; enriquecimento via LLM (sites/notícias/redes) pode rodar com mais frequência sobre a fila já qualificada, para captar sinais de timing (mudança societária, notícia de expansão) entre uma atualização da Anatel e outra.

### 5.3 Ciclo de calibração (o motor de qualificação não nasce pronto)

A seção 5 do documento original já assume isso: "a faixa de porte e os pesos são hipóteses a calibrar contra uma amostra de operadoras conhecidas — não verdades prontas." Isso implica um ciclo de feedback que ainda não está desenhado:

1. Abordagens acontecem (reunião marcada ou não, mandato fechado ou não).
2. Esse resultado devia voltar como dado de calibração — provedores com nota alta que não converteram, ou nota baixa que converteu, recalibram os pesos do scoring.
3. **Isso hoje não existe no pipeline** (seção 6 do documento original só vai até "ordena a fila para o CRM", sem etapa de volta). É um gap de produto, não só de dado.

`[Chute — a desenhar]`: sem esse loop, o motor de qualificação nunca melhora sozinho — fica sempre do tamanho do palpite inicial.

---

## 6. Seção "Métricas de Sucesso" — primeiro rascunho

> Duas camadas diferentes de métrica, que não podem ser misturadas: a **ferramenta** está funcionando (o scoring é bom?) é uma pergunta diferente de o **negócio** está indo bem (a boutique está fechando mais mandatos?). Um Radar tecnicamente ótimo não prova nada se a boutique não converte melhor — e o funil pode melhorar por motivos que não são o Radar.

### 6.1 Métricas da ferramenta (o scoring está certo?)

| Métrica | Como medir | Confiança |
|---|---|---|
| Precisão do score | % de candidatas com nota alta que, após revisão humana, o sócio concorda serem boas candidatas | Provável — mas precisa de amostra revisada manualmente para comparar |
| Cobertura | % de operadoras conhecidas pela boutique (relacionamento já existente) que o Radar também identificou e pontuou bem | Provável — é o teste mais direto de "o Radar não erra o óbvio" |
| Falso negativo grave | Candidatas que a boutique sabe que são boas mas o Radar pontuou baixo | Provável — mais importante que falso positivo, porque esconde oportunidade |

### 6.2 Métricas do funil comercial (o negócio está indo bem?)

| Métrica | Por quê importa | Confiança |
|---|---|---|
| Taxa abordagem → reunião | É a pergunta #1 do investidor (seção 2 deste doc) — hoje sem baseline | Certeza que importa / Chute o número |
| Taxa reunião → contrato recorrente | Mede se o discurso "porta de entrada" está funcionando | Chute — sem dado ainda |
| Tempo médio até contrato recorrente | Indica se o Radar acelera o ciclo, não só filtra melhor | Chute — sem dado ainda |
| % de clientes recorrentes que evoluem para conversa de M&A | É o objetivo final do modelo de negócio (negocio.md) — métrica de longo prazo | Chute — ainda não há histórico |
| Churn de recorrência | Já identificado em cliente.md como risco #1 — falta de entrega percebida | Certeza que é o risco / Chute o número |

### 6.3 O problema de fundo: não existe baseline

**Nenhuma dessas métricas de funil pode provar que o Radar ajudou** sem saber como era o funil *antes* dele. Isso é exatamente a lacuna já registrada na seção 2.4: "Custo do processo manual hoje — entrevistar 1–2 sócios da boutique: quantas horas/mês em prospecção, taxa de conversão abordagem→reunião→mandato." Enquanto isso não existe, qualquer métrica de "sucesso do Radar" compara com o vazio, não com o processo manual que ele substitui.

---

## 7. Panorama de M&A no setor — provedores do interior de SP (24/08/2026)

> Objetivo desta seção: mapear **quem já está comprando** provedores regionais no interior de SP, com que valores/múltiplos quando existem, e o que isso muda para a tese. Responde diretamente à pergunta #3 da seção 2 ("por que ninguém resolveu isso ainda") do ângulo "quem já está consolidando o mercado". Fonte: imprensa especializada em telecom (TELETIME, TeleSíntese, Mercado&Consumo, Exame Insight/BTG) — **não existe uma base única de deals**, o levantamento foi feito notícia por notícia, manualmente, e por isso é necessariamente incompleto (ver 7.5).

### 7.1 Transações confirmadas no interior de SP, 2023–2026

| Data | Comprador | Alvo | Onde (interior SP) | Clientes do alvo | Valor | Múltiplo |
|---|---|---|---|---|---|---|
| Ago–Out/2023 | Alares | Webby Internet | Ourinhos e região (+ PR) | 115 mil | R$ 206 mi | não divulgado |
| Jul–Set/2024 | Alares | Azza Telecom | Sorocaba + 48 cidades SP | 133 mil | R$ 188 mi | não divulgado |
| Mai/2024 | Master Internet | 4 provedores (não nomeados) | Vale do Paraíba (Tremembé, São José dos Campos, Taubaté) | ~15 mil (soma) | não divulgado | custo ~R$1,8–2 mil/cliente (não é múltiplo de EBITDA, é custo de aquisição por assinante) |
| Nov/2025 | Alares | IPNet Telecom | Região metropolitana de Sorocaba (6 municípios) | 25 mil | não divulgado | não divulgado |
| Mai/2026 | Avança Participações (fusão Altarede+K2, dez/2025) | Provedor não identificado | Vale do Paraíba | +30 mil portas B2C | não divulgado | não divulgado |
| Jun–Ago/2026 | Alares | Oquei Telecom | José Bonifácio + São José do Rio Preto + 28 cidades | 68 mil | R$ 189 mi (R$75,6mi à vista + 10 parcelas semestrais) | Receita R$89,2mi / EBITDA R$35,5mi → **≈5,3x EBITDA** (calculado, não divulgado diretamente) |
| Mar/2026 | **Claro** (grande operadora nacional) | Desktop (maior ISP independente do interior de SP) | Interior de SP inteiro (~180 cidades) | ~1,2 milhão | EV R$ 4 bi (R$2,4bi em equity) | **6,1–6,2x EBITDA 2025** (divulgado) |

*Confiança: **Certeza** para a ocorrência e os valores citados (todos com fonte de imprensa); **Provável** para os múltiplos calculados por mim a partir de dados divulgados separadamente (caso Oquei).*

**Contexto de fundo (fora do recorte estrito interior-SP, mas relevante para a tese):** a fusão Vero+Americanet (anunciada ago/2023, R$1,7bi de receita combinada, 1,4mi+ assinantes em 450 cidades) confirma que a lógica de "consolidar municípios de médio/pequeno porte fora de capitais" é uma tese que outros grupos já executam em escala nacional — não é exclusividade do interior de SP.

### 7.2 O que não é público (limite real desta análise)

Como antecipado antes da pesquisa: **valor de transação e múltiplo só aparecem quando o comprador é obrigado a divulgar** (empresa listada em bolsa, caso Desktop/Claro) ou escolhe divulgar para fins de relações públicas/investidores (caso Alares, que tem fundo americano como controlador). Deals menores — os 4 provedores da Master Internet, o provedor da Avança no Vale do Paraíba — **não têm valor nem múltiplo público**, só o fato de terem acontecido.

Isso sugere um **viés de tamanho na cobertura de imprensa**: os dois múltiplos que conseguimos calcular (Oquei ≈5,3x EBITDA, Desktop ≈6,1-6,2x EBITDA) são de operações de R$189mi e R$4bi — ambas **acima** da faixa de porte do ICP do Radar (3.000–30.000 acessos, hipótese ainda a validar). Não há, nesta pesquisa, nenhum múltiplo confiável para uma transação do porte que a boutique pretende assessorar.

### 7.3 Quem está comprando — perfil dos consolidadores ativos no interior de SP

| Consolidador | Perfil | Ritmo | Observação |
|---|---|---|---|
| **Alares** | Controlada pelo fundo americano Grain Management (desde 2021) | Oquei é a **22ª aquisição desde 2015** — o mais ativo e recorrente da lista | ~887 mil clientes pós-Oquei; disputa a liderança de ISP independente em SP |
| **Claro** | Grande operadora nacional (não é "boutique" nem fundo) | Entrada recente e histórica no jogo — Desktop é a **primeira aquisição relevante de um operador regional por uma das 3 grandes teles desde 2014** | Muda o cenário competitivo: os concorrentes dos ISPs pequenos que sobram passam a incluir uma Claro fortalecida |
| **Master Internet** | Regional (Vale do Paraíba), consolidação hiperlocal | Aquisições concentradas na mesma região onde já opera | Lógica declarada: retorno mais rápido comprando onde já tem presença, não expandindo para praça nova |
| **Brasil TecPar** | Regional (zona metropolitana/limítrofe de SP — Guarulhos, Itaquaquecetuba) | 4 aquisições identificadas 2021–2024 | Fora do recorte estrito "interior", mas mesmo padrão de consolidação |
| **Avança Participações** (fusão Altarede+K2, dez/2025) | Nova entrante no interior de SP via Vale do Paraíba (mai/2026) | Meta declarada: 200 mil clientes até 2027 via aquisições de provedores pequenos/médios | Consolidador novo — vale acompanhar, é o mais alinhado ao porte do ICP do Radar |
| Unifique / (pré-venda) Desktop | Identificados pelo BTG (Exame Insight) como tendo "apetite e caixa" para comprar mais ISPs regionais | Unifique ainda concentrado em SC/RS, entra em SP via parceria (não M&A direto) | `[Chute — validar]` se viram compradores diretos no interior de SP |

### 7.4 O que isso muda para a tese do Radar

- **Confirma que a consolidação é real e ativa** — pelo menos 7 transações/movimentos identificados no interior de SP em 3 anos, não é só uma hipótese do documento original.
- **Mas o "alvo típico" visível na imprensa está acima do ICP do Radar.** As transações que viram notícia envolvem provedores de 25 mil a 1,2 milhão de clientes — a faixa de 3.000–30.000 acessos do ICP provavelmente vive numa **cauda longa de add-ons pequenos que nunca vira matéria** (como os "4 provedores" da Master Internet, nunca nomeados). Isso é uma boa notícia para a tese do Radar — sugere que existe um mercado invisível à imprensa, exatamente onde uma ferramenta de prospecção sistemática teria vantagem — mas também significa que **esta pesquisa não prova volume nem preço para o porte-alvo real**, só a existência de compradores ativos maiores.
- **Lista embrionária de "para quem vender"**: Alares, Master Internet, Brasil TecPar e Avança Participações são candidatos naturais de compradores para os mandatos que a boutique (representando o vendedor) poderia originar no interior de SP — isso não existia mapeado em nenhum lugar do documento antes.
- **A entrada da Claro via Desktop (mar/2026)** é o dado mais estrutural desta seção: pela primeira vez em mais de uma década uma das 3 grandes operadoras compra um operador regional relevante. Isso empurra em duas direções que reforçam a mesma conclusão: mais compradores de peso no jogo (janela de saída mais real para quem quer vender) e mais pressão competitiva sobre quem fica pequeno (motivo a mais para profissionalizar ou vender agora). Ambas alinhadas com a dor central já registrada em [cliente.md](contexto/cliente.md).

### 7.5 Pendências para fechar esse panorama

`[Chute — a validar]`:
- Histórico completo da Alares (documento cita "22ª operação desde 2015" — as ~15 aquisições anteriores a 2023 não foram levantadas aqui, ficaram fora do recorte "últimos 3 anos").
- Nomear os 4 provedores comprados pela Master Internet e o provedor comprado pela Avança Participações no Vale do Paraíba — ambos não identificados nas matérias-fonte.
- **A pesquisa mais valiosa a fazer a seguir**: em vez de imprensa, buscar a cauda de deals pequenos via mudança de quadro societário na Receita Federal/Juntas Comerciais — é provavelmente onde está o volume real de transações no porte do ICP do Radar, e a imprensa especializada não cobre.

---

## 9. Comparáveis Nacionais de M&A em Telecom — últimos 10 anos (24/08/2026)

> Gerada a partir do [prompt V2.md](prompt%20V2.md) — escopo mais amplo que a Seção 7 (Brasil inteiro, não só SP; 10 anos, não só 3; inclui infraestrutura de fibra/atacado, não só ISPs de varejo). Serve como **benchmark de mercado mais amplo**, não substitui a Seção 7 (que é o recorte direto para a tese do Radar). Metodologia igual à da Seção 7: levantamento manual em imprensa especializada, sem base de dados única — necessariamente incompleto.

### 9.1 Tabela de transações (2016–2026)

| Empresa adquirida | Comprador | Ano | Região | Valor | Clientes / HPs | Múltiplo | Racional estratégico | Fonte |
|---|---|---|---|---|---|---|---|---|
| 8 provedores regionais (formação da Vero) | Vinci Partners (private equity) | 2019 | Interior de MG | não divulgado | não divulgado | não divulgado | Roll-up: unificar provedores independentes do interior mineiro sob uma marca/gestão única | [Wikipédia](https://pt.wikipedia.org/wiki/Vero_Internet) |
| MKA Telecom + Clic Rápido | Vero | ago/2020 | Interior PR/SC | não divulgado | não divulgado | não divulgado | Expansão geográfica da Vero para a Região Sul | [TELETIME](https://teletime.com.br/04/02/2021/vero-internet-adquire-novo-provedor-com-atuacao-em-pr-e-sc/) |
| ISSO Internet e Telecomunicações | Desktop (com aporte da HIG Capital) | ago/2020 | Interior de SP | não divulgado | não divulgado | não divulgado | Início da fase de M&A da Desktop após aporte de PE, pré-IPO | [Nossa História - Desktop](https://www.ri.desktop.com.br/en/about-desktop/our-history/) |
| Netell Internet (participação) | Desktop | nov/2020 | Interior de SP | não divulgado | não divulgado | não divulgado | Continuidade da estratégia de roll-up pré-IPO | [Nossa História - Desktop](https://www.ri.desktop.com.br/en/about-desktop/our-history/) |
| Copel Telecom (privatização → Ligga) | Bordeaux Fundo de Investimento (Nelson Tanure) | nov/2020 (leilão) / 2021 (fechamento) | Paraná (capital + interior) | R$ 2,39 bi (leilão) / R$ 2,5 bi (fechamento) | não divulgado | não divulgado | Privatização de operadora estatal de telecom — não é M&A privado tradicional, mas é comparável de porte de infraestrutura regional | [Maringa.Com](https://noticias.maringa.com/31557/ligga-telecom-da-copel-sera-vendida-por-r-495-milhoes) / [Portal F&A](https://fusoesaquisicoes.com/acontece-no-setor/tanure-poe-ligga-telecom-a-venda/) |
| Netion Soluções (participação) | Desktop | mar/2021 | Interior de SP | não divulgado | não divulgado | não divulgado | Continuidade do roll-up pré-IPO | [Nossa História - Desktop](https://www.ri.desktop.com.br/en/about-desktop/our-history/) |
| Neofibra + grupo TKNet | Unifique | ago/2021 | Interior SC / RS | não divulgado | não divulgado | não divulgado | Consolidação regional no Sul, logo após IPO (jul/2021) | [ACATE](https://www.acate.com.br/noticias/catarinense-unifique-anuncia-aquisicoes/) |
| 3 provedores regionais | Unifique | out/2021 | Interior de SC | não divulgado | não divulgado | não divulgado | Mesma lógica de consolidação regional pós-IPO | [TELETIME](https://teletime.com.br/01/10/2021/unifique-compra-mais-tres-provedores-regionais-em-sc-veja-valores/) |
| Guaíba Telecom | Unifique | dez/2021 | Interior de RS | R$ 60,93 mi | não divulgado | não divulgado (EBITDA do alvo não publicado) | Expansão no RS | [ACATE](https://www.acate.com.br/noticias/unifique-anuncia-compra-de-tres-provedores-do-sul-sendo-dois-de-sc/) |
| Mosaico Telecom (Clinitec) | Unifique | jan/2022 | Interior de SC | R$ 14,42 mi | não divulgado | não divulgado | Expansão em SC | [ACATE](https://www.acate.com.br/noticias/unifique-anuncia-compra-de-tres-provedores-do-sul-sendo-dois-de-sc/) |
| Renovare Telecom | Vero | mai/2022 | Interior de RS | não divulgado | não divulgado | não divulgado | Continuidade da expansão da Vero no Sul | [TELETIME](https://teletime.com.br/20/05/2022/vero-adquire-provedor-regional-renovare-do-rio-grande-do-sul/) |
| Webby Internet | Alares | ago–out/2023 | Interior de SP (Ourinhos) + PR | R$ 206 mi | 115 mil | não divulgado | Expansão da Alares em SP | [TELETIME](https://teletime.com.br/23/08/2023/alares-anuncia-acordo-para-aquisicao-da-provedora-paulista-webby-internet/) |
| Americanet (fusão) | Vero | dez/2023 | Nacional (forte em SP) | não divulgado (fusão via troca de ações) | 1,4–1,5 milhão (base combinada) | não calculável (é fusão, não compra) | Cria o 2º maior ISP regional do país; sinergias estimadas em R$1 bi (VPN até 2030) | [TELETIME](https://teletime.com.br/28/03/2024/veroamericanet-soma-receita-de-r-16-bilhao-em-2023/) |
| Azza Telecom | Alares | jul–set/2024 | Interior de SP (Sorocaba + 48 cidades) | R$ 188 mi | 133 mil | não divulgado (EBITDA do alvo não publicado) | Alares vira 4º maior ISP de SP | [TELETIME](https://teletime.com.br/17/07/2024/alares-compra-provedor-paulista-azza-por-r-188-milhoes/) |
| 4 provedores não identificados | Master Internet | mai/2024 | Vale do Paraíba/SP | não divulgado | ~15 mil (soma) | custo ~R$1,8–2 mil/cliente (não é múltiplo EBITDA) | Consolidação hiperlocal — comprar onde já opera é mais barato que abrir praça nova | [TELETIME](https://teletime.com.br/13/05/2024/master-internet-adquire-quatro-provedores-no-interior-de-sao-paulo/) |
| CyberNet, Iveloz, DZ7 Internet | Brasil TecPar | 2023–2024 | Guarulhos/Itaquaquecetuba (SP) | não divulgado | 67 mil + 16 mil | não divulgado | Entrada e expansão B2C em SP | [TeleSíntese](https://www.telesintese.com.br/brasil-tecpar-adquire-grande-operacao-em-sao-paulo/) |
| Oi Fibra (base residencial) | V.tal | mar/2024–2025 | Nacional (inclui muitas cidades do interior) | R$ 5,6 bi | base nacional da Oi Fibra (não segmentada aqui) | não divulgado | V.tal absorve a operação de varejo de fibra da Oi, cria unidade "Nio" | [Poder360](https://www.poder360.com.br/economia/vendida-pela-oi-v-tal-vai-investir-r-30-bi-em-infraestrutura/) |
| IPNet Telecom | Alares | nov/2025 | Região metropolitana de Sorocaba/SP | não divulgado | 25 mil | não divulgado | Alares vira 3º maior ISP (PPP) de SP | [TELETIME](https://teletime.com.br/24/11/2025/alares-compra-ipnet-telecom-e-expande-atuacao-no-interior-de-sao-paulo/) |
| CCS Telecom, 3SNET, SerraNet | Unifique | nov/2025 | Interior SC/RS | R$ 87,3 mi (total: R$70,6mi + R$12,41mi + R$4mi) | 33 mil (soma) | não divulgado | Retomada do ritmo de aquisições da Unifique no Sul | [TELETIME](https://teletime.com.br/03/11/2025/unifique-anuncia-a-compra-de-tres-provedores-em-sc-e-rs/) |
| Altarede + K2 Telecom (fusão) | — (criam holding Avança Participações) | dez/2025 | RJ/ES/SP/MG | não divulgado | ~80 mil (base unificada) | não divulgado | Consolidação B2B/B2C entre dois regionais, meta de 200 mil clientes até 2027 via M&A | [TeleSíntese](https://telesintese.com.br/altarede-e-k2-telecom-anunciam-fusao/) |
| Um Telecom | V.tal | dez/2025 | Nordeste (200 cidades) | não divulgado | ~1 mil clientes corporativo/atacado; 20 mil km de fibra | não divulgado | Consolida liderança de fibra neutra no Nordeste | [TELETIME](https://teletime.com.br/29/12/2025/v-tal-adquire-a-um-telecom/) |
| FiBrasil (50%→100%, em 3 etapas) | Vivo (Telefônica) | jul/2025–2026 | Nacional, cidades médias fora de SP capital | R$ 850 mi + R$ 858 mi + R$ 458,7 mi (≈R$ 2,17 bi no total das 3 etapas) | 5,5 milhões de HPs (meta original da joint venture) | não divulgado | Vivo assume controle total da rede neutra de fibra criada com a CDPQ em 2021 | [TELETIME](https://teletime.com.br/10/07/2025/vivo-vai-adquirir-controle-da-fibrasil-por-r-850-milhoes) |
| Provedor não identificado | Avança Participações | mai/2026 | Vale do Paraíba/SP | não divulgado | +30 mil portas B2C | não divulgado | Entrada da Avança no mercado paulista | [TELETIME](https://teletime.com.br/04/05/2026/avanca-participacoes-compra-provedor-no-vale-do-paraiba-e-entra-no-mercado-paulista/) |
| Oquei Telecom | Alares | jun–ago/2026 | Interior de SP (José Bonifácio + 29 cidades) | R$ 189 mi | 68 mil | **≈5,3x EBITDA** (R$189mi / R$35,5mi EBITDA — calculado) | Alares consolida região noroeste paulista | [TELETIME](https://teletime.com.br/18/06/2026/alares-compra-oquei-telecom/) |
| **Desktop** | **Claro** | mar/2026 | Interior de SP inteiro (~180 cidades) | EV R$ 4 bi (R$2,4 bi em equity) | ~1,2 milhão | **6,1–6,2x EBITDA 2025** | 1ª aquisição de operador regional relevante por uma das 3 grandes teles desde 2014 | [XP](https://conteudos.xpi.com.br/acoes/relatorios/broadband-game-claro-compra-a-desktop-em-transacao-com-ev-de-r-40-bilhoes/) |

*Confiança: **Certeza** para datas/valores citados com fonte direta; **Provável**/calculado para os dois múltiplos derivados (Oquei, Desktop — ver metodologia na Seção 7.2). Linha da Ligga/Copel incluída como comparável de porte de infraestrutura regional, mas é privatização/leilão, não M&A de mercado privado — categoria diferente das demais.*

### 9.2 Padrões de valuation e consolidação (análise)

**Múltiplos — dado escasso, mas com um sinal consistente.** Dos ~24 movimentos listados, só **2** têm múltiplo de EBITDA calculável (Oquei ≈5,3x e Desktop 6,1–6,2x) — e o próprio material de BTG citado na Seção 7.1 menciona que a Desktop historicamente comprava a ~5,5x EBITDA antes de negociar a ~6,2x em bolsa. Isso sugere uma faixa de referência de **~5x a 6x EBITDA** para operações de porte médio-grande (R$150mi–R$4bi) — mas **nenhum dado aqui cobre o porte do ICP do Radar** (3.000–30.000 acessos, ordem de grandeza bem menor que qualquer linha desta tabela). Continua valendo o alerta da Seção 7.2: quem divulga valor e múltiplo é ou empresa listada (obrigação regulatória) ou comprador com investidor institucional que quer visibilidade — nenhuma dessas condições costuma valer para um deal pequeno.

**Três arquétipos de comprador, não um só:**
1. **Roll-ups regionais nascidos de private equity** (Vero/Vinci+Warburg Pincus, Desktop/HIG pré-2021) — compram provedor por provedor, cedo, sem holofote, para depois abrir capital.
2. **Consolidadores listados em bolsa comprando com o próprio caixa de IPO** (Unifique, Alares mesmo sem estar na B3 mas com fundo americano por trás) — compram com mais frequência e menor porte por transação (Guaíba R$61mi, Mosaico R$14mi), mantendo ritmo trimestral/semestral.
3. **Entrantes tardios de infraestrutura/grande tele**, comprando pouco, mas cada operação é ordens de grandeza maior — Claro/Desktop (R$4bi), V.tal/Oi Fibra (R$5,6bi), Vivo/FiBrasil (~R$2,2bi somando as 3 etapas). Isso é o movimento mais recente (2024–2026) e o mais estrutural: sinaliza que o topo do mercado está ficando mais concentrado bem na hora em que o meio do mercado (ISPs médios) também está se consolidando.

**Geografia — SP interior é o polo mais quente, Sul é o segundo.** Das transações com localização clara, a maioria concentra-se no interior de SP (Alares, Desktop, Master Internet, Brasil TecPar, Avança) — coerente com ser o estado mais populoso e fragmentado. O Sul (SC/RS) é o segundo polo mais ativo, quase todo protagonizado pela Unifique. O Nordeste aparece mais em infraestrutura de atacado (V.tal/Um Telecom) do que em M&A de ISP de varejo.

**Janela temporal — três fases visíveis:**
- **2016–2020**: formação dos roll-ups via aporte de PE (Vero via Vinci Partners, Desktop via HIG Capital) — silenciosa, poucos valores públicos.
- **2021**: pico de IPOs (Desktop, Brisanet, Unifique, todos na B3 em poucos dias de diferença) — capitalização para acelerar M&A.
- **2023–2026**: consolidação entre os próprios roll-ups (Vero+Americanet), entrada de grandes teles (Claro/Desktop) e fundos de infraestrutura pura (V.tal, Vivo/FiBrasil) — a fase atual, mais ativa e com cheques maiores.

**Padrão de pagamento notável:** a Alares estruturou a compra da Oquei com apenas 40% à vista (R$75,6mi de R$189mi) e o restante em 10 parcelas semestrais — sinaliza que mesmo consolidadores grandes e recorrentes gerenciam caixa com cuidado, o que é um dado útil para calibrar expectativa de "quanto e como" um dono de ISP médio pode esperar receber num deal real (nem sempre é dinheiro à vista integral).

---

## 10. Próximo passo

Essa seção de problema agora tem hierarquia e uma resposta pronta para "por que isso não é só um CRM com filtro". A seção de Mercado (4) tem um número nacional real e uma estimativa para SP com o método exposto. O Panorama de M&A (7) confirma, com transações reais no recorte direto do Radar (SP, 3 anos), que a consolidação já está em curso e começa a esboçar uma lista de "para quem vender". Os Comparáveis Nacionais (9) ampliam esse panorama para 10 anos e o Brasil inteiro, e mostram três fases de mercado (roll-up via PE → IPOs → consolidação entre grandes players) e uma faixa de referência de múltiplo (~5x–6x EBITDA) — mas ambos os levantamentos expõem a mesma lacuna: **os deals visíveis na imprensa são sempre maiores que o ICP-alvo do Radar**, então nenhum dá baseline real de preço/volume para o porte que a boutique pretende assessorar. Modelo de Operação (5) e Métricas de Sucesso (6) têm desenho de processo, mas dependem da mesma lacuna: **nenhuma delas tem baseline real** — o número de SP (4.2), a taxa de conversão hoje (6.3), a amostra de calibração do scoring (5.3) e o volume real de deals no porte do ICP (7.5, 9.2) apontam todos para a mesma tarefa pendente: entrevistar 1–2 sócios da boutique sobre como o processo funciona hoje. Esse é o gargalo comum para destravar as seis seções de uma vez — é só avisar quando quiser encarar essa conversa.

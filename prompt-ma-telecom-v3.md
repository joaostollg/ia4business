# Prompt V3

> Versão refinada do [prompt V2](prompt%20V2.md), depois de uma crítica de "revisor exigente" (o que estava ambíguo, o que faltava, o que sobrava). Ainda não foi executada — é só o texto do prompt reescrito, para uso futuro em [problema-radar-provedores-analise.md](problema-radar-provedores-analise.md).

**Data:** 24/08/2026

**TAREFA**
> Analise operações de M&A relevantes em telecom (ISPs de varejo, fibra óptica e infraestrutura de atacado/backbone) com alvo operando fora de capitais estaduais no Brasil, de 2016 até hoje. Inclua uma operação mesmo que também toque uma capital (ex: privatização estadual, fusão multi-região) — só não inclua deals 100% restritos a capital.

**FORMATO**
> - **Tabela 1 — ISPs de varejo/fibra:** empresa adquirida | comprador | ano | região | valor (R$) | clientes (priorizar; se só houver HPs, marcar "HPs") | múltiplo (especifique o tipo — EV/EBITDA, R$/cliente ou R$/HP — e marque `[calculado]` quando eu derivar de dois dados separados vs. `[divulgado]` quando a fonte já informa direto) | racional estratégico | fonte (veículo, data, link)
> - **Tabela 2 — infraestrutura de atacado/backbone:** mesmas colunas, tabela separada — o padrão de comprador e múltiplo é diferente.
> - Máximo 25 linhas por tabela; se houver mais candidatos, priorize os com valor/múltiplo público e os mais recentes.
> - Ao final: análise de padrões de valuation (múltiplo por faixa de porte, se os dados permitirem) e de consolidação (arquétipos de comprador, fases no tempo, concentração geográfica).

**AMOSTRA**
> Linha-modelo (formato-alvo, não é um deal real):
> `Provedor X | Grupo Y | 2024 | Interior de MG (12 cidades) | R$ 50 mi | 20 mil clientes | 5,0x EBITDA [calculado: R$50mi ÷ R$10mi EBITDA divulgado separadamente] | Consolidação regional, sinergia de rota | TELETIME, 12/03/2024, [link]`

**LIMITE**
> - Não invente valor, múltiplo ou número de clientes. Sem dado público, escreva "não divulgado" — nunca estime sem marcar como estimativa.
> - Toda linha precisa de veículo + data + link. Se duas fontes divergirem em valor, reporte as duas e sinalize o conflito — não escolha uma sozinho.
> - Sempre que nenhuma linha da tabela cair na faixa de 1.000–30.000 clientes, diga isso explicitamente na análise final — a ausência de dado público nessa faixa é, em si, um achado, não um vazio a esconder.
> - Ao comparar valores de anos distantes (ex: 2016 vs. 2026), avise que não estão ajustados por inflação.

---

### O que mudou da V2 para a V3 (crítica que motivou a reescrita)

**Ambíguo na V2:** definição de "interior", tipo de múltiplo não especificado, prioridade entre clientes/HPs não definida, deal anunciado vs. concluído não diferenciado, ISP de varejo e infraestrutura de atacado pedidos numa tabela só, início da janela de "10 anos" não datado.

**Faltava na V2:** critério de porte do alvo (o que puxava o resultado para os deals maiores/mais divulgados), instrução para dado conflitante entre fontes, teto de linhas na tabela, formato de citação de fonte, aviso sobre comparar valores de anos distantes sem ajuste de inflação.

**Sobrava na V2:** a AMOSTRA repetia a TAREFA em vez de dar um exemplo concreto de linha preenchida; o LIMITE misturava três regras de natureza diferente (anti-invenção, exigência de fonte, escopo geográfico) numa lista só.

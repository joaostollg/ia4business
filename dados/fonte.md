# Fonte canônica

> O lugar combinado onde o dado verdadeiro deste negócio de treino mora. Todo
> número que aparece em [painel.html](../painel.html) ou em qualquer regra de
> [regras.md](../regras.md) usa exatamente as definições daqui — se uma conta
> parecer diferente do que está escrito abaixo, a conta está errada, não este
> arquivo.

## Onde o dado vive

**FakeERP** — ERP de treino, documentado em [fake-erp.md](../fake-erp.md).
Caminho **C** (dado de treino) da Aula 12: não é uma planilha real do negócio,
é a base que as Aulas 10–12 usam para praticar o fluxo inteiro (API → fonte →
painel) sem depender de um sistema de verdade ainda não conectado.

- **Quem escreve nela:** o próprio FakeERP (é read-only para nós — só
  consultamos via API, não editamos pedidos).
- **Com que frequência:** os pedidos são fixos por mês de treino — não mudam
  sozinhos. O que muda é o mês que está sendo consultado (mês atual, ao vivo).
- **Como eu percebo que está desatualizada:** não se aplica da mesma forma que
  uma planilha manual — mas se um mês que deveria ter pedido vier vazio
  (`count: 0`) sem ser um dos meses sem dado documentados em `fake-erp.md`,
  é sinal de que a base de treino mudou e este arquivo precisa ser revisto.

## Os 3 números do negócio

Tipo de negócio: **loja/e-commerce** (o FakeERP modela pedidos com valor,
desconto e status — o mesmo padrão de uma loja).

### 1. Volume — Pedidos pagos no mês

**Regra de cálculo:** conta de pedidos do mês com `status == "PAID"` no
relatório `GET /report/{ano}/{mês}`. Pedidos `CANCELLED` e `PENDING` não
entram na contagem.

### 2. Dinheiro — Receita paga no mês

**Regra de cálculo:** soma do campo `total` de cada pedido do mês com
`status == "PAID"`. `CANCELLED` e `PENDING` não entram na soma — é a mesma
pegadinha documentada em `fake-erp.md`: o `totalAmount` que a API devolve
inclui os três status juntos, e não serve como "receita paga".

### 3. Qualidade — Ticket médio pago

**Regra de cálculo:** Receita paga (número 2) ÷ Pedidos pagos (número 1). Se
não houver nenhum pedido pago no mês, o ticket médio é "não aplicável" — nunca
dividir por zero, nunca aproximar.

## Conferência — janeiro/2026 (valores de referência)

| Número | Valor |
|---|---|
| Pedidos pagos | 2 (pedidos 1001 e 1002) |
| Receita paga | R$ 1.430,00 |
| Ticket médio pago | R$ 715,00 |

Esses três valores servem para conferir se qualquer implementação futura
(painel, regra, script) está seguindo esta fonte corretamente.

# fake-erp — contexto da API

> Arquivo de contexto para o próprio Claude reler. Ensina a acessar e usar a API do
> "FakeERP" (ERP de treino da Link School / isilab) sem depender de conector pronto.
> Não tem conteúdo de negócio da boutique — é só o manual da porta.

## O que é

ERP de treino com **duas rotas apenas**: uma para autenticar, outra para puxar o
relatório de pedidos de um mês. Serve para praticar "consumir uma API sem conector".

- **URL base:** `https://fake-erp.isilab.com.br`
- **Manual (OpenAPI):** `https://fake-erp.isilab.com.br/v3/api-docs`
- **Autenticação:** HTTP Bearer com token JWT. Todas as rotas exigem token, menos o login.

## Como autenticar

`POST /auth/login` — corpo JSON com os campos **`login`** e **`password`** (o campo é
`login`, **não** `username`).

```bash
curl -s -X POST https://fake-erp.isilab.com.br/auth/login \
  -H "Content-Type: application/json" \
  -d '{"login":"user","password":"user"}'
```

Resposta:

```json
{ "token": "eyJhbGciOiJIUzM4NCJ9...", "type": "Bearer", "expiresInMs": 3600000 }
```

O `token` vale **1 hora** (`expiresInMs` = 3.600.000). Depois disso, pedir outro.
Credenciais de treino: `user` / `user`.

## Endpoint de relatório

`GET /report/{year}/{month}` — `year` e `month` são números inteiros na própria URL
(ex.: `/report/2026/2` para fevereiro/2026). Exige o header
`Authorization: Bearer <token>`.

```bash
# 1. pega o token e guarda na variável
TOKEN=$(curl -s -X POST https://fake-erp.isilab.com.br/auth/login \
  -H "Content-Type: application/json" \
  -d '{"login":"user","password":"user"}' \
  | python3 -c "import sys,json;print(json.load(sys.stdin)['token'])")

# 2. usa o token para puxar o relatório
curl -s https://fake-erp.isilab.com.br/report/2026/2 \
  -H "Authorization: Bearer $TOKEN" | python3 -m json.tool
```

### Formato da resposta

```json
{
  "year": 2026,
  "month": 2,
  "count": 3,                // nº de pedidos no mês
  "totalValue": 3459.9,      // soma dos valores cheios (antes do desconto)
  "totalDiscount": 310.0,    // soma dos descontos
  "totalAmount": 3149.9,     // totalValue - totalDiscount
  "orders": [
    {
      "orderId": 1005,
      "orderDateTime": "2026-02-03T08:15:00",
      "value": 760.0,        // valor cheio
      "discount": 60.0,
      "total": 700.0,        // value - discount
      "status": "PAID"       // PAID | CANCELLED | PENDING
    }
  ]
}
```

## ⚠️ Pegadinha de negócio (importante)

`totalValue` / `totalAmount` **somam TODOS os pedidos**, inclusive `CANCELLED` e
`PENDING`. "Quanto foi vendido de verdade" = somar apenas os `orders` com
`status == "PAID"`. A API não faz esse filtro; quem faz é a análise.

Ao devolver um relatório, sempre separar:
- **Faturamento pago** = soma de `total` dos pedidos `PAID`
- Pedidos cancelados e pendentes listados à parte, com valor
- Ticket médio = faturamento pago ÷ nº de pedidos `PAID`
- Apontar concentração (um pedido que representa fatia grande do mês) e outliers

Entregar como um analista entregaria — em português, tabela, sem JSON cru.

## Meses com dados

Só **janeiro (1), fevereiro (2), março (3) e julho (7) de 2026** têm pedidos.
Qualquer outro mês volta `count: 0` e `orders: []` com HTTP 200 — **isso não é erro**,
é a base de treino. Se vier vazio, dizer que o mês não tem pedidos registrados.

## Erros comuns e o que fazer

| Sintoma | Causa | O que fazer |
|---|---|---|
| "Não posso fazer essa chamada" | Política do assistente, não erro da API | Rodar pelo terminal (`curl`/script), ou gerar uma página HTML que consome a API |
| HTTP 401 / 403 no login | Nome do campo errado | O campo é `login` e `password` — não `username`, não `email` |
| HTTP 401 numa chamada que já funcionou | Token expirou (passou 1h) | Refazer o `POST /auth/login` e repetir com o token novo |
| Resposta parece inventada | Assistente "deduziu" em vez de chamar | Exigir: fazer a chamada de verdade e mostrar o JSON bruto antes de analisar |
| Veio vazio, sem erro | Mês sem pedidos (ver lista acima) | Informar que o mês não tem dados; não tratar como bug |
| Terminal travado / API fora do ar | Instabilidade do servidor de treino | Alvo reserva: `https://brasilapi.com.br/docs` (sem login) |

## Como pedir (exemplos de frase)

- "me traz o relatório de fevereiro de 2026"
- "compara janeiro e fevereiro de 2026 de vendas pagas"
- "puxa março/2026 e me diz o ticket médio só dos pedidos pagos"

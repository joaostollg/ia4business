---
name: gera-briefing
description: Monta o briefing antes de uma conversa importante — dono de ISP, sócio do Grupo Sun, possível fornecedor de serviço recorrente (contábil, jurídico, marketing), ou professor. Use quando o João disser "vou conversar com...", "tenho reunião com...", "me prepara pra call", "gera um briefing". Lê contexto/negocio.md e contexto/cliente.md e devolve objetivo, pauta, perguntas a fazer, o que não esquecer e o que evitar.
---

# gera-briefing

Preparação para uma conversa que importa. Sai da skill com um roteiro claro do que falar, o que perguntar e o que não deixar passar.

## Passo 1 — Entender a conversa

Se o João não informou, perguntar (uma pergunta por vez, múltipla escolha quando der):

1. **Com quem** é a conversa? (dono de ISP / sócio Grupo Sun / fornecedor de serviço / professor / outro)
2. **Qual o objetivo** dele com essa conversa? (o que ele quer sair tendo conseguido)
3. **Quando** é e por qual canal? (presencial, call, WhatsApp)
4. **O que já aconteceu antes** com essa pessoa? (primeiro contato ou continuação)

## Passo 2 — Ler o contexto

Ler `contexto/negocio.md` e `contexto/cliente.md`. Se a conversa for com dono de ISP, esses dois arquivos são a base do briefing.

## Passo 3 — Montar o briefing

Formato de saída:

```
# Briefing — conversa com <interlocutor> (<data>)

## Objetivo
<1 frase: o que o João quer que essa conversa produza>

## Quem é o interlocutor
<o que sabemos: perfil, o que provavelmente quer, o que provavelmente teme>

## Pauta sugerida
1. <tópico> — <por que entra>
2. ...

## Perguntas a fazer
- <pergunta aberta> → serve para descobrir <o quê>
- ...

## O que NÃO esquecer
- <pontos-chave do contexto que precisam aparecer>

## O que evitar
- <o que não dizer / erros de discurso a não cometer>

## Próximo passo desejado
<o que o João quer que fique combinado ao fim da conversa>
```

## Régua — conversa com dono de ISP

Quando o interlocutor é um dono de ISP, o briefing **tem que** refletir:

- **Não liderar com M&A.** A porta de entrada é serviço recorrente bem entregue. M&A é o objetivo final da relação, não o primeiro assunto.
- **As 3 camadas de medo** do dono, sempre presentes mesmo que ele não admita: (1) perder o controle do negócio que é a vida dele, (2) desconfiança por não entender de M&A, (3) o "e agora?" pós-venda — o que fazer com o dinheiro e com a rotina.
- **A concorrência é a inércia.** O discurso não é "somos melhores que X", é "isso precisa ser resolvido agora, e é mais simples do que parece".
- **Gestão patrimonial pós-venda é parte da oferta**, não um apêndice — pode e deve aparecer como acompanhamento depois da transação.
- **Não prometer o que a boutique ainda não valida entregar.** Estágio atual: ideia/validando, sem cliente pagante fechado. Falar em hipótese e teste, não em track record.
- **Churn é falta de entrega percebida.** Se a conversa envolve escopo, puxar para poucos serviços com resultado visível, não amplitude.

## O que NÃO fazer

- Não salvar arquivo sem o João pedir. Saída vai para o chat; se ele quiser guardar, oferecer salvar em `briefings/AAAA-MM-DD-<interlocutor>.md`.
- Não inventar informação sobre o interlocutor que não foi dada nem está no contexto — marcar como "a confirmar na conversa".

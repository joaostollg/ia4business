# Skills

Skills instaladas no Claude Code para uso neste e nos demais projetos.

| Comando | Para que serve | Quando usar |
|---|---|---|
| `/video-downloader` | Baixa vídeo ou áudio (YouTube e outros sites), ou pega a transcrição para resumir | "resume esse vídeo: `<link>`" · "baixa o áudio em mp3: `<link>`" |
| `/meeting-insights-analyzer` | Analisa transcrições de reunião: quanto você fala, vícios de linguagem, quando evita conflito, estilo de condução | Depois de uma reunião gravada/transcrita, apontar a pasta com os arquivos |
| `/lead-research-assistant` | Encontra e prioriza empresas que seriam bons clientes ou parceiros, com estratégia de abordagem para cada uma | "acha empresas no interior de SP que precisariam de assessoria de M&A" |
| `/competitive-ads-extractor` | Extrai anúncios de concorrentes (Facebook/LinkedIn) e analisa mensagem, dores e padrões de criativo | "extrai os anúncios da `<empresa>` e me diz o que está funcionando" |
| `/recursive-research` | Pesquisa profunda em ciclos, com hierarquia de fontes confiáveis e checkpoints em disco. Ideal para panoramas de mercado e due diligence de M&A | Sempre abrir com `/recursive-research` seguido do tema |

**Como usar:** na maioria dos casos basta escrever o pedido em linguagem natural que o Claude escolhe a skill certa. A barra (`/comando`) serve para garantir o uso — e para a `/recursive-research`, usar sempre a barra.

## Skills deste projeto

Vivem em `.claude/skills/` deste repositório (só funcionam aqui, diferente das globais acima).

| Comando | Para que serve | Quando usar |
|---|---|---|
| `/revisa-repo` | Revisão crítica do repositório antes de qualquer entrega: afirmação vaga, número sem fonte, hipótese tratada como fato, inconsistência com `contexto/`, arquivo fora do padrão de nome | Antes de mandar algo pra fora ou de dar `git push`; ou sob pedido explícito |
| `/gera-briefing` | Monta o briefing antes de uma conversa importante (dono de ISP, sócio do Grupo Sun, fornecedor, professor), lendo `contexto/` | "vou conversar com...", "me prepara pra call" |
| `/fecha-conversa` | Notas cruas de reunião → ata (decisões, ações com dono e prazo, pontos em aberto), propõe atualização do `contexto/` e prepara o commit | Depois de uma reunião, colando as anotações |
| `/internal-comms` | Escreve comunicação interna nos formatos certos (status report, update de liderança, newsletter, FAQ) | Pedir um desses tipos de comunicação |

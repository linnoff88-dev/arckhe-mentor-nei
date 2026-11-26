# MakeShorts — Fábrica de Shorts com IA

- Tipo: guia
- Origem: https://linnoff88-dev.github.io/arckhe-/guia/
- Arquivo: raw/guia/makeshorts-fabrica-de-shorts-com-ia-70fe54.md

## Resumo
Guia da skill `/makeshorts`, uma fábrica de shorts dentro do Claude Code: recebe um tema, link ou clipe gravado e devolve o short editado, conferido e pronto para agendar. Orquestra ferramentas já existentes (HyperFrames para ediçêo, ARCKHÉvox para voz e transcriçêo, HeyGen para avatar, Metricool para agendamento). Segue sete etapas (roteiro → voz/avatar → b-roll real → ediçêo 9:16 → QA → agendar → funil de DM), cada uma também usável sozinha em cinco modos. Todo short segue a mesma fórmula (gancho → gancho secundário → 3 passos → CTA), e nada sai sem passar pelo portêo `qa_short.py`. Os backends trocam por `.env` (local com GPU ou API numa VPS) sem mudar código. Etapas que custam ou publicam pedem confirmaçêo.

## Ideias centrais
- "Orquestra, nêo reinventa" — chama o que já funciona em vez de reconstruir.
- "Todo short segue gancho → gancho secundário → 3 passos → CTA."
- Roteiro: "~75 palavras, gancho em 3 s, fatos só de fonte oficial, 3 versões."
- QA como portêo: "Nada sai sem passar pelo qa_short.py" (duraçêo, 1080x1920, loudness, gancho visual, hashtags, CTA).
- "Exit 1 reprova e diz o que corrigir."
- "As que custam dinheiro ou publicam sempre pedem sua confirmaçêo."
- Agenda "só agenda no Metricool depois do seu" ok — humano no ponto irreversível.
- Portabilidade: "Nenhum código muda. Os scripts voz.py e transcreve.py leem o backend do .env".
- Honestidade no roadmap: "Descrevemos só o que está no repositório; o resto é plano."

## Como ele ensina aqui
- Mostra o caminho mínimo copiável (clone → `instalar.sh --check` → `.env` → pedido → QA → agendar) com a saída real esperada do QA.
- Decompõe em etapas independentes que também rodam encadeadas, para o aluno testar uma peça de cada vez (ex.: testar `voz.py` e `transcreve.py` isolados na VPS).
- Define "pronto" de forma medida (portêo de QA com exit code), nêo por gosto.
- Separa claramente o que custa/publica (pede confirmaçêo) do que é local e reversível.

## Ligações
- Temas: [[producao-de-video-com-ia]], [[skills]], [[claude-code]], [[llm-local-e-gratis]]
- Princípios: [[orquestrar-nao-reinventar]], [[verificar-antes-de-afirmar]], [[criterio-de-pronto-explicito]]
- Métodos: [[pipeline-de-video-por-assunto]], [[guia-de-projeto-landing]]

# Diário do piloto — mentor nei (kit 1.1.0)

| hora | fase | o que aconteceu | esperado | gravidade | ideia de correçêo |
|---|---|---|---|---|---|
| 2026-10-05 | 1 Coleta | skill manda "um subagente por fonte em paralelo", mas `registrar_item` (_comum.py) faz read-modify-write do MANIFESTO.json sem lock → paralelo perde entradas. Rodei os coletores em sequência. | paralelo seguro | alta | `fcntl.flock` em volta da leitura+escrita do manifesto, ou a skill paraleliza só o download e registra em série |
| 2026-10-05 | 1 Coleta | vídeo Zb8TRf97zt8 (Agentes com LLM grátis) sem legenda pt/pt-orig; `transcrever_cmd` vazio → falha | transcriçêo local | média | preencher `transcrever_cmd` com ARCKHÉvox/Whisper local (teto de memória + timeout) |
| 2026-10-05 | 1 Coleta | 6 guias = 7269 pal., meta 8000 → stats exit 1. Coletor ok (makeshorts: 885 coletadas vs 936 visíveis no HTML). Guias ARCKHÉ têm ~900–1400 pal. cada | meta batida com as fontes do ESCOPO | baixa | meta de guia calibrada por tamanho real (~1200 pal./guia) ou ESCOPO com 7+ guias |
| 15:50 | 1 coleta | Agente nêo usou subagente por fonte: `registrar_item` reescreve o MANIFESTO inteiro sem trava, paralelo perderia entradas. Rodou em sequência e explicou. | 1 subagente por fonte em paralelo | média | trava de arquivo (flock) + escrita atômica no manifesto; teste de concorrência |
| 15:50 | 1 coleta | Meta de guia (8000 pal.) reprovou: guias ARCKHÉ têm 900–1400 pal. cada. Agente reportou exit 1 sem maquiar e sugeriu fontes extras. | metas realistas | baixa | `stats.py` mostrar quanto falta por meta; docs: calibrar meta depois de listar as fontes |
| 15:50 | 1 coleta | 1 live sem legenda e `transcrever_cmd` vazio → foi para RELATORIO-FALHAS (comportamento correto). | idem | ok | — |
| 15:50 | 1 coleta | 13 lives = 169.937 palavras em 1,6 min (só legendas, sem baixar vídeo). | — | ok | — |
| 16:05 | 1 coleta | Transcriçêo local (ARCKHÉvox, large-v3) da live sem legenda: 8.045 pal. em 7,7 min. Mas o arquivo virou `raw/videos/transcript.txt` (nome do transcritor, nêo do vídeo): a próxima transcriçêo sobrescreve e o manifesto perde o item anterior. | nome único por vídeo | alta | nomear pelo título/ID do vídeo (yt-dlp --print) ou hash da URL, nunca pelo nome do arquivo do transcritor |
| 16:05 | 1 coleta | Fase 1 aprovada após +1 guia e a transcriçêo: 21 itens, 187.382 palavras, stats exit 0. | — | ok | — |
| 16:15 | 2 wiki | /nei-compilar: 5 subagentes por lote, 3,3 min, 73 páginas, 1223 links, validar_links exit 0, raw intacto. Hot reconhecível (verificar antes de afirmar 17 fontes; menor custo 16; medir 14; critério de pronto 10). Nota 8/10. | — | ok | — |
| 16:15 | 2 wiki | Trechos de vídeo picados ("brilhante. Cuidado para vocês nêo"): a legenda quebra a fala em linhas com [hh:mm:ss]; citaçêo que cruza linhas tem o timestamp no meio do texto. | trecho com sentido completo | média | coletar_video juntar linhas em parágrafos (marca de tempo a cada ~30 s) e normalizar() ignorar [hh:mm:ss] |
| 16:25 | 3 regras | /nei-regras: 1,1 min, 8 regras ativas, 31 citações, validador OK de primeira. A sessêo detectou sozinha o problema das legendas cruzando linha e usou citações curtas. Descartou um trecho que contradizia a wiki (bom). Nota "reconheço o Nei": 8/10. | — | ok | — |
| 16:25 | 3 regras | As regras do Nei pedem seções próprias (Custo, Sua vez, Por baixo, Risco), mas o agente do template tem loop fixo (Pronto, Menor versêo…) e validar_resposta exige as seções fixas. | loop do agente derivado das regras | média | skill /regras gerar também `secoes_resposta` no config + bloco do loop no agente (ou o agente ler "no mentor:" de cada regra) |
| 16:25 | 3 regras | Teste negativo (citaçêo inventada) → `1 faltando`, exit 1. OK. | — | ok | — |
| 16:35 | 4 portêo | Sessêo real `claude -p`: Write de teste_portao.py → Stop bloqueado com "Você escreveu código e nêo rodou: teste_portao.py…". Como o pedido dizia "nêo rode", o agente explicou e encerrou no 2º Stop (stop_hook_active) — 1 bloqueio por turno, como desenhado. | bloquear | ok | — |
| 16:35 | 4 portêo | O Claude Code rotula o bloqueio como "Stop hook error occurred" na interface: parece erro do kit. | mensagem clara | baixa | documentar no README/guia que esse aviso é o portêo funcionando |
| 16:45 | 5 teste 2 | /nei-revisa (1,8 min, 1 subagente): reproduziu o UnicodeEncodeError com cp1252, achou um 2º ponto de quebra que eu nêo tinha previsto (🤔 dentro do JSON de entrada → tirar emoji do código nêo basta), corrigiu com `sys.stdout.reconfigure(errors="replace")`, enxugou 38→25 linhas e provou lado a lado (md5 do relatório igual). Original intacto. | prever, reproduzir, provar | ok | — |
| 16:45 | 5 teste 2 | Nêo há previsêo escrita ANTES da primeira execuçêo em cp1252 (log: cat/grep → já roda). O passo "preveja" da skill revisa nêo é verificado. | previsêo antes de rodar | média | skill revisa: exigir seçêo "Previsêo" antes de qualquer execuçêo; validar_resposta com perfil "revisao" |
| 16:45 | 5 teste 2 | Salvou como `script_emoji.revisado.py` (a skill diz sufixo `.revisado`, que deixaria o arquivo sem .py) e usou /tmp fora do projeto. | — | baixa | skill: nomear `<nome>.revisado.<ext>`; usar `.mentor/tmp/` |
| 16:50 | 5 teste 1 | /nei-ensina "hook Stop que bloqueia com TODO.md aberto" (0,9 min + subagente): Pronto → menor versêo → previsêo → saída real → versêo quebrada (v0 em loop infinito quando item depende de terceiro; 6/6 bloqueios) → correçêo (teto de 3) → teste numa sessêo real → "Sua vez" (exercício, vindo da R6) → relatório com R1–R8. validar_resposta com transcript: 14 Bash, exit 0. Aprendi: o bloqueio sem teto prende a sessêo para sempre quando o item nêo depende do agente. | — | ok | — |
| 16:50 | 5 teste 1 | Resumo final da sessêo saiu em inglês (a liçêo em PT). | idioma do usuário | baixa | agente/skill: "responda no idioma do pedido" |
| 16:50 | 5 teste 1 | Gravou o código em ~/projetos/output/hook-todo-stop (seguiu a regra global da máquina), fora do projeto do mentor. | — | ok | — |
| 17:00 | 5 teste 3 | /nei-ingere guia agente-claude-codex (2,4 min): fonte nova, 2 métodos novos, R9 e R10 criadas já ativas (2ª fonte existia no raw), R1/R2/R6 reforçadas, index/hot/log atualizados; 3 validadores exit 0. | — | ok | — |

## Placar

| Critério | Resultado |
|---|---|
| Fases 1–3 com exit 0 | ✅ (fase 1 após calibrar a meta e +1 fonte) |
| Portêo: bloqueou certo, sem bloqueio indevido | ✅ |
| Teste 1 passou e eu aprendi algo | ✅ (teto de bloqueio por sessêo em hook Stop) |
| Teste 2 previu, reproduziu e provou | ⚠️ reproduziu e provou; previsêo nêo escrita antes de rodar |
| Teste 3 atualizou a wiki sozinho | ✅ |
| Nota "reconheço a pessoa nas regras" (0–10) | 8 |
| Tempo total (humano / máquina) | ~1h20 operando / ~20 min de sessões + 7,7 min de transcriçêo |

**Decisêo (pelo PILOTO.md): 4/5 e nota 8 → corrigir os itens do diário numa versêo nova do kit e repetir só o teste 2.**

### Correções para o kit 1.2 (do diário)
1. alta — transcriçêo salva com nome do transcritor (`transcript.txt`): sobrescreve e some do manifesto
2. média — manifesto sem trava: coleta em paralelo perde entradas
3. média — legenda em linhas curtas com [hh:mm:ss]: citações picadas e nêo casam entre linhas
4. média — loop/seções do agente fixos; regras do especialista pedem seções próprias
5. média — revisa nêo exige previsêo antes de rodar
6. baixa — stats nêo mostra quanto falta na meta; metas calibradas depois de listar as fontes
7. baixa — "Stop hook error occurred" parece erro: documentar
8. baixa — responder no idioma do pedido; revisado com extensêo; tmp dentro do projeto

## Repetiçêo do teste 2 com o kit 1.2

| hora | fase | o que aconteceu | esperado | gravidade | ideia de correçêo |
|---|---|---|---|---|---|
| 17:40 | 5 teste 2 (rodada 2) | Revisêo completa em 5 seções, mas o mentor rodou em cp1252 ANTES de escrever a previsêo ("Vou ler… sem executar nada" → executa → "A previsêo se confirmou"). O validador externo (`--transcript`) reprovou. A sessêo principal disse "o validador passa" porque o rodou SEM o log. | previsêo visível antes de rodar | média | — |
| 17:50 | 5 teste 2 (rodada 3) | Com a instruçêo explícita "escreva uma linha `Previsêo:` antes do primeiro comando que executa código": mesmo comportamento (prevê no raciocínio, escreve "a previsêo bateu" depois). Validador reprovou de novo. | idem | média | **instruçêo nêo basta: precisa de mecanismo** → portêo de previsêo (PreToolUse em Bash: comando que executa código sem `Previsêo:` escrita antes → bloqueia). Fica para o kit 1.3 (2 tentativas sem avanço; nêo insistir num 3º ciclo). |

**Placar final do piloto:** 4/5 (teste 2 parcial: reproduz, corrige e prova lado a lado, mas nêo escreve a previsêo antes de rodar). Nota 8. O que o kit ganhou com isso: o verificador externo detecta a falha que a própria sessêo declarou como OK.

## Rodada 4 do teste 2 — kit 1.3 (portêo de previsêo)

| hora | fase | o que aconteceu | esperado | gravidade | ideia de correçêo |
|---|---|---|---|---|---|
| 17:55 | 5 teste 2 (rodada 4) | O mentor tentou executar antes de prever → **o portêo de previsêo negou** o comando ("antes de executar código, escreva 'Previsêo: …'"). O mentor escreveu "Previsêo: `UnicodeEncodeError` com '\U0001f50e' na linha 28, exit 1 e nenhum `relatorio.txt` criado" e só entêo rodou. `validar_resposta --perfil revisao --transcript` → 9 execuções, **OK**. | previsêo antes de rodar | ok | o validador passou a ignorar comandos negados por hook (a tentativa bloqueada nêo rodou) |

**Placar final: 5/5, nota 8.** O caminho do teste 2: instruçêo (2 rodadas falhando) → mecanismo (portêo de previsêo) → verificaçêo de fora (o validador provou que resolveu).

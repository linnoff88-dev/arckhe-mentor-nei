# Resultados dos testes de aceitaçêo — piloto mentor nei (kit 1.1.0)

| data | teste | passou? | evidência (arquivo/saída) | observaçêo |
|---|---|---|---|---|
| 2026-10-05 | 1 Construir e ensinar | sim | `testes/respostas/2026-10-05-hook-stop-todo.md`; `validar_resposta.py --transcript logs/teste1.jsonl` → 14 Bash, exit 0 | versêo quebrada real (loop infinito), teste numa sessêo real, "Sua vez" vindo da R6 |
| 2026-10-05 | 2 Revisar script "que funciona" | parcial | `logs/teste2.jsonl`; `script_emoji.revisado.py` roda em cp1252, md5 do relatório igual ao original | reproduziu e provou lado a lado, achou 2º ponto de quebra; **sem previsêo escrita antes de rodar** |
| 2026-10-05 | 3 Ingerir fonte nova | sim | `wiki/log.md` (linha nova), stats/links/citações exit 0, 22 fontes, 10 regras / 40 citações | R9 e R10 novas já ativas; R1, R2, R6 reforçadas |
| 2026-10-05 | 2 (rodadas 2 e 3, kit 1.2) | parcial | `logs/teste2b.jsonl`, `logs/teste2c.jsonl`; `validar_resposta --perfil revisao --transcript` → reprovado nas duas | 5 seções completas e prova lado a lado; previsêo continua só depois de rodar → portêo de previsêo no kit 1.3 |
| 2026-10-05 | 2 (rodada 4, kit 1.3) | **sim** | `logs/teste2d.jsonl`; portêo de previsêo negou 1 execuçêo; `validar_resposta --perfil revisao --transcript` → 9 Bash, OK | previsêo escrita antes de rodar; reproduziu, corrigiu, lado a lado |

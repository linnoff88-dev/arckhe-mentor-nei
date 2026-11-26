# Execuçêo Longa — agentes que trabalham horas e dias sem perder o rumo

- Tipo: guia
- Origem: https://linnoff88-dev.github.io/arckhe-/guia/
- Arquivo: raw/guia/execucao-longa-agentes-que-trabalham-horas-e-dias--b43579.md

## Resumo
Guia do repositório `execucao-longa`: método e templates para agentes (Codex CLI ou Claude Code com `/goal`) trabalharem horas ou dias sem se perder. A base tem três peças: objetivo verificável (Resultado, Restrições, Verificaçêo), estado em arquivos (goal, plan, state, progress, failures, decisions + canal.md) e tetos e portões humanos. O ciclo é: lê o estado → próxima açêo → executa → testa → corrige → salva + commit, e para com três ciclos sem avanço. Há dois caminhos: interativo (com hook que manda reler o estado depois de compactar e avisa nas faixas de 50/70/85% de contexto) e headless (`loop-longrun.sh` com timeout, teto de memória, flock e testes protegidos). Ensina uma escala de critério de pronto (0 a 4, com mínimo no nível 3), o modo fila, cuidados com cache e compactaçêo, e separa o que está confirmado do que nêo tem fonte. O roadmap mostra fases medidas (piloto concluído em 3 min, medir-sessao lendo 1,8 GB em cerca de 9 s, `recall` com 90 mil trechos).

## Ideias centrais
- O objetivo precisa ser provado por comando: "comandos cuja saída prova que terminou."
- O estado mora em arquivos, nêo na memória: "o agente relê os arquivos em vez de depender da memória da conversa."
- Portões humanos: "Gasto de crédito, API, produçêo e ações irreversíveis viram portêo humano."
- Parar por estagnaçêo: "Três ciclos sem avanço mensurável = parar e chamar o humano."
- Critério bom: "Bom critério: alguém de fora consegue checar sem confiar no agente, e o agente nêo consegue cumprir por atalho."
- Critério burlável é armadilha: "Dá para apagar teste ou gerar página vazia."
- Quem julga é o teste: "Quem decide continuar é o teste"
- O problema é a compactaçêo repetida: "Nêo é o /goal , é a compactaçêo repetida."
- Fila só fecha com prova: "Fechar exige evidência (saída de teste, arquivo, hash de commit)"
- Cache custa: "Perder o cache custa 12,5x a 25x."

## Como ele ensina aqui
- Pede conferência independente no fim: "Confira você mesmo (rode o teste final, veja o hash dos testes)", e mediçêo com `medir-sessao.py` anotada no progress.md.
- Separa evidência de boato: blocos Confirmado, Sem fonte e Nêo é padrêo oficial; ex.: "A sessêo de 11 dias / 573 turnos / 1,3 GB nêo tem fonte pública."
- Ensina com exemplo do critério ruim ao bom (traduzir o guia para inglês: nível 0 → 2 → 3–4).
- Reporta o piloto com números: "concluída no 1º ciclo, em 3 min, e conferida à parte."
- Limita a autonomia: "O agente cria no máximo 5 tarefas por execuçêo".

## Ligações
- Temas: [[execucao-longa]], [[contexto-e-memoria]], [[custo-de-tokens]], [[filas-e-workers]], [[claude-code]]
- Princípios: [[criterio-de-pronto-explicito]], [[verificar-antes-de-afirmar]], [[medir-antes-de-acreditar]]
- Métodos: [[execucao-longa-com-estado-em-arquivos]], [[guia-de-projeto-landing]]

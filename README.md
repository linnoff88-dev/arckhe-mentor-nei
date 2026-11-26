ï»¿# mentor-nei ê¢â¬â piloto real do kit Mentor-Especialista

Mentor no Claude Code que ensina **no mêÂ©todo do Nei Maldaner (ARCKHÉ)**: usar Claude Code e agentes de IA na prêÂ¡tica, construindo junto. Foi gerado com o kit **[linnoff88-dev/mentor-especialista](https://github.com/linnoff88-dev/mentor-especialista)** e serve de **exemplo real completo**: acervo, wiki, regras com citaêÂ§êÂ£o, testes e o diêÂ¡rio do piloto.

> Copia o mêÂ©todo, nêÂ£o a pessoa. O acervo êÂ© conteêÂºdo pêÂºblico do prêÂ³prio Nei (lives do canal [@linnoff88-devx](https://www.youtube.com/@linnoff88-devx) e guias em linnoff88-dev.github.io), publicado aqui com autorizaêÂ§êÂ£o dele.

## NêÂºmeros

| | |
|---|---|
| Fontes | 22 (14 lives + 8 guias) |
| Palavras | ~189 mil (legendas pêÂºblicas + 1 transcriêÂ§êÂ£o local) |
| Wiki | 76 pêÂ¡ginas, ~1.300 links (fontes, temas, princêÂ­pios, mêÂ©todos) |
| Regras | 10, todas ativas, 40 citaêÂ§êÂµes conferidas por script |
| Piloto | **5/5** critêÂ©rios (4/5 atêÂ© o portêÂ£o de previsêÂ£o do kit 1.3), nota 8 em "reconheêÂ§o a pessoa nas regras" |

As regras mais fortes, pelo nêÂºmero de fontes: **conferir o resultado real antes de dizer "pronto"**, **definir o critêÂ©rio de pronto antes de comeêÂ§ar**, **medir com e sem antes de acreditar** e **usar o menor custo que resolve**. Veja [`regras.md`](regras.md) e [`wiki/hot.md`](wiki/hot.md).

## Usar

```bash
git clone https://github.com/linnoff88-dev/mentor-nei.git
cd mentor-nei
python3 tools/stats.py && python3 tools/validar_links.py && python3 tools/validar_citacoes.py
claude      # abra o Claude Code nesta pasta
```

Dentro do Claude Code:

```
/nei-ensina me ensina a fazer um hook Stop que bloqueia enquanto o TODO.md tiver itens abertos
/nei-revisa caminho/do/script.py
/nei-ingere https://linnoff88-dev.github.io/arckhe-<outro-guia>/guia/
```

## O que olhar

- [`PILOTO-DIARIO.md`](PILOTO-DIARIO.md): tudo o que aconteceu em cada fase, o placar e as correêÂ§êÂµes que viraram o kit 1.2
- [`testes/aceitacao/RESULTADOS.md`](testes/aceitacao/RESULTADOS.md): os 3 testes de aceitaêÂ§êÂ£o e as evidêÂªncias
- [`testes/respostas/`](testes/respostas/): liêÂ§êÂµes reais do mentor (com PrevisêÂ£o ê¢â â SaêÂ­da real ê¢â â VersêÂ£o quebrada ê¢â â RelatêÂ³rio)
- [`exemplos/hook-todo-stop/`](exemplos/hook-todo-stop/): o cêÂ³digo que o mentor construiu no teste 1, incluindo a versêÂ£o ingêÂªnua que entra em loop infinito
- [`FALHAS.md`](FALHAS.md): falhas de operaêÂ§êÂ£o e a menor correêÂ§êÂ£o de cada uma

## Atualizar para a versêÂ£o nova do kit

```bash
python3 ../mentor-especialista/novo-mentor.py --atualizar .
```

LicenêÂ§a MIT (cêÂ³digo). O conteêÂºdo de `raw/` pertence ao autor; êÂ© usado aqui para estudo.

---

Projeto  - ARCKHÉe
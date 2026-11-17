# mentor-nei â€” piloto real do kit Mentor-Especialista

Mentor no Claude Code que ensina **no mÃ©todo do Nei Maldaner (INEMA)**: usar Claude Code e agentes de IA na prÃ¡tica, construindo junto. Foi gerado com o kit **[inematds/mentor-especialista](https://github.com/inematds/mentor-especialista)** e serve de **exemplo real completo**: acervo, wiki, regras com citaÃ§Ã£o, testes e o diÃ¡rio do piloto.

> Copia o mÃ©todo, nÃ£o a pessoa. O acervo Ã© conteÃºdo pÃºblico do prÃ³prio Nei (lives do canal [@inematdsx](https://www.youtube.com/@inematdsx) e guias em inematds.github.io), publicado aqui com autorizaÃ§Ã£o dele.

## NÃºmeros

| | |
|---|---|
| Fontes | 22 (14 lives + 8 guias) |
| Palavras | ~189 mil (legendas pÃºblicas + 1 transcriÃ§Ã£o local) |
| Wiki | 76 pÃ¡ginas, ~1.300 links (fontes, temas, princÃ­pios, mÃ©todos) |
| Regras | 10, todas ativas, 40 citaÃ§Ãµes conferidas por script |
| Piloto | **5/5** critÃ©rios (4/5 atÃ© o portÃ£o de previsÃ£o do kit 1.3), nota 8 em "reconheÃ§o a pessoa nas regras" |

As regras mais fortes, pelo nÃºmero de fontes: **conferir o resultado real antes de dizer "pronto"**, **definir o critÃ©rio de pronto antes de comeÃ§ar**, **medir com e sem antes de acreditar** e **usar o menor custo que resolve**. Veja [`regras.md`](regras.md) e [`wiki/hot.md`](wiki/hot.md).

## Usar

```bash
git clone https://github.com/inematds/mentor-nei.git
cd mentor-nei
python3 tools/stats.py && python3 tools/validar_links.py && python3 tools/validar_citacoes.py
claude      # abra o Claude Code nesta pasta
```

Dentro do Claude Code:

```
/nei-ensina me ensina a fazer um hook Stop que bloqueia enquanto o TODO.md tiver itens abertos
/nei-revisa caminho/do/script.py
/nei-ingere https://inematds.github.io/<outro-guia>/guia/
```

## O que olhar

- [`PILOTO-DIARIO.md`](PILOTO-DIARIO.md): tudo o que aconteceu em cada fase, o placar e as correÃ§Ãµes que viraram o kit 1.2
- [`testes/aceitacao/RESULTADOS.md`](testes/aceitacao/RESULTADOS.md): os 3 testes de aceitaÃ§Ã£o e as evidÃªncias
- [`testes/respostas/`](testes/respostas/): liÃ§Ãµes reais do mentor (com PrevisÃ£o â†’ SaÃ­da real â†’ VersÃ£o quebrada â†’ RelatÃ³rio)
- [`exemplos/hook-todo-stop/`](exemplos/hook-todo-stop/): o cÃ³digo que o mentor construiu no teste 1, incluindo a versÃ£o ingÃªnua que entra em loop infinito
- [`FALHAS.md`](FALHAS.md): falhas de operaÃ§Ã£o e a menor correÃ§Ã£o de cada uma

## Atualizar para a versÃ£o nova do kit

```bash
python3 ../mentor-especialista/novo-mentor.py --atualizar .
```

LicenÃ§a MIT (cÃ³digo). O conteÃºdo de `raw/` pertence ao autor; Ã© usado aqui para estudo.

---

Projeto adaptado da comunidade INEMA TDS - Marca Arckhe
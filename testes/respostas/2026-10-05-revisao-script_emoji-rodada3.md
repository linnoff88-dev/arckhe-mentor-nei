# Revisêo — `testes/aceitacao/script_emoji.py`

> Rodada 3. A revisêo segue o método (Previsêo → Reproduçêo → Correçêo → Lado a lado → Relatório). Nêo fala em nome de Nei Maldaner.

## Previsêo

Escrita na conversa **antes** do primeiro comando que executou código (R3). Até ali, só tinha lido os arquivos com `cat`.

> Previsêo: no terminal UTF-8 normal o script imprime "🔎 lendo 5 comentários…", três linhas "❓" (ana "Como faço isso no Windows?", caio "Funciona com Python 3.9?", edu "Onde baixo o arquivo de exemplo? 🤔?"), "✅ 3 perguntas salvas em relatorio.txt", e sai com exit 0. Com `PYTHONIOENCODING=cp1252` (o que acontece num console Windows redirecionado, num pipe ou num CI com locale cp1252) ele cai já no primeiro `print`: `UnicodeEncodeError: 'charmap' codec can't encode character '\U0001f50e'`, exit 1, e o `relatorio.txt` nem chega a ser gravado, porque a escrita vem depois dos prints. A armadilha é tirar só os emojis decorativos: o dado real (edu, "🤔") também tem emoji, entêo imprimir a pergunta dele quebra do mesmo jeito. A correçêo precisa estar na saída (`errors="replace"`), nêo só no texto fixo. Quem recebe aceitaria o resultado no Linux/mac, mas nêo no Windows. Eu cortaria os emojis decorativos e o "…". Outros pontos de quebra, de prioridade menor: `comentarios.json` ausente ou vazio gera traceback cru (FileNotFoundError/JSONDecodeError), e comentário sem a chave "texto" gera KeyError. Caminho e rede estêo ok, porque o caminho é relativo a `__file__` e nêo há rede.

**Risco (R8)**, em ordem de dano:

| Onde quebra | Por quê | Checagem que barra cedo |
|---|---|---|
| Codificaçêo do terminal (Windows cp1252, pipe, CI) | `print` de emoji fora do charset | `sys.stdout.reconfigure(errors="replace")` |
| Dado real com emoji | o 🤔 do comentário quebra mesmo sem os emojis fixos | a mesma linha acima, que vale para qualquer texto |
| Entrada ausente/corrompida | traceback cru | `try` na leitura → mensagem curta + exit 1 |
| Comentário sem "texto" | `KeyError` | `c.get("texto", "")` |
| Caminho / rede | ok: relativo a `__file__`, sem rede | — |

**Corte (R7)**: os emojis decorativos (🔎 ❓ ✅), o "…" e a funçêo `perguntas()` de 5 linhas, que vira uma compreensêo de lista. O `carregar()` passou para dentro do `main`.

## Reproduçêo

Ambiente normal (Linux, UTF-8):

```
🔎 lendo 5 comentários…
❓ Como faço isso no Windows?
❓ Funciona com Python 3.9?
❓ Onde baixo o arquivo de exemplo? 🤔?
✅ 3 perguntas salvas em relatorio.txt
exit=0
```

`PYTHONIOENCODING=cp1252 python3 testes/aceitacao/script_emoji.py`:

```
Traceback (most recent call last):
  File ".../testes/aceitacao/script_emoji.py", line 38, in <module>
    sys.exit(main())
  File ".../testes/aceitacao/script_emoji.py", line 28, in main
    print(f"\U0001f50e lendo {len(comentarios)} coment�rios�")
  ...
UnicodeEncodeError: 'charmap' codec can't encode character '\U0001f50e' in position 0: character maps to <undefined>
exit=1
ls: cannot access 'testes/aceitacao/relatorio.txt': No such file or directory
```

**A previsêo bateu**: mesmo erro, mesma linha, exit 1, e nenhum relatório gravado.

Teste da armadilha. Com uma "meia correçêo" (sem nenhum emoji fixo, imprimindo só as perguntas) em cp1252:

```
- Como fa�o isso no Windows?
- Funciona com Python 3.9?
- exit=1
UnicodeEncodeError: 'charmap' codec can't encode character '\U0001f914' in position 33
```

Confirmado: o emoji do dado (`\U0001f914` = 🤔) também derruba. Tirar os emojis nêo resolve.

Bordas, no original: sem `comentarios.json` → `FileNotFoundError` cru. Comentário sem "texto" (`[{"autor":"x"}]`) → `KeyError: 'texto'`, exit 1.

## Correçêo

Arquivo novo ao lado: `testes/aceitacao/script_emoji.revisado.py`. O original nêo mudou. Sêo 29 linhas contra 38.

| Mudou | Por quê |
|---|---|
| `sys.stdout.reconfigure(errors="replace")` | resolve a codificaçêo na saída, para qualquer texto (fixo **ou** do dado); o caractere sem mapa vira `?` |
| saem 🔎 ❓ ✅ e "…" | enfeite que nêo paga o risco (R7) |
| `try` em volta da leitura (`OSError`, `ValueError`) → `erro ao ler …`, exit 1 | falhar cedo e claro na entrada (R8) |
| `c.get("texto", "")` | comentário incompleto nêo derruba o lote |
| `perguntas()` + `carregar()` viram 1 compreensêo dentro do `main` | menos superfície, mesma regra (termina em "?") |
| `relatorio.txt` continua `"\n".join(...)` em UTF-8 | a saída útil fica **byte a byte** igual |

**Custo (R4)**: correçêo em script puro, só com stdlib. Nêo precisa de modelo nem de API.

## Lado a lado

Mesma entrada (`comentarios.json`), ambiente normal:

```
orig exit=0
rev exit=0
--- out.rev:
lendo 5 comentários
- Como faço isso no Windows?
- Funciona com Python 3.9?
- Onde baixo o arquivo de exemplo? 🤔?
3 perguntas salvas em relatorio.txt
--- perguntas (sem o marcador) iguais?
IGUAIS
--- relatorio.txt iguais?
IDENTICOS
528019359831db71
528019359831db71
```

Onde o original quebrou (cp1252), o revisado passa:

```
lendo 5 coment�rios
- Como fa�o isso no Windows?
- Funciona com Python 3.9?
- Onde baixo o arquivo de exemplo? ??
3 perguntas salvas em relatorio.txt
exit=0
relatorio cp1252 IDENTICO
```

A tela degrada (🤔 → `?`), mas o arquivo continua em UTF-8 e idêntico ao do original.

Bordas:

| Caso | Original | Revisado |
|---|---|---|
| sem `comentarios.json` | `FileNotFoundError` (traceback) | `erro ao ler comentarios.json: [Errno 2] ...`, exit 1 |
| `[{"autor":"x"}]` (sem "texto") | `KeyError: 'texto'`, exit 1 | `lendo 1 comentários` / `0 perguntas salvas`, exit 0 |

Uma previsêo minha **errou**: achei que a cópia em `.mentor/tmp/` nêo teria entrada. Havia lá um `comentarios.json` de outra rodada (`[{"autor":"x"}]`), e o resultado foi 0 perguntas com exit 0. Repeti numa pasta vazia (`semdados/`) e aí deu o erro previsto. O engano virou prova extra do caso "sem texto".

Nêo conferido: Windows de verdade. Simulei com `PYTHONIOENCODING=cp1252`. No console interativo do Windows (Python ≥3.6) o original costuma funcionar; a quebra aparece com redirecionamento, pipe ou agendador.

## Relatório

| Passo | O que rodou | O que saiu | Regra |
|---|---|---|---|
| 1 | `cat` do script, dos dados, das regras e do validador (só leitura) | achou emoji no texto fixo **e** no dado | R7, R8 |
| 2 | `Previsêo:` escrita antes de executar | UnicodeEncodeError em `\U0001f50e`, exit 1, sem relatório | R3, R2 |
| 3 | original normal + `PYTHONIOENCODING=cp1252` | exit 0 / `UnicodeEncodeError`, exit 1, sem `relatorio.txt`: **acertou** | R1 |
| 4 | meia correçêo (sem emoji fixo) em cp1252 | quebra em `\U0001f914` (dado) | R1, R8 |
| 5 | escreveu `script_emoji.revisado.py` (29 linhas) | `reconfigure(errors="replace")`, leitura protegida, `.get` | R7, R8, R4 |
| 6 | lado a lado, entrada igual | perguntas IGUAIS; `relatorio.txt` IDENTICOS (sha 528019359831db71) | R1, R3 |
| 7 | revisado em cp1252 | exit 0, relatório idêntico | R1 |
| 8 | bordas: sem entrada / sem "texto" | original: traceback / KeyError; revisado: erro claro / 0 perguntas | R8 |
| 9 | `python3 tools/validar_resposta.py ... --perfil revisao` | `OK: resposta seguiu o loop`, exit=0 | R1 |

**Sua vez (R6)**: rode `PYTHONIOENCODING=ascii python3 testes/aceitacao/script_emoji.revisado.py`. Antes, preveja quantos `?` aparecem na linha do Windows. Depois, num script seu que imprime dado de usuário, procure todo `print` e pergunte se ele aguenta um emoji.

**Revisêo (R9)**: para uma 2ª passada, entregue a outro modelo, só para leitura, os **dois arquivos** e esta saída de terminal, nêo este resumo.

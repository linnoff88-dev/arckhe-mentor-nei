## Pronto
Você quer um hook Stop que nêo deixe o turno terminar enquanto o `TODO.md` tiver linhas `- [ ]`. Antes de escrever o código, definimos o que é sucesso e o que é aborto (R2).

Suposições (nêo estavam no pedido):
- O `TODO.md` fica na raiz do projeto (`$CLAUDE_PROJECT_DIR`).
- Se o arquivo nêo existir, o turno termina normalmente.
- O hook bloqueia no máximo 3 vezes seguidas por sessêo. Esse é o critério de aborto.

Critérios verificáveis:
1. Com itens `- [ ]` abertos, o hook imprime `{"decision":"block","reason":...}` com a lista dos itens. Com tudo `[x]`, ou sem o arquivo, nêo imprime nada e sai com exit 0.
2. Na 4ª tentativa seguida ele libera e avisa no stderr. Assim a sessêo nunca fica presa.
3. Numa sessêo real do Claude Code, o Claude é obrigado a fechar os itens antes de parar (R1).

Para manter o hook isolado, montei tudo numa sandbox separada: `exemplos/hook-todo-stop/`. Assim ele nêo se empilha no portêo que este projeto já usa (R8).

## Menor versêo
O contrato do evento Stop é este: o Claude Code manda um JSON pelo stdin (`session_id`, `cwd`, `stop_hook_active`). Se o script imprimir `{"decision":"block","reason":"..."}`, o turno nêo termina e o `reason` volta para o Claude como instruçêo. Para isso basta um script Python que só usa a biblioteca padrêo, sem LLM e sem gastar tokens (R4, R7).

`.claude/settings.json` (o evento Stop nêo leva `matcher`):
```json
{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          { "type": "command", "command": "python3 \"$CLAUDE_PROJECT_DIR/.claude/hooks/stop_todo.py\"", "timeout": 10 }
        ]
      }
    ]
  }
}
```

A v0 ingênua, `.claude/hooks/stop_todo_v0.py`:
```python
#!/usr/bin/env python3
"""v0 (ingênua): bloqueia o Stop enquanto TODO.md tiver '- [ ]'. SEM trava anti-loop."""
import json, os, re, sys
from pathlib import Path

ABERTO = re.compile(r"^\s*[-*]\s+\[ \]\s*(.*)$")

evento = json.load(sys.stdin)
raiz = Path(os.environ.get("CLAUDE_PROJECT_DIR") or evento.get("cwd") or ".")
todo = raiz / "TODO.md"
if not todo.exists():
    sys.exit(0)
abertos = [m.group(1) for l in todo.read_text(encoding="utf-8").splitlines() if (m := ABERTO.match(l))]
if abertos:
    print(json.dumps({"decision": "block",
                      "reason": f"TODO.md ainda tem {len(abertos)} item(ns) aberto(s): "
                                + "; ".join(abertos) + ". Termine e marque com [x]."}))
sys.exit(0)
```

A regex pega subitens indentados (`  - [ ]`) e também `* [ ]`. Nêo pega `[x]` nem `[X]`.

O `TODO.md` de teste:
```markdown
# TODO

- [ ] criar ok.txt
- [x] item já feito
  - [ ] subitem indentado
```

## Previsêo
Escrita antes de rodar (R3):
- **Caso 1, o TODO acima:** bloqueia e lista 2 itens, `criar ok.txt` e `subitem indentado`. Exit 0, com o JSON no stdout.
- **Caso 2, sem `TODO.md`:** nada no stdout, exit 0.
- **Caso 3, só `- [x]` e `- [X]`:** nada no stdout, exit 0.
- **Ponta a ponta com `claude -p "diga apenas oi"`:** o hook barra o fim do turno, o Claude cria o `ok.txt` e marca os itens com `[x]`.

## Saída real
Os três casos da v0:
```
== caso 1: 2 abertos
{"decision": "block", "reason": "TODO.md ainda tem 2 item(ns) aberto(s): criar ok.txt; subitem indentado. Termine e marque com [x]."}
exit=0
== caso 2: sem TODO.md
exit=0
== caso 3: tudo [x]
exit=0
```
Bateu com a previsêo nos três casos.

Na sessêo real (`claude -p "diga apenas oi"` na sandbox, pela assinatura, com a versêo com teto ativa no `settings.json`), o pedido era só um "oi", mas o hook obrigou o Claude a resolver o TODO:
```
Criei o `ok.txt` com o texto "feito" e conferi que ele existe. Marquei os dois itens com `[x]` no `TODO.md`.
== exit=0
== ok.txt:
feito
== TODO.md:
- [x] criar ok.txt com o texto "feito"
- [x] item já feito
  - [x] subitem: conferir que ok.txt existe
```
Bateu com a previsêo (R1: conferimos o arquivo, nêo só a fala do agente).

## Versêo quebrada
O cenário de quebra é um item que o agente nêo consegue fechar sozinho, como "aguardar aprovaçêo do cliente". Simulei 6 Stops seguidos. Do segundo em diante vai `stop_hook_active=true`, que é o que o Claude Code manda quando um hook já bloqueou antes.

Previsêo: a v0 ignora esse sinal e bloqueia sempre.
```
tentativa 1 active=false -> "decision": "block"
tentativa 2 active=true -> "decision": "block"
tentativa 3 active=true -> "decision": "block"
tentativa 4 active=true -> "decision": "block"
tentativa 5 active=true -> "decision": "block"
tentativa 6 active=true -> "decision": "block"
```
**Causa:** a v0 nêo tem critério de aborto. Se um item nêo pode ser fechado, a sessêo gira para sempre, gastando turnos (R2, R4).

Também testei um TODO salvo no Windows, com finais de linha CRLF (`\r\n`). Eu esperava que quebrasse, mas nêo quebrou: `splitlines()` já remove o `\r`, e a contagem de `\r` na saída deu `0`. É um medo a menos, confirmado pela mediçêo e nêo por suposiçêo (R3).

**Correçêo:** `.claude/hooks/stop_todo.py` ganha um contador por sessêo e um teto. Qualquer erro interno sai com exit 0, para o hook nunca derrubar a sessêo (R8).
```python
#!/usr/bin/env python3
"""Hook Stop: nêo deixa o turno terminar enquanto TODO.md tiver itens '- [ ]'.

Contrato: lê o JSON do evento no stdin; para bloquear imprime {"decision":"block","reason":...}.
Critério de aborto: no máximo TODO_STOP_MAX bloqueios seguidos por sessêo (padrêo 3);
depois libera e avisa no stderr. Zerou os abertos -> zera o contador.
Qualquer erro interno -> exit 0 (o hook nunca prende a sessêo).
"""
import json, os, re, sys
from pathlib import Path

ABERTO = re.compile(r"^\s*[-*]\s+\[ \]\s*(.*)$")


def main() -> int:
    evento = json.load(sys.stdin)
    raiz = Path(os.environ.get("CLAUDE_PROJECT_DIR") or evento.get("cwd") or ".")
    todo = raiz / "TODO.md"
    sessao = re.sub(r"[^A-Za-z0-9_.-]", "_", evento.get("session_id") or "sem-sessao")
    cont = raiz / ".claude" / "todo-stop" / f"{sessao}.count"
    maximo = int(os.environ.get("TODO_STOP_MAX", "3"))

    linhas = todo.read_text(encoding="utf-8").splitlines() if todo.exists() else []
    abertos = [m.group(1) for l in linhas if (m := ABERTO.match(l))]
    if not abertos:
        cont.unlink(missing_ok=True)
        return 0

    n = int(cont.read_text()) if cont.exists() else 0
    if n >= maximo:
        cont.unlink(missing_ok=True)
        print(f"[stop_todo] liberado após {maximo} bloqueios; ainda abertos: {'; '.join(abertos)}", file=sys.stderr)
        return 0

    cont.parent.mkdir(parents=True, exist_ok=True)
    cont.write_text(str(n + 1))
    print(json.dumps({"decision": "block",
                      "reason": f"TODO.md tem {len(abertos)} item(ns) aberto(s) (bloqueio {n + 1}/{maximo}): "
                                + "; ".join(abertos)
                                + ". Termine cada um e marque com [x]. Se algum nêo depende de você, "
                                  "explique por que e deixe aberto."}))
    return 0


if __name__ == "__main__":
    try:
        sys.exit(main())
    except Exception:
        sys.exit(0)
```

Previsêo: 1/3, 2/3 e 3/3, a 4ª libera e a 5ª recomeça em 1/3. Com stdin que nêo é JSON, ou com tudo `[x]`: exit 0 e nada no stdout.

Saída real:
```
tentativa 1 -> bloqueio 1/3
tentativa 2 -> bloqueio 2/3
tentativa 3 -> bloqueio 3/3
tentativa 4 -> liberado após 3 bloqueios
tentativa 5 -> bloqueio 1/3
== stdin lixo
exit=0
== fechou tudo
exit=0 saida_vazia
```
Bateu com a previsêo. O loop infinito virou no máximo 3 insistências, e depois o hook libera.

**Ao levar para o seu projeto (R8):**
- Ponha `.claude/todo-stop/` no `.gitignore`.
- Se o projeto já tiver outro Stop hook, saiba que os dois rodam juntos. Basta um bloquear para o turno continuar.

**Por baixo (R5):** o mecanismo é o mesmo de qualquer gate de CI. Um programa lê o evento, decide e responde por código de saída ou por JSON. Mudam a ferramenta e o nome do evento, mas o padrêo continua: ler a entrada, decidir e responder num formato combinado.

## Sua vez
No seu próprio projeto (R6):
1. Copie a pasta `.claude/` da sandbox e crie um `TODO.md` com 2 itens.
2. Mude a regex para ignorar `- [ ]` que estejam dentro de blocos de código (entre ` ``` ` e ` ``` `). Antes de rodar, escreva quantos itens você espera que o hook liste.
3. Rode `echo '{"session_id":"x"}' | CLAUDE_PROJECT_DIR=$PWD python3 .claude/hooks/stop_todo.py` e compare com a sua previsêo.
4. Critério de pronto: o item dentro do bloco de código nêo aparece no `reason`, e os itens de fora aparecem. Você tem 3 tentativas. Se nêo fechar, volte para a versêo que funcionava e escreva o que travou.

## Relatório
| Passo | O que rodou | O que saiu | Regra que guiou |
|---|---|---|---|
| Definir pronto e aborto | escrito antes do código | 3 critérios, teto de 3 bloqueios | R2 |
| Menor versêo (v0) | 1 script stdlib + 5 linhas de settings | hook mínimo, zero tokens por checagem | R7, R4 |
| Previsêo | escrita antes de cada execuçêo | números concretos (2 itens, 1/3…4ª libera) | R3 |
| Casos da v0 | 3 entradas via stdin | block com 2 itens / exit 0 / exit 0 | R1 |
| Entrada ruim | stdin nêo-JSON, arquivo ausente, CRLF | exit 0; CRLF nêo quebra (0 `\r`) | R8, R3 |
| Versêo quebrada | 6 Stops com `stop_hook_active=true` na v0 | 6 blocks, loop sem fim | R2 |
| Correçêo | mesmo laço na versêo com teto | libera na 4ª, reinicia na 5ª | R2, R8 |
| Prova real | `claude -p "diga apenas oi"` na sandbox | `ok.txt` = "feito", 3 itens `[x]` | R1 |
| Fundamento | resposta por exit code / JSON | mesmo padrêo de gate de CI | R5 |
| Exercício | regex ignorando blocos de código | no projeto do aluno | R6 |

Arquivos:
- `exemplos/hook-todo-stop/.claude/hooks/stop_todo.py`
- `exemplos/hook-todo-stop/.claude/hooks/stop_todo_v0.py`
- `exemplos/hook-todo-stop/.claude/settings.json`
- `exemplos/hook-todo-stop/TODO.md`

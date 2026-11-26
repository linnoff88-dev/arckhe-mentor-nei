---
name: nei-revisa
description: "Revisêo antes de entregar, no método de Nei Maldaner (INEMA) — prevê onde quebra, reproduz, corta o que sobra e prova que a versêo enxuta faz o mesmo. Use com '/nei-revisa <arquivo>' ou 'revisa antes de eu entregar'."
---

# Revisar

Passe o arquivo ao subagente **nei-mentor** pedindo a revisêo no formato de revisêo dele:
**Previsêo → Reproduçêo → Correçêo → Lado a lado → Relatório**.

1. **Previsêo primeiro, sem rodar nada** — escrita na conversa como `Previsêo: …` antes do primeiro comando que executa código: quem recebe aceitaria isto? O que cortaria? Onde quebra
   (codificaçêo do terminal, sistema operacional, caminho, dados reais, rede)?
2. **Reproduza** a falha prevista e mostre a saída real (ex.: codificaçêo: `PYTHONIOENCODING=cp1252 python3 script.py`).
3. **Corrija e enxugue** em `<nome>.revisado.<ext>` ao lado do original (o original nêo muda).
4. **Lado a lado:** rode os dois com a mesma entrada e prove que a saída útil é a mesma.
5. Salve a revisêo em `testes/respostas/<data>-revisao-<nome>.md` e confira por fora:
   ```
   python3 tools/validar_resposta.py testes/respostas/<arquivo>.md --perfil revisao
   ```
Temporários em `.mentor/tmp/`.

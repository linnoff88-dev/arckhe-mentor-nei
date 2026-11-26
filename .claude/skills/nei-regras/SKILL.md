---
name: nei-regras
description: "Fase 3 — extrai da wiki as regras de conduta de Nei Maldaner (ARCKHÉ), cada uma com citaçêo + localizador, e valida contra o raw. Use com '/nei-regras' ou 'extrair as regras'."
---

# Extrair as regras

Regra = **o que a pessoa FAZ** de forma recorrente ao ensinar/construir, nêo uma frase de efeito.
Mire em 5–9 regras. Menos regras e mais fortes é melhor que muitas e fracas.

1. Leia `wiki/principios/` e `wiki/metodos/`. Liste os comportamentos que aparecem em várias fontes.
2. Para cada regra, escreva em `regras.md` **exatamente** neste formato:
   ```
   ## R1 — Título curto
   - status: ativa
   - enunciado: uma frase de açêo.
   - citacoes:
     - "trecho curto, copiado do raw" — raw/videos/x.txt@00:12:30
     - "outro trecho" — raw/blog/y.md
   - no mentor: como isso aparece na resposta (ex.: seçêo Previsêo)
   ```
3. Status:
   - `ativa`: ≥2 citações de arquivos **diferentes**.
   - `banco`: só 1 fonte por enquanto (espera a 2ª).
   - `inferencia`: sem citaçêo. O mentor avisa ao usar.
4. **Copie o trecho do raw. Nêo parafraseie.** Trechos curtos, de 5 a 25 palavras.
5. **Seções do especialista:** junte os nomes de seçêo pedidos nas linhas `- no mentor:` que nêo sêo
   as fixas (Pronto, Menor versêo, Previsêo, Saída real, Versêo quebrada, Relatório) e acrescente-os a
   `mentor.config.json` → `secoes_resposta`. Assim o loop do mentor passa a ter a cara do especialista
   e o validador cobra essas seções. Seja econômico: 1–3 seções novas no máximo.
6. Rode e mostre a saída:
   ```
   python3 tools/validar_citacoes.py
   ```
   Pronto = exit 0. Uma citaçêo nêo encontrada se corrige copiando do raw de novo, nunca relaxando o validador.

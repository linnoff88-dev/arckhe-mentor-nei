# musicavideo — música, capa e clipe a partir de uma frase

- Tipo: guia
- Origem: https://inematds.github.io/musicavideo/guia/
- Arquivo: raw/guia/musicavideo-musica-capa-e-clipe-a-partir-de-uma-fr-b4bc8f.md

## Resumo
Guia do projeto `musicavideo`: a partir de uma frase solta, gera música, capa e clipe que conversam entre si, porque antes sai um plano comum (`plano.json`, de esquema fechado), montado com base num banco de estilos medido em material real (BPM, tom, instrumentaçêo, voz). Sêo quatro fases (pesquisa opcional → plano grátis → execuçêo por parte → PACOTE.md), com portêo por parte (ver → ajusta → ok → faz), estado retomável em `estado.json`, custo estimado na tela antes de qualquer chamada, teto de gasto e provedores plugáveis (adapter + models.json). Capa e clipe custam US$ 0 (Agnes); a única parte paga é a música (Suno via Kie, cerca de US$ 0,08). Inclui painel local sempre no ar (systemd), publicaçêo no Hugging Face terminando no push, acervo pesquisável e um roadmap honesto: Kling e fal estêo a testar, ainda sem uso contra a API real.

## Ideias centrais
- O plano é o produto: "Planejar é barato. Executar é caro."
- Portêo humano por parte: "Parte sem ok nêo gera. Aprovar uma nêo aprova as outras."
- Custo visível antes de gastar: "Mostra o custo estimado por parte e o total antes de chamar qualquer API."
- Teto de gasto que para sem perder trabalho: "O teto para antes de estourar: o que coube fica pronto, o resto continua aprovado"
- Motor é dado, nêo código: "O motor mora no plano, nunca no código."
- Falha isolada: "Falha de um provedor nêo derruba a corrida"
- Grátis por padrêo: "Os defaults de capa e clipe custam US$ 0 de propósito"
- Sem chave, avisa em vez de quebrar: "Sem chave, ele aparece indisponível com o motivo — nunca estoura na hora de gerar."
- Pronto nêo é publicado: "Subir nêo é consequência de ficar pronto: produçêo pronta é material de trabalho."
- Publicar termina no push: "publicar termina no push, nêo no meio."

## Como ele ensina aqui
- Passo a passo com comandos reais e a saída esperada (o bloco de custo estimado que termina em `confirmar? [s/N]`).
- Mostra primeiro o caminho com portêo e só no fim o atalho sem portêo, com trava de gasto (`--teto`).
- Roadmap honesto separa o que está pronto (exercitado) do que está a testar (só teste de contrato): "Adapters escritos e cobertos por teste de contrato, ainda nêo exercitados contra a API real."
- Dependência mínima ("Zero dependência pip: tudo em stdlib.") e chaves lidas em runtime, nunca copiadas para o repositório.

## Ligações
- Temas: [[producao-de-video-com-ia]], [[custo-de-tokens]], [[orquestracao-de-modelos]], [[git-e-deploy]], [[llm-local-e-gratis]]
- Princípios: [[menor-custo-que-resolve]], [[verificar-antes-de-afirmar]], [[criterio-de-pronto-explicito]], [[independencia-de-plataforma]], [[simplicidade-e-corte]]
- Métodos: [[pipeline-de-video-por-assunto]], [[plano-antes-de-gastar-com-portao]], [[guia-de-projeto-landing]]

# LIVE： Bench Studio com 81 Modelos de IA - Instalaçêo do Zero

- Tipo: video
- Origem: https://www.youtube.com/watch?v=tfLpUHmpvFk
- Arquivo: raw/videos/live-bench-studio-com-81-modelos-de-ia-instalacao-do-zero.txt

## Resumo
Live de apresentaçêo e instalaçêo do Bench Studio (versêo BR, PT/EN), o estúdio de geraçêo de imagem e vídeo do Nei, que agora tem 81 modelos de vários provedores (Agnes grátis, Kie, Fal, Magnific). Ele explica a arquitetura: o estúdio web na porta 5200 é só uma casca, e a API/servidor na 8787 nêo fica exposta à internet. Explica também o cálculo de custo antes de gerar, a senha criptografada para acesso remoto, o script que abre e fecha a porta no firewall, os "modos" editáveis sem mexer no código e um MCP para outros sistemas chamarem o motor, que roda um job por vez (para paralelizar, precisa de fila). Durante a demo aparece um bug real: imagem-para-imagem falha sem a chave do Fal. Ele pede a causa raiz ao agente, corrige e testa até funcionar. Depois faz uma instalaçêo do zero numa VPS antiga, que trava em versêo de Node e Next. Resolve com o chat, sobe o Node para 24 e manda documentar o comando que funcionou nas instruções de instalaçêo. Fecha com a liçêo de que VPS desatualizada é o problema mais comum, de que é preciso validar requisitos antes e de que vale aprender o que nêo muda.

## Ideias centrais
- Ferramenta própria em vez de plataforma paga: "A ideia dela é substituir o Higfields e" (todas essas ferramentas) (@00:02:49); pensada para quem começa: "tá começando ter uma ferramenta para ser" (produtivo) (@00:52:36)
- Custo calculado antes de rodar, base para precificar: "Calculando o preço antes de tu rodar," (@00:12:48); "vou cobrar baseado nêo no meu tempo, eu" (vou cobrar em cima do custo) (@00:09:34)
- Segurança por camadas: "porque tu nêo fala com a API, tu fala" (com a página web) (@00:12:34); a API fica só para o estúdio e outras aplicações: "é a API. você nêo acessa ela, o estúdio" (@00:55:38)
- Limite de concorrência assumido e explícito: "uma limitaçêo. Ele roda um por vez." (@00:10:11); para paralelizar: "tempo, eu vou usar uma fila, né? Ou eu" (@00:10:16); a ideia é ser "que ele seja um serviço para as filas," (@01:16:56)
- Configuraçêo no lugar de código: "certo? Em vez de mexer no código, tu vai" (cadastrando nos modos) (@00:17:40)
- Depuraçêo por causa raiz: o agente responde "causa raiz confirmada. O concerto é" (pequeno) (@00:24:17), e a conclusêo: "Ou seja, o falo ali é desnecessário para" (todo mundo que nêo é Fal) (@00:24:52)
- Erro de instalaçêo vai pro chat, que é tutor: "ó, que que eu sugiro sempre fazer? Marca" (@01:01:32) "aqui e vai pro chat. Eu podia te chamar" (@01:01:41); "Entêo a, né? O chat é um tremendo de um" (tutor) (@01:04:03)
- Requisito tem que ser validado antes: "ser um pré-requisito, meu amigo." (@01:25:04); o problema mais comum: "vejo, tá? As VPS nêo estêo atualizadas e" (@01:34:52)
- Testar em ambiente zerado, porque no ambiente pronto tudo passa: "que que dá de erro, porque quando tu já" (tem tudo atualizado, tudo dá certo) (@01:56:24)
- Nêo se apaixonar por modelo; usar o melhor de cada um: "Tu vai analisar, os caras estêo tudo" (chutêo) (@01:58:20); "melhor de cada um, certo? Onde" (@01:59:28); aprender o que nêo muda: "vêo aprendendo aquilo que nêo vai mudar." (@02:01:37)

## Como ele ensina aqui
- Instala do zero na frente do público, numa VPS antiga, de propósito, para mostrar os erros que o aluno vai encontrar. Nêo usa a máquina pronta.
- Quando quebra, cola o erro e o contexto no agente, pede a causa raiz, corrige, refaz o mesmo teste ("pela milésima vez") e só declara resolvido quando a geraçêo passa ("Bingo. Resolvido.").
- Transforma o que deu certo em documentaçêo: "Agora anote que fiz isso e o teu comando" (@01:31:45). Quer log em toda execuçêo: "Certo? Gerando um log aqui. Ou seja," (@01:30:28).
- Pede ao chat comandos simples quando a resposta vem cheia de "linguiça" e explica o ganho: aprender aos poucos e nêo fazer o que nêo precisa.
- Diz o tamanho real do trabalho sem esconder: "Imagina, eu fiz isso num em dois dias," (@01:40:33). Por isso existe versêo, e o próximo passo é testar antes de liberar: "antes de eu liberar porque senêo dá esse" (@01:38:10).

## Ligações
- Temas: [[producao-de-video-com-ia]], [[infraestrutura-basica]], [[git-e-deploy]], [[mcp]], [[filas-e-workers]], [[custo-de-tokens]], [[orquestracao-de-modelos]], [[llm-local-e-gratis]]
- Princípios: [[verificar-antes-de-afirmar]], [[menor-custo-que-resolve]], [[criterio-de-pronto-explicito]], [[fundamentos-antes-de-ferramentas]], [[construir-junto-ao-vivo]], [[medir-antes-de-acreditar]]
- Métodos: [[depurar-ao-vivo]], [[app-na-vps]], [[live-construindo-ao-vivo]]

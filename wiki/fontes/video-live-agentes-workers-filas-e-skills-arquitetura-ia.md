# LIVE： Agentes, Workers, Filas e Skills - Arquitetura IA

- Tipo: video
- Origem: https://www.youtube.com/watch?v=vnSZUDjvdZQ
- Arquivo: raw/videos/live-agentes-workers-filas-e-skills-arquitetura-ia.txt

## Resumo
Live no fim de um dia do curso de 5 dias (infraestrutura → bot → filas → workers/skills/agentes → segurança e pagamentos). Ele revisa a arquitetura: o bot recebe a mensagem, um controlador põe na fila já com a lista de skills disponíveis, workers puxam da fila e disparam agentes. Dá a definiçêo de cada peça: agente é programa com IA, worker é programa sem IA, fila é base de dados, skill é *um livro, uma receita*. Defende skills aprovadas como forma de evitar alucinaçêo e ganhar segurança, mas alerta para o risco de baixar skills de origem desconhecida (há mais de 1 milhêo no skills.sh). Ao vivo, clona o repositório OKF do Google, erra o nome, procura a fonte certa e manda o agente resumir e publicar uma página no portal. Mostra materiais de pagamento (Asaas, Nubank/Pix) com a conta de taxas e impostos, o publicador de redes que levou ban do Google por falta de reautenticaçêo e fecha dizendo para aprender fundamentos e o que nêo muda, nêo a interface da ferramenta da semana.

## Ideias centrais
- Entender a estrutura para ser maestro: "estrutura e ser um maestro de uma" (@00:14:01)
- Fazer rápido sem entender nêo serve: "de forma superficial e nêo vai saber" (@00:13:46)
- Skills aprovadas na fila: "Isso faz com que ele nêo alucine, isso" (@00:15:03)
- Definições das peças: "O worker é um programa sem IA." (@00:34:39) / "programa é o skill, porque o skill é um" (@00:34:28)
- Começar pequeno: "disse? Cria uma fila, cria um worker." (@00:33:39)
- Risco de skill de terceiros: "estar instalando vírus, nós podemos" (@00:16:51)
- Skill elimina o repetitivo: "Porque todo esse trabalho repetitivo eu" (@00:24:10)
- Calcular custo antes de cobrar: "pagamento, tem que calcular esse teu" (@00:27:11)
- Separar gerador de publicador: "separar gerador de conteúdo com, tá," (@00:41:29)
- Aprender o que nêo muda: "vocês têm que aprender? O que nêo muda?" (@00:48:28)

## Como ele ensina aqui
- Explica cada peça com uma definiçêo curta e uma analogia (skill como currículo e prova do médico; maestro de orquestra), depois liga tudo num fluxo (entrada → controlador → fila → worker → agente).
- Justifica o curso de 5 dias contra o atalho de "uma hora": o objetivo é saber agir quando der problema, nêo só gerar o sistema.
- Faz ao vivo e deixa o erro aparecer: `git clone` falha, ele confere quem publicou o repositório antes de baixar (diz que nêo sabe quem é a empresa) e só entêo clona a fonte do Google.
- Põe número no custo (taxas de Pix/cartêo, 30–40% de impostos, ~R$1.000/mês de infraestrutura) e compara com outros custos de um negócio.
- Mostra a própria falha operacional (ban do Google por nêo reautenticar o publicador) como liçêo.

## Ligações
- Temas: [[filas-e-workers]], [[skills]], [[agentes-de-ia]], [[infraestrutura-basica]], [[atendimento-com-agentes]], [[producao-de-video-com-ia]], [[contexto-e-memoria]]
- Princípios: [[automatizar-o-repetitivo]], [[menor-custo-que-resolve]], [[verificar-antes-de-afirmar]], [[fundamentos-antes-de-ferramentas]]
- Métodos: [[arquitetura-bot-fila-worker-skill]], [[live-construindo-ao-vivo]]

# LIVE： Fim dos Skills？ Auditoria de Ablaçêo no Claude Code

- Tipo: video
- Origem: https://www.youtube.com/watch?v=601gW2hmP64
- Arquivo: raw/videos/live-fim-dos-skills-auditoria-de-ablacao-no-claude-code.txt

## Resumo
A live parte da fala do criador do Claude Code (*pare de usar skills e CLAUDE.md*). Ele usa a analogia estagiário × funcionário de 20 anos: modelos novos nêo precisam de passo a passo, e instruçêo prescritiva atrapalha. Ele estudou, testou com e sem, e montou um prompt de "auditoria de ablaçêo" (publicado como projeto audit-ablacao-cc). Roda o prompt em modo só-leitura no próprio sistema de produçêo. O relatório mostra que o custo real estava nas descrições de 114 skills (~14 mil tokens por sessêo), em famílias de skills "canibais" (formato-curso v1–v5), em caminhos Windows quebrados no Linux e em playbooks antigos que o modelo 5 já nêo precisa. Depois mostra outros assuntos: MCP sem estado e escalável, CLI do Cloudflare DNS, Pi.dev com modelo local, e a melhoria dos prompts do promo avatar. Nessa melhoria, um modelo forte faz a crítica e o plano, outro executa, com um commit por tarefa. Fecha com a conta de assinatura vs API e com o "modo simples" que tira os prompts do sistema.

## Ideias centrais
- Modelo novo dispensa receita de bolo: "passo a passo. E isso é um erro. Isso" (@00:04:28)
- Ele testou antes de recomendar: "comparei, testei com e sem e descobri" (@00:05:23)
- O alvo da auditoria: "do meu cloud MD e das minhas skills." (@00:06:08)
- O custo escondido está nas descrições: "está no cloud MD, mas nas descriçêo das" (@00:09:50)
- Skills sem uso continuam carregadas: "pra memória o tempo todo, mesmo que eu" (@00:11:25)
- Copiar skill entre sistemas quebra (caminho Windows no Linux): "Ou seja, eu só copiei. Ah, aquela" (@00:13:00)
- Auditar sem alterar: "auditoria, nêo alterar sem você ler," (@00:15:30)
- Validaçêo vale mais que prompt: "um prompt é ter validaçêo das regras. Ou" (@00:49:20)
- Usar a IA na preparaçêo, nêo em cada consulta: "eu uso a Iá na preparaçêo, eu usei ela" (@01:08:50)
- Calcular assinatura vs API: "Só que eu pago só 200 por mês." (@01:28:20)
- Sem os prompts, o modelo vai melhor: "se você tirar todos os promptes, o cloud" (@01:47:23)

## Como ele ensina aqui
- Mostra o próprio sistema sendo *chamado de lixo* pelo relatório, sem esconder: expõe o erro para ensinar.
- Reforça a restriçêo antes de rodar em produçêo: "será alterado, só relatório, exemplo. Eu" (@00:09:27)
- Separa planejar de executar: limpa o contexto, usa o modelo forte em esforço alto para criticar e planejar, salva o plano e executa com outro modelo em esforço médio. "melhor de cada um. Usei o Fibo para ele" (@01:03:14)
- Interage com o modelo para corrigir premissas erradas: "Veja, se a gente nêo tá interagindo, ele" (@01:02:12). Exige um commit por mudança, para poder reverter.
- Mede o custo da sessêo (1% da cota semanal) e atribui o resultado a limpar o contexto: "Entêo, tu tem que se acostumar de limpar" (@00:58:23). Faz a conta de tokens ao vivo: "isso, é nêo calcular isso aí." (@01:24:19)

## Ligações
- Temas: [[skills]], [[claude-code]], [[custo-de-tokens]], [[contexto-e-memoria]], [[mcp]], [[orquestracao-de-modelos]], [[llm-local-e-gratis]]
- Princípios: [[simplicidade-e-corte]], [[medir-antes-de-acreditar]], [[verificar-antes-de-afirmar]], [[menor-custo-que-resolve]]
- Métodos: [[auditoria-de-ablacao]], [[triagem-de-modelo-e-esforco]]

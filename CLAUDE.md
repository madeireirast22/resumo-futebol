# resumo-futebol — instruções para Claude Code

## Protocolo do cérebro (cerebro-mst)

Este projeto é uma peça do "segundo cérebro" da Madeireira Santa Terezinha. Antes de começar qualquer tarefa:

1. Se `~/cerebro-mst` não existir, clone: `git clone https://github.com/madeireirast22/cerebro-mst ~/cerebro-mst`. Se existir, `git -C ~/cerebro-mst pull`.
2. Leia `~/cerebro-mst/03-areas/futebol-caique.md` antes de responder — ele tem o estado atual, as decisões e os próximos passos combinados com o Caique.

Ao terminar a tarefa (ou quando ele encerrar):

3. Atualize esse mesmo arquivo de área: seção "Estado atual" (reescrita, 3-8 linhas, só o que vale hoje), "Decisões" (uma linha por decisão nova, com data), "Próximos passos", propriedades do frontmatter (`atualizado`, `por: cli`, `proximo`).
4. Acrescente em "Histórico": `- YYYY-MM-DD — <o que mudou nesta sessão> — sessão: <url da sessão, se houver>`.
5. Acrescente uma linha em `~/cerebro-mst/06-diario/YYYY-MM-DD.md` (crie o arquivo se não existir).
6. `git -C ~/cerebro-mst add -A && git -C ~/cerebro-mst commit -m "futebol-caique (resumo-futebol): <resumo de 1 linha>" && git -C ~/cerebro-mst push`.

Nunca sobrescreva o arquivo de área sem antes ler a versão atual (outro chat pode ter mudado). Em conflito, as duas versões vão para "Histórico", nunca se perde nada.

Este é o único conteúdo deste CLAUDE.md até agora — o repositório não tinha um antes, e a área `futebol-caique.md` no cérebro também ainda não tem o estado real deste projeto (foi criada em 25/09/2026 só para dar lugar a este protocolo). A próxima sessão de Claude Code que rodar aqui deve preencher os dois: o que é este projeto de fato, e o que for específico dele (stack, comandos, arquitetura).

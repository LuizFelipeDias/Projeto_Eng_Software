# CloudVault — Instruções para o Claude Code

O projeto usa as mesmas regras para o Copilot e para o Claude Code. Fonte única:
@.github/copilot-instructions.md

## Contexto obrigatório antes de implementar
- Requisitos: @specs/requisitos.md
- Histórias e backlog: @specs/backlog.md
- Critérios de aceitação: @docs/plano-de-teste.md
- Decisão de arquitetura: @docs/adr/ADR-001-arquitetura-camadas-localstack.md
- Fluxo de branches e PRs: @docs/convencao-versionamento.md

## Regras de trabalho
1. Uma história do backlog por sessão. Se o número da história ou os critérios de aceitação não forem informados, pergunte antes de começar.
2. TDD: escreva o teste, rode `npm test` e mostre que ele falha; só então implemente o mínimo para passar.
3. Nunca importe `@aws-sdk/*` fora de `backend/src/adapters/` (ADR-001, RNF-03).
4. Credenciais apenas fictícias (`test`/`test`, endpoint `http://localhost:4566`). Nunca leia, exiba nem commite `.env` ou o `LOCALSTACK_AUTH_TOKEN`.
5. Antes de concluir: `npm run lint` e `npm test` passando no pacote alterado.
6. Nunca faça merge, push na `main` ou `git push --force`. Trabalhe na branch da tarefa.
7. Ao terminar: liste os arquivos alterados, os comandos executados e qualquer desvio da spec, para o registro em `docs/registro-ia.md`.

## Agentes do projeto (Claude Code)
- `implementador` (`.claude/agents/implementador.md`): implementa uma história com TDD.
- `revisor` (`.claude/agents/revisor.md`): emite parecer sobre o diff, sem editar arquivos.

A decisão de integrar código é sempre de um integrante humano, que responde pelo que entra na `main`.

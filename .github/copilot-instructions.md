# Instruções do Agente — CloudVault

## Objetivo do Projeto
Sistema web de upload, processamento assíncrono e gerenciamento de arquivos simulando AWS (S3, DynamoDB, SQS) localmente via LocalStack, com back-end em Node.js (TypeScript/Express) e front-end em React. Specs em `specs/`, decisões em `docs/adr/`.

## Regra de Arquitetura (ADR-001)
* **Arquitetura em camadas:** `backend/src/controllers/` (HTTP) → `backend/src/services/` (regras de negócio) → `backend/src/adapters/` (infraestrutura LocalStack/AWS).
* **Restrição rígida:** É PROIBIDO importar `@aws-sdk/*` fora de `backend/src/adapters/`. Use sempre as interfaces de adaptadores (padrão Adapter).

## Comandos de Build, Teste e Lint
* Back-end: `cd backend && npm run lint && npm test`
* Front-end: `cd frontend && npm run lint && npm test && npm run build` (testes com Vitest)
* Infraestrutura local: `docker compose up -d`

## Convenções e Proibições
* **TDD:** Escreva o teste falhando antes de implementar a funcionalidade.
* **Nomes:** Arquivos em `kebab-case` (ex.: `file-service.ts`), classes/interfaces em `PascalCase`, variáveis/funções em `camelCase`.
* **Padrão de projeto:** Adapter para toda integração com AWS; implementações `InMemory*` para testes unitários.
* **Proibido:** Commitar segredos/chaves reais da AWS, alterar código sem testes passando ou ignorar erros de lint com `eslint-disable` sem justificativa.

## Responsabilidade
Todo código gerado por agentes de IA só é integrado após revisão humana; o revisor humano que aprova e integra o PR responde pelo código integrado na `main`.

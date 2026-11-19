# Plano de Teste — CloudVault

## 1. O que será testado
Validação de entrada e upload de arquivos (`RF-01`), persistência de metadados (`RF-02`), processamento assíncrono via fila SQS (`RF-03`), listagem/filtro (`RF-04`), download (`RF-05`), exclusão (`RF-06`) e latência de upload (`RNF-01`).

## 2. Níveis de teste
* **Unidade (Jest no back-end, Vitest no front-end):** Regras de negócio nos `Services` usando adaptadores em memória (`InMemory*`).
* **Integração/Componentes:** Comunicação real dos `Adapters` com LocalStack (S3, DynamoDB e SQS) via Docker.
* **Sistema:** Fluxo completo da API (upload → fila → worker → listagem) com Supertest.
* **Aceitação:** Validação manual no front-end React pelo PO seguindo os critérios das histórias.

## 3. Item do backlog com TDD obrigatório
* **Item #3 (H1 — Upload de arquivos e metadados):** Os testes unitários do `FileService` (arquivo válido, arquivo > 10 MB, extensão inválida e rollback em caso de falha no DynamoDB) serão escritos e executados falhando **antes** da implementação.
* **Pior candidato a TDD:** item #5 (dashboard React) — o comportamento visual do filtro é mais fácil de validar manualmente do que especificar antes em teste.

## 4. Registro dos resultados
Os resultados das execuções de integração e aceitação serão registrados em uma tabela em `docs/resultados-testes.md` contendo: **ID do Requisito**, **Cenário Testado**, **Resultado Esperado**, **Status (Aprovado/Reprovado)** e **Data**.

## 5. Cronograma previsto
* **Sprint 1:** Testes unitários (TDD) de H1 e H2 + testes de integração com LocalStack no CI.
* **Sprint 2:** Testes de H4, H5, adaptação à mudança surpresa e teste de desempenho do `RNF-01`.

---

## Tabela: Histórias Prioritárias × Critérios de Aceitação

| História | Critério de Aceitação (definido pelo grupo) | Tipo | Automatizável? |
| :--- | :--- | :--- | :--- |
| **H1 (Upload)** | Upload de PDF de 2 MB retorna HTTP `201`, grava no S3 e cria registro `PENDENTE` no DynamoDB. | Funcional | Sim |
| **H1 (Upload)** | Upload de arquivo `.exe` ou > 10 MB retorna HTTP `400` sem gravar no S3/DynamoDB. | Funcional | Sim |
| **H1 (Upload)** | Tempo de resposta do `POST /api/files` é ≤ 1.500 ms (p95) para arquivos de 5 MB (`RNF-01`). | Não Funcional | Sim |
| **H2 (Fila SQS)** | Após upload, uma mensagem com `fileId` é publicada na fila SQS e o worker atualiza o status para `PROCESSADO`. | Funcional | Sim |
| **H2 (Fila SQS)** | Se o arquivo não existir no S3 quando o worker processar, o status é atualizado para `ERRO`. | Funcional | Sim |
| **H3 (Listagem)** | A tela exibe nome, tamanho e status de todos os arquivos cadastrados no DynamoDB. | Funcional | Sim |
| **H3 (Listagem)** | Selecionar o filtro `PROCESSADO` exibe apenas arquivos com esse status. | Funcional | Não (manual/UI) |

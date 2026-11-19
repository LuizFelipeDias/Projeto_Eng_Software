# Mini-Spec: Do Ticket Vago à Especificação Verificável

## Ticket vago original
> "Fazer upload de arquivos na nuvem"

## Mini-spec reescrita

* **Objetivo e para quem:** Permitir que estudantes e membros de laboratórios enviem arquivos (PDF, PNG, JPG ou TXT) pela interface web do CloudVault, armazenando o binário no S3 (LocalStack), gravando metadados no DynamoDB e disparando processamento assíncrono via SQS.
* **Critérios de aceitação verificáveis:**
  1. Enviar um arquivo `.pdf` de 2 MB via `POST /api/files` retorna HTTP `201` com `{ id, filename, status: "PENDENTE" }`.
  2. Enviar um arquivo maior que 10 MB ou com extensão não permitida (ex.: `.exe`) retorna HTTP `400` com mensagem clara de validação e **não** grava nada no S3 nem no DynamoDB.
  3. Após upload válido, o objeto existe no bucket S3 configurado e uma mensagem contendo o `fileId` é enfileirada na fila SQS.
* **RNF mensurável associado:** `RNF-01` — O endpoint `POST /api/files` deve responder em até **1.500 ms** (p95) para arquivos de até 5 MB em ambiente local, delegando validações pesadas à fila SQS.
* **Contexto de arquitetura:** Segue o `ADR-001` (Arquitetura em Camadas + padrão Adapter). O `FileController` valida a entrada HTTP e chama `FileService`; o `FileService` orquestra o fluxo chamando as interfaces `StorageAdapter`, `MetadataRepository` e `QueueAdapter` (nunca o `@aws-sdk/*` diretamente).
* **Armadilha (ponto de falha comum):** Salvar o arquivo no S3 e falhar ao gravar no DynamoDB, deixando arquivos "órfãos" no bucket sem registro no banco. **Mitigação:** se a gravação no DynamoDB falhar, o `FileService` deve executar rollback removendo o objeto recém-criado no S3 antes de retornar erro `500`. Se a publicação na SQS falhar depois do DynamoDB, o registro permanece `PENDENTE` e é marcado para reprocessamento, sem apagar o arquivo.

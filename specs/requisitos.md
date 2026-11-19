# Especificação de Requisitos — CloudVault

## Requisitos Funcionais (RF)

* **RF-01 — Upload de arquivo para nuvem simulada (S3):** O sistema deve permitir o envio de arquivos (PDF, PNG, JPG, TXT) de até 10 MB via interface web, armazenando-os em um bucket S3 no LocalStack.
  * *Critério de aceitação:* Ao enviar um arquivo válido, a API retorna status `201 Created` com ID único e o objeto existe no bucket S3 do LocalStack.
* **RF-02 — Registro de metadados (DynamoDB):** O sistema deve salvar os metadados de cada arquivo (ID, nome original, tamanho, tipo MIME, data de upload e status `PENDENTE`) em uma tabela DynamoDB no LocalStack.
  * *Critério de aceitação:* Imediatamente após o upload, uma consulta na tabela DynamoDB pelo ID retorna todos os metadados preenchidos com status inicial `PENDENTE`.
* **RF-03 — Processamento assíncrono via fila (SQS):** O sistema deve publicar uma mensagem na fila SQS após o upload para que um worker valide a integridade do arquivo e atualize seu status para `PROCESSADO` ou `ERRO`.
  * *Critério de aceitação:* Quando a mensagem é consumida pelo worker, o status do arquivo no DynamoDB muda de `PENDENTE` para `PROCESSADO` (ou `ERRO`, se o objeto não existir no S3) sem bloquear a resposta HTTP do upload.
* **RF-04 — Listagem e filtro de arquivos:** O front-end deve exibir a lista de arquivos enviados com nome, tamanho, data e status atual, permitindo filtrar por status (`TODOS`, `PENDENTE`, `PROCESSADO`, `ERRO`).
  * *Critério de aceitação:* A tela lista os registros retornados pela API e, ao selecionar um status no filtro, exibe apenas os arquivos correspondentes.
* **RF-05 — Download de arquivo:** O usuário deve poder baixar qualquer arquivo com status `PROCESSADO` diretamente pela interface web.
  * *Critério de aceitação:* Ao clicar em "Baixar", o sistema gera uma URL pré-assinada (ou stream do S3) e o arquivo baixado possui conteúdo idêntico ao original.
* **RF-06 — Exclusão de arquivo:** O usuário deve poder excluir um arquivo, removendo tanto o objeto no S3 quanto o registro no DynamoDB.
  * *Critério de aceitação:* Após confirmar a exclusão, o arquivo deixa de aparecer na listagem e retorna `404 Not Found` se consultado novamente.

## Requisitos Não Funcionais (RNF)

* **RNF-01 — Desempenho de resposta do upload (mensurável):** A API de upload (`POST /api/files`) deve responder em até **1.500 ms** no percentil 95 para arquivos de até 5 MB em ambiente local com LocalStack.
  * *Critério de aceitação:* Em teste automatizado com 20 requisições sequenciais de arquivos de 5 MB, 95% das respostas retornam em ≤ 1.500 ms.
* **RNF-02 — Portabilidade de ambiente local (mensurável):** Toda a infraestrutura de nuvem simulada (LocalStack com bucket S3, tabela DynamoDB e fila SQS) deve subir com um único comando (`docker compose up -d`) e estar pronta para uso em até **60 segundos**.
  * *Critério de aceitação:* Executar o comando em máquina limpa com Docker sobe o container saudável e cria automaticamente os 3 recursos AWS simulados em até 60 s.
* **RNF-03 — Desacoplamento de provedor de nuvem:** O código de regras de negócio (`services`) não pode depender diretamente do SDK da AWS, devendo usar interfaces/adaptadores para facilitar testes e manutenção.
  * *Critério de aceitação:* Nenhum arquivo dentro de `backend/src/services/` contém `import` de `@aws-sdk/*`.
* **RNF-04 — Segurança e ausência de segredos:** Nenhuma credencial real da AWS pode ser versionada; o sistema deve usar exclusivamente variáveis de ambiente documentadas em `.env.example`.
  * *Critério de aceitação:* Varredura no repositório confirma ausência de chaves reais e funcionamento completo usando credenciais fictícias (`test`/`test`).

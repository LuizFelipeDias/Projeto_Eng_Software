# Histórias de Usuário e Backlog Priorizado

## Histórias de Usuário

* **H1 (Luiz Felipe):** Como estudante, quero enviar arquivos pela interface web para armazená-los com segurança na nuvem simulada.
  * *Fluxo principal:* Seleciona arquivo → clica em Enviar → vê confirmação e arquivo listado como `PENDENTE`.
  * *Fluxo alternativo:* Arquivo > 10 MB ou tipo inválido → exibe alerta de erro sem enviar.
  * *Requisitos:* RF-01, RF-02, RNF-01.
* **H2 (Luiz Felipe):** Como usuário do laboratório, quero que meus arquivos sejam validados em segundo plano via fila para não ficar travado esperando o processamento.
  * *Fluxo principal:* Upload publica na SQS → worker consome mensagem → atualiza status no DynamoDB para `PROCESSADO`.
  * *Fluxo alternativo:* Objeto não encontrado no S3 → worker atualiza status para `ERRO`.
  * *Requisitos:* RF-03, RNF-01, RNF-03.
* **H3 (Gustavo):** Como estudante, quero visualizar todos os meus arquivos enviados e filtrar por status para acompanhar o que já foi processado.
  * *Fluxo principal:* Acessa dashboard → vê tabela de arquivos → seleciona filtro `PROCESSADO`.
  * *Fluxo alternativo:* Nenhum arquivo com o status escolhido → exibe mensagem "Nenhum arquivo encontrado".
  * *Requisitos:* RF-04.
* **H4 (Gustavo):** Como estudante, quero baixar um arquivo já processado para utilizá-lo em meus estudos.
  * *Fluxo principal:* Clica em "Baixar" em um item `PROCESSADO` → download inicia automaticamente.
  * *Fluxo alternativo:* Item ainda `PENDENTE` → botão de download fica desabilitado.
  * *Requisitos:* RF-05.
* **H5 (Samuel):** Como usuário, quero excluir arquivos que não preciso mais para manter meu espaço organizado.
  * *Fluxo principal:* Clica em "Excluir" → confirma modal → arquivo some da lista e é removido do S3 e DynamoDB.
  * *Fluxo alternativo:* Cancela o modal → nada é removido.
  * *Requisitos:* RF-06.
* **H6 (Samuel):** Como desenvolvedor da equipe, quero subir todo o ambiente LocalStack (S3, SQS, DynamoDB) com um único comando Docker para testar o sistema sem configuração manual.
  * *Fluxo principal:* Roda `docker compose up -d` → script de init cria bucket, tabela e fila automaticamente.
  * *Requisitos:* RNF-02, RNF-04.

## Backlog Priorizado (conduzido pelo Product Owner)

| # | Descrição | Tipo | Sprint 1? |
| :--- | :--- | :--- | :--- |
| 1 | Configurar Docker Compose com LocalStack + script de criação automática de S3, DynamoDB e SQS (H6) | Arquitetura | Sim |
| 2 | Estruturar back-end em camadas com interfaces Adapter para AWS SDK v3 e pipeline de CI (testes + lint) | Arquitetura | Sim |
| 3 | Implementar upload de arquivos para S3 e gravação de metadados no DynamoDB (H1) | História | Sim |
| 4 | Implementar publicação na fila SQS e worker assíncrono de atualização de status (H2) | História | Sim |
| 5 | Criar dashboard React com listagem e filtro de arquivos por status (H3) | História | Sim |
| 6 | Implementar download de arquivos processados via S3 (H4) | História | Não (Sprint 2) |
| 7 | Implementar exclusão sincronizada de arquivos no S3 e DynamoDB (H5) | História | Não (Sprint 2) |
| 8 | Documentar guia de execução local, variáveis de ambiente e coleção de endpoints no README | Documentação | Não (Sprint 2) |

## Cobertura dos requisitos
Cada RF e RNF da [spec](requisitos.md) está ligado a pelo menos uma história: RF-01/RF-02 (H1), RF-03 (H2), RF-04 (H3), RF-05 (H4), RF-06 (H5), RNF-01 (H1, H2), RNF-02 e RNF-04 (H6), RNF-03 (H2 e item 2).

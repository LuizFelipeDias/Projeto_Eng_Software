# Arquitetura do Sistema — CloudVault

Decisão registrada em [ADR-001](adr/ADR-001-arquitetura-camadas-localstack.md).

## Diagrama de Blocos

```mermaid
flowchart LR
    subgraph Cliente
        FE[Front-end React<br/>Dashboard & Upload]
    end

    subgraph Servidor [Back-end Node.js - Camadas]
        CTRL[Controllers / Routes<br/>Validação HTTP]
        SVC[Services<br/>Regras de Negócio]
        ADP[Adapters AWS SDK v3<br/>S3 / DynamoDB / SQS]
        WRK[Worker SQS<br/>Processamento Assíncrono]
    end

    subgraph NuvemLocal [Docker: LocalStack]
        S3[(AWS S3<br/>Arquivos)]
        DDB[(DynamoDB<br/>Metadados)]
        SQS[[AWS SQS<br/>Fila de Eventos]]
    end

    FE -->|HTTP REST| CTRL
    CTRL --> SVC
    SVC --> ADP
    ADP -->|Upload/Download| S3
    ADP -->|CRUD Metadados| DDB
    ADP -->|Enfileira Evento| SQS
    SQS -->|Consome Mensagem| WRK
    WRK -->|Atualiza Status| DDB
```

## O que o diagrama não diz
Conforme Sommerville (2019, p. 149–150), o diagrama de blocos mostra a estrutura macro e o fluxo principal, mas não especifica:
* Os contratos exatos de dados (schemas JSON das requisições, respostas e mensagens da fila SQS).
* O tratamento de falhas e estratégias de rollback (ex.: o que ocorre se o S3 gravar o arquivo e o DynamoDB falhar).
* Detalhes de concorrência, limites de tamanho de arquivo (10 MB) e latência máxima (RNF-01).
* Políticas de autenticação, CORS e variáveis de ambiente.
* Que o worker também passa pela camada de Adapters (e não acessa o SDK diretamente) — a seta `WRK → DDB` é simplificação.

# ADR-001: Arquitetura em Camadas com Padrão Adapter para Integração ao LocalStack

* **Status:** Aceito
* **Data:** 2026-11-19

## Contexto
O CloudVault precisa integrar um back-end Node.js a três serviços simulados da AWS no LocalStack (S3, DynamoDB e SQS) e a um front-end React. O time tem 3 integrantes, duas sprints de 2 semanas e precisa garantir testes automatizados rápidos (TDD) sem acoplar a regra de negócio ao SDK da AWS.

**Requisitos Não Funcionais atendidos:**
1. **[RNF-03](../../specs/requisitos.md) (Desacoplamento):** A camada de serviços não pode importar diretamente o `@aws-sdk/*`.
2. **[RNF-02](../../specs/requisitos.md) (Portabilidade):** O sistema deve rodar localmente com LocalStack e poder migrar para AWS real apenas trocando variáveis de ambiente.

## Decisão
Adotaremos o estilo de **Arquitetura em Camadas** no back-end (`Routes/Controllers` → `Services` → `Adapters/Repositories`), combinado ao estilo **Cliente-Servidor** entre front-end React e API REST Node.js.

**Padrão de projeto pretendido:** **Adapter** (GoF).
* **Problema:** O AWS SDK v3 possui interfaces verbosas e específicas de infraestrutura que dificultam testes unitários isolados.
* **Solução:** Criar interfaces de domínio (`StorageAdapter`, `MetadataRepository`, `QueueAdapter`) com implementações concretas para LocalStack/AWS e implementações em memória (`InMemory*`) para testes unitários.
* **Consequência:** Testes unitários rodam em milissegundos sem precisar do Docker ligado; testes de integração validam os adaptadores reais contra o LocalStack.

## Consequências e Mitigação de Desvantagens
* **Desvantagem do estilo em camadas (Sommerville, 2019, p. 157):** Separação rígida entre camadas pode introduzir sobrecarga de desempenho (múltiplas passagens de dados) e código repetitivo (boilerplate).
* **Mitigação adotada pelo grupo:** Manteremos apenas 3 camadas enxutas (Controller, Service e Adapter), evitando camadas intermediárias desnecessárias (como DTOs redundantes em operações simples) e usando streams no upload de arquivos para preservar o tempo de resposta do `RNF-01`.
* **Desvantagem do estilo cliente-servidor (Sommerville, 2019):** o back-end é ponto único de falha e há dependência da rede. Como o sistema roda localmente, o risco é aceito nesta fase.

## Regra para os Agentes de IA
> "Arquitetura em camadas: Controllers só tratam HTTP; Services contêm regras de negócio e NUNCA importam `@aws-sdk/*`; todo acesso ao S3, DynamoDB e SQS deve ocorrer exclusivamente na pasta `backend/src/adapters/`."

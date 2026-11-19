# CloudVault — Plataforma de Armazenamento e Processamento em Nuvem Local

## Tema e Escopo
- **Para quem:** estudantes e laboratórios acadêmicos que precisam armazenar, validar e gerenciar arquivos digitais.
- **Qual problema:** centralizar uploads com processamento assíncrono e rastreabilidade de metadados em uma arquitetura de nuvem simulada localmente com LocalStack (AWS S3, SQS e DynamoDB), sem custos de nuvem real.
- **O que não entra:** cobrança/faturamento, edição online de arquivos e deploy em conta AWS paga.

**Linguagem principal:** Node.js (back-end em TypeScript/Express + AWS SDK v3), com front-end em React.

## Equipe

| Nome completo | GitHub | Papel | O que cobre bem | Justificativa do papel |
| :--- | :--- | :--- | :--- | :--- |
| Luiz Felipe Dias de Araujo | [@LuizFelipeDias](https://github.com/LuizFelipeDias) | Arquiteto Técnico / Dev | Programar, arquitetura, infraestrutura | Possui experiência prévia com Node.js, React, Docker e arquitetura de sistemas. |
| Gustavo Mioduski Rodrigues | [@gustavo-github](https://github.com/gustavomioduski11) | Product Owner / QA | Negociar, documentar, testar | Facilidade em estruturar requisitos, priorizar valor e validar critérios de aceitação. |
| Samuel da Rocha | [@samuel-github](https://github.com/samuel-github) | Scrum Master / Dev | Programar, testar, facilitar processo | Boa organização de fluxo de trabalho, versionamento e remoção de impedimentos. |

*Cobertura de perfis:* o grupo cobre os quatro perfis recomendados (negociar, programar, testar e documentar). Como somos 3 integrantes, os testes e a documentação são compartilhados entre PO e Scrum Master, enquanto o desenvolvimento é dividido entre Arquiteto e Scrum Master.

## Definição de Pronto (DoD)
Uma história ou tarefa só é considerada pronta quando atender a todos os critérios abaixo:
1. Todos os critérios de aceitação definidos na história foram verificados e atendidos.
2. Os testes automatizados (unitários e de integração) passam integralmente na pipeline de CI.
3. O código passa sem erros nas regras de lint configuradas no repositório.
4. O Pull Request foi revisado e aprovado por pelo menos um integrante humano diferente do autor.
5. Nenhuma regra arquitetural do `ADR-001` foi violada (verificado pelo agente revisor e pelo revisor humano).
6. O uso de agentes de IA na tarefa foi registrado em `docs/registro-ia.md`.

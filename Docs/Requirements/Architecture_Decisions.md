# Decisões de Arquitetura — SmartLab

## Decisão 1 — Modelo Arquitectural Inicial
- Escolha: Modular Monolith com API central.
- Justificativa: rapidez de desenvolvimento, menor TCO inicial, facilidade de certificação e auditoria.
- Critério de mudança: migrar para microservices quando um módulo justificar escalabilidade independente ou autonomia tecnológica (ex.: Analytics, AI Workers, Training Hub).

## Decisão 2 — API Central como Single Source of Truth
- Todos os módulos comunicam apenas com a API central.
- Evita dependências diretas entre módulos e facilita vendas por módulo.

## Decisão 3 — Banco de dados
- PostgreSQL como repositório transacional.
- TimescaleDB opcional para séries temporais.
- Estratégia: schemas por domínio / ou row-level tenancy para multi‑tenant.

## Decisão 4 — Observabilidade e Compliance
- Audit trail imutável, logs estruturados e tracing.
- Padrão de contratos: OpenAPI + validação Zod.

## Decisão 5 — Frontend
- Monorepo com `shell` + módulos (lazy loading por licença).
- UI: React + UI5 Web Components para aparência SAP‑like.

## Decisão 6 — IA e Workers
- IA desacoplada: workers/queues (BullMQ) que consomem dados e produzem outputs (resumos, sugestões, análises).
- RAG para documentos regulatórios e SOPs.

## Critérios de extração para microservices
- Carga e performance (latência ou CPU intensivo).
- Fronteiras de dados (necessidade de DB separado).
- Ciclo de deploy independente e tamanho da equipa.

## Plano de migração (resumo)
1. Identificar módulos candidatos (Analytics, Training Hub, Report Generator).
2. Extrair com API Gateway e contratos estáveis.
3. Migrar infra de observabilidade e deploy para suportar serviços separados.

## Referências
- Module Contract: Docs/Requirements/Module_Contract.md (contrato obrigatório entre Shell e módulos).
- Frontend Diagram: Docs/Requirements/Frontend_Diagram.md (regras do Shell e carregamento dos módulos).


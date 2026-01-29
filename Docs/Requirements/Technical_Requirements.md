# Requisitos Técnicos — SmartLab

## Visão geral
Requisitos técnicos essenciais para a implantação, operação e evolução do SmartLab numa instalação industrial/regulada.

## Infraestrutura e implantação
- Arquitetura inicial: Modular Monolith com API central (NestJS/TypeScript).
- Suporte a implantação on‑premise e cloud híbrida.
- Base de dados primária: PostgreSQL (suportar TimescaleDB para séries temporais).
- Redis para cache e filas leves (BullMQ para jobs assíncronos).
- Armazenamento de objetos para ficheiros e evidências (S3‑compatible).
- Execução em contêineres (Docker) e orquestração opcional (Kubernetes) para clientes maiores.

## Observabilidade e operações
- Logs estruturados (JSON), correlação por requestId/traceId.
- Tracing distribuído quando for adotado microservices (OpenTelemetry).
- Métricas (Prometheus) e dashboards (Grafana).
- Backups consistentes e políticas RPO/RTO documentadas.

## Segurança e compliance
- Autenticação: JWT / Supabase Auth ou provider compatível.
- RBAC granulada com políticas contextuais (tenant, role, scope).
- Audit trail imutável por operações críticas (aprov. documentos, resultados LIMS).
- Assinaturas eletrónicas para aprovações (fluxos de compliance).
- Encriptação em repouso e em trânsito (TLS, DB encryption onde aplicável).

## Dados e integridade
- Versionamento de conteúdos críticos (documentos, procedimentos, resultados).
- Registo de evidências: metadados, timestamp, actor, hash.
- Contratos de API firmes (OpenAPI + validação Zod/Schema).

## Performance e escalabilidade
- Escalar verticalmente na fase 1; avaliar extrair módulos para microservices quando necessário.
- Componentes de alta carga (analytics, reports, AI) desenhados para escalar separadamente (jobs, workers, filas).

## Dev & QA
- TypeScript strict mode no backend e frontend.
- Testes unitários e E2E (pipeline CI).
- Monorepo recomendado inicialmente (apps + packages).

## Dependências críticas
- Node.js + NestJS, Prisma (ORM), PostgreSQL, Redis, BullMQ, React + UI5 Web Components.

## Observações finais
- Projetar para evoluir: começar simples (monolith modular) e documentar pontos de extracção para microservices.

## Referências relevantes
- Module Contract: veja Docs/Requirements/Module_Contract.md para o contrato Shell↔Módulos.
- Frontend Diagram: veja Docs/Requirements/Frontend_Diagram.md para a arquitetura do Shell e regras de carregamento.

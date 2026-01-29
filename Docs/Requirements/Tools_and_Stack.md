# Ferramentas e Stack Recomendada — SmartLab

## Backend
- Runtime: Node.js
- Framework: NestJS (TypeScript)
- ORM: Prisma
- DB: PostgreSQL (+ TimescaleDB para séries temporais)
- Cache/Queues: Redis + BullMQ
- Storage: S3‑compatible
- Auth: Supabase Auth (opcional) ou provider compatível

## Frontend
- Framework: React + TypeScript
- UI: UI5 Web Components
- Grid: ag‑Grid
- State: Zustand ou XState (fluxos críticos)
- Forms: React Hook Form

## Analytics / IA
- Python stack para SPC/ML (pandas, scikit‑learn)
- Workers: FastAPI / Python Workers integrados por filas
- RAG: vector DB (opcional) + embeddings provider (OpenAI/Azure/local)

## DevOps / Observability
- Contêineres: Docker
- Orquestração: Kubernetes (para clientes enterprise)
- CI: GitHub Actions / GitLab CI
- Monitoring: Prometheus + Grafana
- Tracing: OpenTelemetry

## Document generation & search
- PDF: Puppeteer / pdf‑lib
- Full‑text search: PostgreSQL FTS ou Meilisearch

## Notas
- Priorizar soluções com suporte on‑premise.
- Escolher bibliotecas com maturidade e comunidade ativa.

# Arquitetura da API Central — SmartLab

## Visão Geral
Documento técnico descreve a arquitetura da API central (core), incluindo módulos, serviços internos, subsistemas de IA, eventos e desenho de bases de dados. Destina‑se a guiar implementação em NestJS/TypeScript e decisões de extração futura para microservices.

## Objetivos principais
- Single Source of Truth: API central expõe todos os contratos de domínio.
- Modularidade: domínios isolados logicamente (bounded contexts) dentro do monólito modular.
- Auditabilidade: todas ações críticas produzam evidência imutável.
- Extensibilidade: permitir extração de módulos (Analytics, Training Hub) quando necessário.

## Módulos (domínios) — visão de alto nível
- Auth & Identity: login, SSO, token issuance, session management.
- Tenant & Billing: tenancy, licensing, regulatoryProfile.
- Audit & Compliance: audit trail imutável, evidence store.
- Module Registry / Schema Registry: registro de módulos, contratos e nomeação de schemas.
- LIMS: amostras, ensaios, resultados, COA.
- QMS: documentos, NC/CAPA, auditorias, competência.
- MES: ordens produção, linhas, status, CIP.
- FSMS: HACCP, CCP monitoring, checklists.
- TMS: cursos, quizzes, certificados (Training Hub).
- Materials: lotes, fornecedores, inventário.
- Reports & Analytics: semantic layer, scheduled reports.
- Tools: AI assistants, notebooks, QC tools.

Cada módulo define suas rotas, permissões e eventos, mas nunca gerencia auth/token ou estado global.

## Serviços internos (core services)
- API Gateway / Router Core: roteamento, versionamento de APIs, rate limiting.
- SmartLabApiClient (SDK interno): client tipado para uso interno entre modules.
- Event Bus (in‑process pub/sub): eventos domínios (fase monolith). Abstração para broker (Kafka/Rabbit) quando extraído.
- Jobs / Workers (BullMQ + Redis): tarefas assíncronas (reports, IA jobs, imports/exports).
- Document Service: armazenamento e versionamento de documentos, PDF generation (puppeteer/pdf-lib).
- Storage Adapter: S3‑compatible object storage for artifacts and evidences.
- Search Service: FTS via Postgres / Meilisearch for fast document lookups.
- Notification Service: email, in‑app notifications, webhooks.
- Tracing & Observability: OpenTelemetry, logs JSON, Prometheus metrics.

## Arquitetura IA (subsistema)
- IA Workers: desacoplados do core; consomem filas (BullMQ) para processar tarefas long‑running (RAG ingestion, slide generation, result pre‑validation).
- RAG pipeline: ingestion → chunking → embeddings → vector DB (Milvus/pgvector) → retriever.
- LLM Provider layer: adaptadores para OpenAI/Azure/local LLMs; configurações por tenant/regulatoryProfile.
- IA APIs expostas: endpoints controlados que retornam sugestões (pre‑validação, resumo, geração de slides) e geram eventos quando aplicável.
- Governance: prompt templates versionadas, cost/latency quotas, PII redaction, evidence attachments for audit.

## Events (event‑driven patterns)
- Padrão: eventos domain‑centric (typed schemas, versioned). Eventos publicados no Event Bus.
- Exemplos padrão (shape minimal):
  - LOT_STARTED {lotId, tenantId, timestamp}
  - LOT_COMPLETED {lotId, tenantId, timestamp, producedQty}
  - ANALYSIS_SCHEDULED {sampleId, methodId}
  - ANALYSIS_COMPLETED {sampleId, resultId, status}
  - NC_CREATED {ncId, sourceModule, severity}
  - CAPA_CLOSED {capaId, resolution}
  - TRAINING_COMPLETED {userId, courseId, score}
- Contratos de evento: JSON Schema + OpenAPI event documentation. Versionamento obrigatório.
- Delivery: in‑process pub/sub → adapter para Kafka/Rabbit for cross‑service comms when split.

## Modelo de Dados e DB
- DB primária: PostgreSQL (single logical cluster). Recomendações:
  - Schemas por domínio (schema per module) ou row‑level tenancy ( dependendo do modelo de tenancy escolhido ).
  - Timeseries: TimescaleDB para séries temporais (sensor data, KPIs, process metrics).
  - Audit Trail: tabela append‑only com hash e referencia ao object storage (evidence blobs).
  - Versionamento: separate tables for versions or temporal tables; all changes carry metadata (actorId, timestamp, changeType).
- Indexing: índices GC/retention policies; FTS para documentos.
- Schema Governance: todos os módulos consultam `SmartLabSchemaRegistry` antes de criar/modificar entidades; conflitos geram bloqueio de CI/pull request.

## Contratos e validação
- API Contracts: OpenAPI (REST) + GraphQL optional for analytics.
- Input/Output validation: Zod (or io-ts) on server boundaries; automatic generation of types for SDK and client.
- Backwards compatibility: semantic versioning for endpoints; deprecation headers + warnings.

## Tenancy & Authorization
- Multi‑tenant options:
  - Single DB with row‑level tenancy (simpler ops), or
  - DB schemas per tenant (stronger isolation), or
  - Hybrid: schema per major customer.
- RBAC: roles + contextual permissions (resource attributes). Policy engine pluggable (Casbin-like).
- Licensing: tenant profile returned at login; Shell builds menu.

## Observabilidade, backups e DR
- Logs: JSON structured logs, central aggregator (Loki/Elasticsearch).
- Tracing: OpenTelemetry for traces across API → workers → IA providers.
- Metrics: Prometheus instrumentation and Grafana dashboards per module.
- Backups: logical pg dumps + file/object storage backups; tested restore procedures and RTO/RPO documented.

## Segurança e compliance
- TLS everywhere; DB encryption at rest.
- Audit trail immutável and signed evidence (hash + storage location).
- Secrets: vault (HashiCorp Vault) for production.
- Data residency: support per `regulatoryProfile` (AO, SADC, GLOBAL) — routing to specific infra or restrictions on LLM providers.

## Extração para microservices — critérios
- Carga/Performance: modules that require independent scaling (Reports, Analytics, Training Hub, IA Workers).
- Autonomia tecnológica: need for Python ecosystem (analytics) or GPU (IA).
- Independent deploy cadence: teams that release separately.

Plano de extração resumido:
1. Harden API contracts (OpenAPI + event schemas).
2. Introduce message broker (Kafka) and migrate Event Bus adapter.
3. Extract read‑heavy or CPU/GPU modules as services with their own DB if needed.
4. Ensure tracing and observability across services.

## Recomendações tecnológicas (curtas)
- Backend: Node.js + NestJS, TypeScript, Prisma.
- DB: PostgreSQL (+ TimescaleDB), pgvector (or Milvus) for embeddings.
- Queue: Redis + BullMQ for workers; Kafka for cross‑service events.
- Storage: S3‑compatible.
- IA: adapter layer for OpenAI/Azure/local + vector DB for RAG.
- CI/CD: GitHub Actions; IaC: Terraform.

## Anexos práticos
- Endpoint contract patterns: use OpenAPI tags per module; include `x‑module` extension.
- Event schema registry: store JSON Schemas in `Module Registry` and require CI validation on publish.
- ModuleContext interface (Shell → Module): enforce via types in `packages/api‑client` SDK.

---

## Diagramas Arquiteturais

### 1. Arquitetura Geral do Sistema (NestJS Modular Monolith)

```
┌─────────────────────────────────────────────────────────────────────────┐
│                          Frontend Apps                                   │
│               (React + UI5 Web Components, Shell + Modules)              │
└──────────────────────────────┬──────────────────────────────────────────┘
                               │ REST / GraphQL
┌──────────────────────────────▼──────────────────────────────────────────┐
│                      NestJS Core API                                     │
│                                                                           │
│  ┌──────────────────────────────────────────────────────────────────┐   │
│  │ API Gateway / HTTP Layer                                         │   │
│  │ - Request validation (Zod pipes)                                 │   │
│  │ - CORS, rate limiting                                            │   │
│  │ - Version negotiation                                            │   │
│  └──────────────────────────────────────────────────────────────────┘   │
│                                │                                         │
│  ┌─────────────────────┬───────▼─────────┬───────────────────────────┐  │
│  │  Core Services      │  Domain         │ IA & Workers            │  │
│  │  ─────────────────  │  Modules        │ ──────────────────────── │  │
│  │                     │  ──────────────  │                         │  │
│  │ • Auth & RBAC       │                 │ • RAG Ingester         │  │
│  │ • Audit Trail       │ • LIMS          │ • Slide Generator      │  │
│  │ • Tenant/License    │ • QMS           │ • Result Validator     │  │
│  │ • Event Bus         │ • FSMS          │ • Analysis Suggester   │  │
│  │ • Module Registry   │ • MES           │                         │  │
│  │ • Document Svc      │ • TMS           │ (via BullMQ + Redis)    │  │
│  │ • Notification      │ • Materials     │                         │  │
│  │ • Search            │ • Reports       │                         │  │
│  │ • Storage Adapter   │ • Tools         │                         │  │
│  └─────────────────────┴─────────────────┴───────────────────────────┘  │
│                                │                                         │
│  ┌──────────────────────────────▼──────────────────────────────────┐   │
│  │ Data Layer & External Services                                  │   │
│  │ - PostgreSQL (audit trail, txnal data)                          │   │
│  │ - TimescaleDB (sensor data, KPIs)                               │   │
│  │ - Redis (cache, job queue)                                      │   │
│  │ - S3-compatible (documents, evidences)                          │   │
│  │ - Vector DB (embeddings for RAG)                                │   │
│  │ - LLM providers (OpenAI/Azure/local)                            │   │
│  └──────────────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────────┘
```

### 2. Fluxo de Eventos (Event-Driven Integration)

```
┌──────────┐        ┌──────────┐        ┌──────────┐        ┌──────────┐
│   MES    │        │  LIMS    │        │   QMS    │        │  FSMS    │
└────┬─────┘        └────┬─────┘        └────┬─────┘        └────┬─────┘
     │                   │                    │                   │
     │ LOT_STARTED       │                    │                   │
     └──────┐            │                    │                   │
            │ Event Bus  │                    │                   │
            ├──────────►(Pub/Sub)             │                   │
            │            │                    │                   │
            │            │ ANALYSIS_SCHEDULED │                   │
            │            ├───────────┐        │                   │
            │            │           │        │                   │
            │            │      (consume)     │                   │
            │            │           │        │                   │
            │            │ ANALYSIS_COMPLETED │                   │
            │            └──────┐────┘        │                   │
            │                   │ Event Bus   │                   │
            │                   ├────────────►(Pub/Sub)           │
            │                   │             │                   │
            │                   │             │ NC_CREATED        │
            │                   │             ├──────┐            │
            │                   │             │      │            │
            │                   │             │ (consume/consume) │
            │                   │             │      │            │
            │                   │      CAPA_CLOSED  │            │
            │                   │             └─────┼────────────►(consume)
            │                   │                    │            │
            └───────────────────┴────────────────────┴────────────┘
```

### 3. Estrutura de Pastas (NestJS Modular Monolith)

```
smartlab-api/
├── src/
│   ├── main.ts                         # Entry point
│   ├── app.module.ts                   # Root module
│   ├── config/                         # Environment & config
│   │   ├── database.config.ts
│   │   ├── redis.config.ts
│   │   └── providers.config.ts
│   │
│   ├── core/                           # Core services (shared)
│   │   ├── auth/
│   │   │   ├── auth.service.ts
│   │   │   ├── auth.controller.ts
│   │   │   ├── guards/
│   │   │   │   ├── jwt.guard.ts
│   │   │   │   └── rbac.guard.ts
│   │   │   └── auth.module.ts
│   │   │
│   │   ├── audit/
│   │   │   ├── audit.service.ts
│   │   │   ├── audit.repository.ts
│   │   │   └── audit.module.ts
│   │   │
│   │   ├── event-bus/
│   │   │   ├── event-bus.service.ts
│   │   │   ├── event.registry.ts
│   │   │   └── event-bus.module.ts
│   │   │
│   │   ├── module-registry/
│   │   │   ├── module-registry.service.ts
│   │   │   ├── schema-registry.service.ts
│   │   │   └── module-registry.module.ts
│   │   │
│   │   ├── document/
│   │   │   ├── document.service.ts
│   │   │   ├── pdf.generator.ts
│   │   │   └── document.module.ts
│   │   │
│   │   ├── storage/
│   │   │   ├── s3.adapter.ts
│   │   │   ├── storage.service.ts
│   │   │   └── storage.module.ts
│   │   │
│   │   ├── notification/
│   │   │   ├── notification.service.ts
│   │   │   ├── email.provider.ts
│   │   │   └── notification.module.ts
│   │   │
│   │   └── shared-types/
│   │       ├── module-context.ts
│   │       ├── event.types.ts
│   │       └── common.dto.ts
│   │
│   ├── modules/                        # Domain modules
│   │   ├── lims/
│   │   │   ├── lims.module.ts
│   │   │   ├── sample/
│   │   │   │   ├── sample.service.ts
│   │   │   │   ├── sample.controller.ts
│   │   │   │   ├── sample.repository.ts
│   │   │   │   └── dto/
│   │   │   ├── analysis/
│   │   │   ├── result/
│   │   │   └── lims.events.ts
│   │   │
│   │   ├── qms/
│   │   │   ├── qms.module.ts
│   │   │   ├── document/
│   │   │   ├── non-conformity/
│   │   │   ├── capa/
│   │   │   └── qms.events.ts
│   │   │
│   │   ├── fsms/
│   │   ├── mes/
│   │   ├── tms/
│   │   ├── materials/
│   │   ├── reports/
│   │   └── tools/
│   │
│   ├── workers/                        # IA & async workers
│   │   ├── aia.worker.ts               # AI Worker orchestration
│   │   ├── rag.ingester.ts
│   │   ├── slide.generator.ts
│   │   ├── result.validator.ts
│   │   └── workers.module.ts
│   │
│   └── interceptors/
│       ├── audit.interceptor.ts
│       ├── tracing.interceptor.ts
│       └── error.interceptor.ts
│
├── prisma/
│   ├── schema.prisma                   # Prisma schema (multi-schema)
│   └── migrations/
│
├── test/
│   ├── unit/
│   ├── integration/
│   └── e2e/
│
├── docker-compose.yml                  # Local dev: Postgres, Redis, etc
├── .env.example
├── .eslintrc.js
├── tsconfig.json
└── package.json
```

---

## Exemplos Práticos

### Interface TypeScript: ModuleContext

```typescript
// src/core/shared-types/module-context.ts

export interface Tenant {
  id: string;
  name: string;
  regulatoryProfile: 'AO' | 'SADC' | 'GLOBAL';
  modules: string[]; // ex: ['LIMS', 'QMS', 'FSMS']
}

export interface User {
  id: string;
  email: string;
  role: string;
  competencies: string[];
  tenantId: string;
}

export interface ModuleContext {
  tenant: Tenant;
  user: User;
  permissions: string[]; // ex: ['LIMS:CREATE_SAMPLE', 'QMS:APPROVE_DOCUMENT']
  
  api: SmartLabApiClient;          // Typed SDK for internal calls
  eventBus: EventBus;               // Pub/sub for domain events
  ai: SmartLabAI;                   // IA assistant (RAG, suggestions)
  
  ui: {
    dialogs: DialogService;
    notifications: NotificationService;
  };
}

export interface SmartLabAI {
  generateSlides(content: string, tenant: string): Promise<Slide[]>;
  validateResult(result: AnalysisResult): Promise<ValidationSuggestion>;
  ragsearch(query: string, source: string): Promise<RagResult[]>;
}

export interface EventBus {
  publish<T extends DomainEvent>(event: T): Promise<void>;
  subscribe<T extends DomainEvent>(
    eventType: string,
    handler: (event: T) => Promise<void>
  ): void;
}
```

### OpenAPI Endpoint Example (LIMS: Create Sample)

```yaml
# docs/openapi/lims.yaml (ou inline em LIMS controller com @ApiOperation)

/api/v1/lims/samples:
  post:
    summary: Create a new sample
    operationId: createSample
    x-module: LIMS
    tags:
      - LIMS
    requestBody:
      required: true
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/CreateSampleDto'
    responses:
      '201':
        description: Sample created successfully
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/SampleDto'
      '400':
        description: Validation error
      '403':
        description: Insufficient permissions
    security:
      - bearerAuth: []

components:
  schemas:
    CreateSampleDto:
      type: object
      required:
        - sampleCode
        - materialId
        - quantity
      properties:
        sampleCode:
          type: string
          minLength: 1
          maxLength: 50
        materialId:
          type: string
          format: uuid
        quantity:
          type: number
          minimum: 0
        samplingDate:
          type: string
          format: date-time
        notes:
          type: string
          maxLength: 500

    SampleDto:
      type: object
      properties:
        id:
          type: string
          format: uuid
        sampleCode:
          type: string
        materialId:
          type: string
          format: uuid
        quantity:
          type: number
        status:
          type: string
          enum: [PENDING, IN_ANALYSIS, COMPLETED, REJECTED]
        createdAt:
          type: string
          format: date-time
        createdBy:
          type: string
          format: uuid
```

### NestJS Controller Example (LIMS)

```typescript
// src/modules/lims/sample/sample.controller.ts

import { Controller, Post, Get, Body, UseGuards, UseInterceptors } from '@nestjs/common';
import { JwtGuard } from '../../../core/auth/guards/jwt.guard';
import { RbacGuard } from '../../../core/auth/guards/rbac.guard';
import { AuditInterceptor } from '../../../interceptors/audit.interceptor';
import { SampleService } from './sample.service';
import { CreateSampleDto, SampleDto } from './dto';

@Controller('api/v1/lims/samples')
@UseGuards(JwtGuard, RbacGuard)
@UseInterceptors(AuditInterceptor)
export class SampleController {
  constructor(private readonly sampleService: SampleService) {}

  @Post()
  async create(
    @Body() dto: CreateSampleDto,
    @CurrentUser() user: User,
    @CurrentTenant() tenant: Tenant
  ): Promise<SampleDto> {
    const sample = await this.sampleService.create(dto, user, tenant);
    
    // Emit event for audit and downstream subscribers
    await this.eventBus.publish({
      type: 'SAMPLE_CREATED',
      aggregateId: sample.id,
      tenantId: tenant.id,
      actorId: user.id,
      timestamp: new Date(),
      data: sample,
    });
    
    return sample;
  }
}
```

### JSON Schema for Event: SAMPLE_CREATED

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "SampleCreated",
  "description": "Event emitted when a new sample is created in LIMS",
  "type": "object",
  "properties": {
    "type": {
      "const": "SAMPLE_CREATED",
      "type": "string"
    },
    "aggregateId": {
      "type": "string",
      "format": "uuid",
      "description": "Sample ID"
    },
    "tenantId": {
      "type": "string",
      "format": "uuid"
    },
    "actorId": {
      "type": "string",
      "format": "uuid",
      "description": "User who created the sample"
    },
    "timestamp": {
      "type": "string",
      "format": "date-time"
    },
    "version": {
      "type": "integer",
      "default": 1,
      "description": "Event schema version"
    },
    "data": {
      "type": "object",
      "properties": {
        "id": {
          "type": "string",
          "format": "uuid"
        },
        "sampleCode": {
          "type": "string"
        },
        "materialId": {
          "type": "string",
          "format": "uuid"
        },
        "quantity": {
          "type": "number"
        },
        "status": {
          "enum": ["PENDING", "IN_ANALYSIS", "COMPLETED", "REJECTED"]
        }
      },
      "required": ["id", "sampleCode", "materialId"]
    }
  },
  "required": ["type", "aggregateId", "tenantId", "actorId", "timestamp", "data"]
}
```

### Event Subscriber Example (QMS listening to LIMS events)

```typescript
// src/modules/qms/qms.event-handler.ts

import { Injectable } from '@nestjs/common';
import { EventBus } from '../../core/event-bus/event-bus.service';

@Injectable()
export class QmsEventHandler {
  constructor(
    private readonly eventBus: EventBus,
    private readonly qmsService: QmsService
  ) {}

  onModuleInit() {
    // Subscribe to LIMS events
    this.eventBus.subscribe('ANALYSIS_COMPLETED', async (event) => {
      // Trigger QMS workflow (e.g., auto-create checkpoint or notify)
      await this.qmsService.handleAnalysisCompleted(event.data);
    });
  }
}
```

### Prisma Schema (Multi-Schema Example)

```prisma
// prisma/schema.prisma

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

generator client {
  provider = "prisma-client-js"
}

// Core schemas (shared across modules)
model Tenant {
  id                  String    @id @default(cuid())
  name                String
  regulatoryProfile   String    // 'AO', 'SADC', 'GLOBAL'
  modules             String[]  // ['LIMS', 'QMS', ...]
  createdAt           DateTime  @default(now())

  @@schema("core")
}

model AuditTrail {
  id          String    @id @default(cuid())
  tenantId    String
  actorId     String
  action      String
  resourceType String
  resourceId  String
  changes     Json      // JSON diff
  timestamp   DateTime  @default(now())
  evidenceUrl String?   // S3 path to supporting file/hash

  @@index([tenantId, timestamp])
  @@index([resourceType, resourceId])
  @@schema("core")
}

// LIMS schema
model Sample {
  id            String    @id @default(cuid())
  tenantId      String
  sampleCode    String
  materialId    String
  quantity      Float
  status        String    // PENDING, IN_ANALYSIS, COMPLETED
  createdAt     DateTime  @default(now())
  updatedAt     DateTime  @updatedAt
  createdBy     String

  @@unique([tenantId, sampleCode])
  @@index([tenantId, status])
  @@schema("lims")
}

// QMS schema
model Document {
  id              String    @id @default(cuid())
  tenantId        String
  title           String
  version         Int
  status          String    // DRAFT, APPROVED, ARCHIVED
  approvedBy      String?
  approvalDate    DateTime?
  createdAt       DateTime  @default(now())

  @@index([tenantId, status])
  @@schema("qms")
}
```

### API Client SDK (Internal, TypeScript)

```typescript
// packages/api-client/src/smartlab.client.ts

export class SmartLabApiClient {
  constructor(private readonly baseUrl: string, private readonly token: string) {}

  // LIMS operations
  async createSample(dto: CreateSampleDto): Promise<SampleDto> {
    return this.post('/api/v1/lims/samples', dto);
  }

  async getSample(id: string): Promise<SampleDto> {
    return this.get(`/api/v1/lims/samples/${id}`);
  }

  // QMS operations
  async approveDocument(docId: string, approvalData: ApprovalDto): Promise<void> {
    return this.post(`/api/v1/qms/documents/${docId}/approve`, approvalData);
  }

  // Helper methods
  private async post<T>(path: string, body: any): Promise<T> {
    const response = await fetch(`${this.baseUrl}${path}`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'Authorization': `Bearer ${this.token}`,
      },
      body: JSON.stringify(body),
    });
    return response.json();
  }

  private async get<T>(path: string): Promise<T> {
    const response = await fetch(`${this.baseUrl}${path}`, {
      headers: {
        'Authorization': `Bearer ${this.token}`,
      },
    });
    return response.json();
  }
}
```

---

## Checklist de Implementação (Phase 1)

- [ ] Scaffold NestJS project com structure acima.
- [ ] Implementar core: Auth, Audit, Event Bus.
- [ ] LIMS MVP: Sample CRUD + eventos.
- [ ] QMS MVP: Document versioning + aprovação simples.
- [ ] PostgreSQL schema governance (SmartLabSchemaRegistry).
- [ ] Testes unitários para core services.
- [ ] Pipeline CI (linting, type-check, unit tests).
- [ ] Docker Compose com Postgres, Redis, e app.
- [ ] Documentação OpenAPI (auto-gerada ou manual).

---

## Próximos Passos

1. Confirm requisitos de tenancy (single DB com row-level vs schema-per-tenant).
2. Selecionar providers de LLM (OpenAI/Azure/local).
3. Iniciar scaffold do NestJS repo com CI/CD pipeline.
4. Definir data de MVP e sprints.
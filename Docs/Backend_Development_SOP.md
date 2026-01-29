# SOP — Desenvolvimento Backend SmartLab

## 1. Visão Geral

Este SOP (Standard Operating Procedure) define as práticas, padrões e workflows obrigatórios para desenvolvimento backend do SmartLab. Aplica-se a:
- NestJS Core API (monolith modular)
- Domain modules (LIMS, QMS, FSMS, MES, TMS, Materials, etc.)
- Core services (Auth, Audit, Event Bus, Document Service, etc.)
- IA Workers (desacoplados, BullMQ-based)

**Objetivo**: garantir modularidade, auditabilidade, testabilidade, escalabilidade para extração futura de microservices e conformidade ISO/regulatória.

---

## 2. Stack Tecnológico

| Camada | Tecnologia | Motivo |
|--------|-----------|--------|
| Runtime | Node.js 20+ LTS | Suporte estendido, performance |
| Framework | NestJS 10+ | Enterprise structure, guards, interceptors, modularity |
| Linguagem | TypeScript (strict mode) | Type safety, auditoria, refactoring seguro |
| ORM | Prisma | Type-safe, migrations, multi-schema support |
| Database | PostgreSQL 15+ | Transações, ACID, FTS, jsonb, arrays, extensões (TimescaleDB) |
| Timeseries | TimescaleDB | Sensor data, KPIs, process metrics otimizados |
| Cache | Redis 7+ | Session store, cache layer, distributed locks |
| Job Queue | BullMQ | Task scheduling, IA workers, async processing |
| Validation | Zod | Schema validation, type inference |
| API Docs | OpenAPI 3.0 / Swagger | Auto-generated, always-in-sync |
| Testing | Jest / Vitest | Unit tests, mocks, coverage |
| E2E Tests | Supertest + Jest | API contract testing |
| Logging | Pino | Structured JSON logs, performance |
| Monitoring | OpenTelemetry + Prometheus | Distributed tracing, metrics |
| Secrets | Vault (HashiCorp) ou .env | Encryption, rotation |
| Package Mgr | pnpm | Monorepo efficiency |

---

## 3. Estrutura do Projeto (NestJS Modular Monolith)

```
smartlab-api/
├── src/
│   ├── main.ts                         # Bootstrap
│   ├── app.module.ts                   # Root module
│   │
│   ├── config/
│   │   ├── database.config.ts
│   │   ├── redis.config.ts
│   │   ├── jwt.config.ts
│   │   ├── vault.config.ts
│   │   └── app.config.ts
│   │
│   ├── core/                           # Shared services (non-domain)
│   │   ├── auth/
│   │   │   ├── auth.service.ts         # JWT issuance, validation
│   │   │   ├── auth.controller.ts
│   │   │   ├── strategies/
│   │   │   │   ├── jwt.strategy.ts
│   │   │   │   └── local.strategy.ts
│   │   │   ├── guards/
│   │   │   │   ├── jwt.guard.ts
│   │   │   │   ├── rbac.guard.ts
│   │   │   │   └── optional-jwt.guard.ts
│   │   │   ├── decorators/
│   │   │   │   ├── current-user.ts
│   │   │   │   ├── current-tenant.ts
│   │   │   │   └── require-permission.ts
│   │   │   └── auth.module.ts
│   │   │
│   │   ├── audit/
│   │   │   ├── audit.service.ts        # Append-only trail
│   │   │   ├── audit.interceptor.ts    # Automatiza capturas
│   │   │   ├── audit.repository.ts
│   │   │   └── audit.module.ts
│   │   │
│   │   ├── event-bus/
│   │   │   ├── event-bus.service.ts    # Pub/sub em-process
│   │   │   ├── event.registry.ts       # Schema validation
│   │   │   ├── event.types.ts
│   │   │   └── event-bus.module.ts
│   │   │
│   │   ├── module-registry/
│   │   │   ├── module-registry.service.ts
│   │   │   ├── schema-registry.service.ts
│   │   │   └── module-registry.module.ts
│   │   │
│   │   ├── document/
│   │   │   ├── document.service.ts
│   │   │   ├── pdf.generator.ts        # Puppeteer/pdf-lib
│   │   │   ├── document.repository.ts
│   │   │   └── document.module.ts
│   │   │
│   │   ├── storage/
│   │   │   ├── s3.adapter.ts           # S3-compatible
│   │   │   ├── storage.service.ts
│   │   │   └── storage.module.ts
│   │   │
│   │   ├── notification/
│   │   │   ├── notification.service.ts
│   │   │   ├── email.provider.ts
│   │   │   ├── notification.repository.ts
│   │   │   └── notification.module.ts
│   │   │
│   │   ├── search/
│   │   │   ├── search.service.ts       # PostgreSQL FTS ou Meilisearch
│   │   │   └── search.module.ts
│   │   │
│   │   ├── ai/
│   │   │   ├── ai.service.ts           # LLM adapter layer
│   │   │   ├── rag.service.ts          # Vector DB integration
│   │   │   ├── providers/
│   │   │   │   ├── openai.provider.ts
│   │   │   │   ├── azure.provider.ts
│   │   │   │   └── local.provider.ts
│   │   │   └── ai.module.ts
│   │   │
│   │   ├── jobs/
│   │   │   ├── jobs.service.ts         # BullMQ orchestration
│   │   │   ├── job.processors.ts
│   │   │   └── jobs.module.ts
│   │   │
│   │   ├── tenant/
│   │   │   ├── tenant.service.ts
│   │   │   ├── tenant.repository.ts
│   │   │   └── tenant.module.ts
│   │   │
│   │   └── shared-types/
│   │       ├── domain-event.ts
│   │       ├── module-context.ts
│   │       └── common.types.ts
│   │
│   ├── modules/                        # Domain modules (bounded contexts)
│   │   ├── lims/
│   │   │   ├── lims.module.ts
│   │   │   ├── lims.controller.ts      # Route definitions
│   │   │   │
│   │   │   ├── sample/
│   │   │   │   ├── sample.service.ts
│   │   │   │   ├── sample.controller.ts
│   │   │   │   ├── sample.repository.ts
│   │   │   │   ├── dto/
│   │   │   │   │   ├── create-sample.dto.ts
│   │   │   │   │   └── sample.dto.ts
│   │   │   │   ├── types/
│   │   │   │   │   └── sample.types.ts
│   │   │   │   └── sample.module.ts
│   │   │   │
│   │   │   ├── analysis/
│   │   │   │   ├── analysis.service.ts
│   │   │   │   ├── analysis.controller.ts
│   │   │   │   ├── analysis.repository.ts
│   │   │   │   ├── dto/
│   │   │   │   ├── validators/
│   │   │   │   │   └── result.validator.ts
│   │   │   │   └── analysis.module.ts
│   │   │   │
│   │   │   ├── result/
│   │   │   ├── coa/
│   │   │   │
│   │   │   └── lims.events.ts          # Event definitions (SAMPLE_CREATED, etc)
│   │   │
│   │   ├── qms/
│   │   │   ├── qms.module.ts
│   │   │   ├── qms.controller.ts
│   │   │   ├── document/
│   │   │   ├── non-conformity/
│   │   │   ├── capa/
│   │   │   │   ├── capa.service.ts
│   │   │   │   ├── capa.workflow.ts   # State machine (XState ou custom)
│   │   │   │   └── capa.controller.ts
│   │   │   └── qms.events.ts
│   │   │
│   │   ├── fsms/
│   │   │   ├── fsms.module.ts
│   │   │   ├── haccp/
│   │   │   ├── ccp-monitoring/
│   │   │   └── fsms.events.ts
│   │   │
│   │   ├── mes/
│   │   │   ├── mes.module.ts
│   │   │   ├── production-order/
│   │   │   ├── line/
│   │   │   ├── cip/
│   │   │   └── mes.events.ts
│   │   │
│   │   ├── tms/
│   │   │   ├── tms.module.ts
│   │   │   ├── course/
│   │   │   ├── quiz/
│   │   │   ├── certificate/
│   │   │   └── tms.events.ts
│   │   │
│   │   ├── materials/
│   │   │   ├── materials.module.ts
│   │   │   ├── batch/
│   │   │   ├── supplier/
│   │   │   └── materials.events.ts
│   │   │
│   │   ├── reports/
│   │   │   ├── reports.module.ts
│   │   │   ├── report.service.ts
│   │   │   ├── report.generator.ts
│   │   │   ├── views/              # SQL views versionadas
│   │   │   └── reports.controller.ts
│   │   │
│   │   └── tools/
│   │       ├── tools.module.ts
│   │       ├── notebook/
│   │       └── quality-tools/
│   │
│   ├── workers/                        # Async job processors (BullMQ)
│   │   ├── workers.module.ts
│   │   ├── aia.worker.ts               # AI assistant worker
│   │   ├── report.worker.ts            # Report generation worker
│   │   ├── notification.worker.ts      # Email/SMS sender
│   │   └── import.worker.ts            # Data import processor
│   │
│   ├── interceptors/
│   │   ├── audit.interceptor.ts        # Captura metadata
│   │   ├── tracing.interceptor.ts      # OpenTelemetry
│   │   ├── error.interceptor.ts        # Global error handling
│   │   └── validation.interceptor.ts   # Response validation
│   │
│   ├── pipes/
│   │   ├── validation.pipe.ts          # Zod validation
│   │   └── parse-uuid.pipe.ts
│   │
│   ├── filters/
│   │   └── all-exceptions.filter.ts    # Exception handling
│   │
│   ├── middleware/
│   │   ├── logger.middleware.ts
│   │   ├── correlation-id.middleware.ts
│   │   └── tenant-resolver.middleware.ts
│   │
│   └── utils/
│       ├── crypto.ts
│       ├── date-utils.ts
│       ├── validators.ts
│       └── error-codes.ts
│
├── prisma/
│   ├── schema.prisma                   # Multi-schema
│   ├── migrations/
│   │   ├── 001_core.sql
│   │   ├── 002_lims.sql
│   │   ├── 003_qms.sql
│   │   └── ...migration_timestamp...
│   └── seed.ts                         # Initial data
│
├── test/
│   ├── unit/
│   │   ├── modules/
│   │   │   ├── lims/
│   │   │   │   └── sample.service.spec.ts
│   │   │   └── qms/
│   │   └── core/
│   │       └── audit.service.spec.ts
│   │
│   ├── integration/
│   │   ├── lims.e2e.spec.ts
│   │   └── event-bus.integration.spec.ts
│   │
│   └── e2e/
│       └── api.e2e.spec.ts
│
├── docs/
│   ├── openapi/
│   │   ├── openapi.yaml               # Main spec ou gerado
│   │   ├── lims.openapi.yaml
│   │   ├── qms.openapi.yaml
│   │   └── schemas/
│   │       ├── sample.schema.yaml
│   │       ├── event.schema.yaml
│   │       └── ...
│   └── architecture/
│       └── module-dependencies.md
│
├── .env.example
├── .eslintrc.js
├── .prettierrc
├── docker-compose.yml                  # Local dev: Postgres, Redis, etc
├── jest.config.js
├── tsconfig.json
├── package.json
└── README.md
```

---

## 4. Padrões de Codificação

### 4.1 TypeScript Strict Mode (obrigatório)

```json
{
  "compilerOptions": {
    "strict": true,
    "noImplicitAny": true,
    "strictNullChecks": true,
    "strictFunctionTypes": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": true,
    "forceConsistentCasingInFileNames": true,
    "resolveJsonModule": true,
    "moduleResolution": "node"
  }
}
```

### 4.2 NestJS Module Pattern (Bounded Context)

```typescript
// src/modules/lims/lims.module.ts

import { Module } from '@nestjs/common';
import { PrismaModule } from '../../core/prisma/prisma.module';
import { EventBusModule } from '../../core/event-bus/event-bus.module';
import { SampleModule } from './sample/sample.module';
import { AnalysisModule } from './analysis/analysis.module';
import { LimsController } from './lims.controller';

@Module({
  imports: [
    PrismaModule,
    EventBusModule,
    SampleModule,
    AnalysisModule,
  ],
  controllers: [LimsController],
  exports: [SampleModule, AnalysisModule], // Permite que outros módulos usem
})
export class LimsModule {}
```

### 4.3 Service com Injeção de Dependências

```typescript
// src/modules/lims/sample/sample.service.ts

import { Injectable, NotFoundException, BadRequestException } from '@nestjs/common';
import { PrismaService } from '../../../core/prisma/prisma.service';
import { EventBusService } from '../../../core/event-bus/event-bus.service';
import { AuditService } from '../../../core/audit/audit.service';
import { CreateSampleDto } from './dto/create-sample.dto';
import type { Sample } from './types/sample.types';

@Injectable()
export class SampleService {
  constructor(
    private readonly prisma: PrismaService,
    private readonly eventBus: EventBusService,
    private readonly audit: AuditService,
  ) {}

  async create(dto: CreateSampleDto, userId: string, tenantId: string): Promise<Sample> {
    // Validação lógica
    if (dto.quantity <= 0) {
      throw new BadRequestException('Quantity must be positive');
    }

    // Transação (garantir consistência)
    const sample = await this.prisma.$transaction(async (tx) => {
      const created = await tx.sample.create({
        data: {
          ...dto,
          tenantId,
          createdBy: userId,
        },
      });
      return created;
    });

    // Publicar evento (rastreabilidade)
    await this.eventBus.publish({
      type: 'SAMPLE_CREATED',
      aggregateId: sample.id,
      tenantId,
      actorId: userId,
      timestamp: new Date(),
      data: sample,
    });

    // Registar auditoria automaticamente (via interceptor)
    return sample;
  }

  async findById(id: string, tenantId: string): Promise<Sample> {
    const sample = await this.prisma.sample.findUnique({
      where: { id_tenantId: { id, tenantId } },
    });

    if (!sample) {
      throw new NotFoundException(`Sample ${id} not found`);
    }

    return sample;
  }

  async list(filters: Record<string, any>, tenantId: string): Promise<Sample[]> {
    return this.prisma.sample.findMany({
      where: {
        tenantId,
        ...filters,
      },
    });
  }
}
```

### 4.4 Controller com Guards e Decoradores

```typescript
// src/modules/lims/sample/sample.controller.ts

import {
  Controller,
  Post,
  Get,
  Body,
  Param,
  UseGuards,
  UseInterceptors,
} from '@nestjs/common';
import { JwtGuard } from '../../../core/auth/guards/jwt.guard';
import { RbacGuard } from '../../../core/auth/guards/rbac.guard';
import { AuditInterceptor } from '../../../interceptors/audit.interceptor';
import { CurrentUser } from '../../../core/auth/decorators/current-user';
import { CurrentTenant } from '../../../core/auth/decorators/current-tenant';
import { RequirePermission } from '../../../core/auth/decorators/require-permission';
import { SampleService } from './sample.service';
import { CreateSampleDto } from './dto/create-sample.dto';
import type { User, Tenant } from '../../../core/shared-types';

@Controller('api/v1/lims/samples')
@UseGuards(JwtGuard, RbacGuard)
@UseInterceptors(AuditInterceptor)
export class SampleController {
  constructor(private readonly sampleService: SampleService) {}

  @Post()
  @RequirePermission('LIMS:CREATE_SAMPLE')
  async create(
    @Body() dto: CreateSampleDto,
    @CurrentUser() user: User,
    @CurrentTenant() tenant: Tenant,
  ) {
    return this.sampleService.create(dto, user.id, tenant.id);
  }

  @Get(':id')
  @RequirePermission('LIMS:VIEW_SAMPLE')
  async getById(
    @Param('id') id: string,
    @CurrentTenant() tenant: Tenant,
  ) {
    return this.sampleService.findById(id, tenant.id);
  }

  @Get()
  @RequirePermission('LIMS:LIST_SAMPLES')
  async list(@CurrentTenant() tenant: Tenant) {
    return this.sampleService.list({}, tenant.id);
  }
}
```

### 4.5 DTO com Zod Validation

```typescript
// src/modules/lims/sample/dto/create-sample.dto.ts

import { z } from 'zod';

export const CreateSampleSchema = z.object({
  sampleCode: z.string().min(1).max(50),
  materialId: z.string().uuid(),
  quantity: z.number().positive(),
  samplingDate: z.string().datetime().optional(),
  notes: z.string().max(500).optional(),
});

export type CreateSampleDto = z.infer<typeof CreateSampleSchema>;

// Validação no pipe
import { PipeTransform, BadRequestException } from '@nestjs/common';

export class ZodValidationPipe implements PipeTransform {
  constructor(private schema: z.ZodSchema) {}

  transform(value: any) {
    try {
      return this.schema.parse(value);
    } catch (error) {
      throw new BadRequestException('Validation failed');
    }
  }
}

// Uso no controller
@Post()
async create(@Body(new ZodValidationPipe(CreateSampleSchema)) dto: CreateSampleDto) {
  return this.sampleService.create(dto);
}
```

### 4.6 Event Handling (Domain Events)

```typescript
// src/modules/lims/lims.events.ts

export type LimsEvent =
  | SampleCreated
  | AnalysisScheduled
  | AnalysisCompleted;

export interface SampleCreated {
  type: 'SAMPLE_CREATED';
  aggregateId: string;
  tenantId: string;
  actorId: string;
  timestamp: Date;
  version: number;
  data: {
    id: string;
    sampleCode: string;
    materialId: string;
    quantity: number;
  };
}

export interface AnalysisCompleted {
  type: 'ANALYSIS_COMPLETED';
  aggregateId: string;
  tenantId: string;
  timestamp: Date;
  data: {
    sampleId: string;
    resultId: string;
    status: string;
  };
}
```

### 4.7 Event Subscriber (Cross-Module Integration)

```typescript
// src/modules/qms/qms.event-handler.ts

import { Injectable, OnModuleInit } from '@nestjs/common';
import { EventBusService } from '../../core/event-bus/event-bus.service';
import { QmsService } from './qms.service';
import type { AnalysisCompleted } from '../lims/lims.events';

@Injectable()
export class QmsEventHandler implements OnModuleInit {
  constructor(
    private readonly eventBus: EventBusService,
    private readonly qmsService: QmsService,
  ) {}

  onModuleInit() {
    // Subscribe to LIMS event
    this.eventBus.subscribe<AnalysisCompleted>(
      'ANALYSIS_COMPLETED',
      async (event) => {
        // Trigger QMS workflow
        await this.qmsService.handleAnalysisCompleted(event);
      },
    );
  }
}
```

### 4.8 Repository Pattern (Data Access)

```typescript
// src/modules/lims/sample/sample.repository.ts

import { Injectable } from '@nestjs/common';
import { PrismaService } from '../../../core/prisma/prisma.service';
import type { Sample } from './types/sample.types';

@Injectable()
export class SampleRepository {
  constructor(private readonly prisma: PrismaService) {}

  async create(data: any): Promise<Sample> {
    return this.prisma.sample.create({ data });
  }

  async findById(id: string, tenantId: string): Promise<Sample | null> {
    return this.prisma.sample.findUnique({
      where: { id_tenantId: { id, tenantId } },
    });
  }

  async update(id: string, tenantId: string, data: any): Promise<Sample> {
    return this.prisma.sample.update({
      where: { id_tenantId: { id, tenantId } },
      data,
    });
  }

  async delete(id: string, tenantId: string): Promise<void> {
    await this.prisma.sample.delete({
      where: { id_tenantId: { id, tenantId } },
    });
  }

  async listByStatus(status: string, tenantId: string): Promise<Sample[]> {
    return this.prisma.sample.findMany({
      where: { status, tenantId },
      orderBy: { createdAt: 'desc' },
    });
  }
}
```

---

## 5. Fluxo de Desenvolvimento

### 5.1 Setup Local

```bash
# 1. Clone e install
git clone https://github.com/fredyus29-wq/SmartLab-V7.git
cd smartlab-api
pnpm install

# 2. Setup environment
cp .env.example .env
# Edit .env with local values

# 3. Start dev database (Docker Compose)
docker-compose up -d

# 4. Run migrations
pnpm prisma migrate deploy

# 5. Seed data (opcional)
pnpm prisma db seed

# 6. Start dev server
pnpm dev

# Acesso: http://localhost:3000
# Swagger UI: http://localhost:3000/api/docs
```

### 5.2 Feature Development

```bash
# 1. Create feature branch
git checkout -b feature/LIMS-123-sample-approval

# 2. Implement feature
# - Create service, controller, DTO, types
# - Add event definitions
# - Update Prisma schema if needed

# 3. Run migration (se schema changed)
pnpm prisma migrate dev --name add_sample_approval

# 4. Write tests
pnpm test

# 5. Type check e lint
pnpm type-check
pnpm lint
pnpm lint:fix
```

### 5.3 Testing Strategy

```bash
# Unit tests
pnpm test:unit

# Integration tests
pnpm test:integration

# E2E tests
pnpm test:e2e

# Coverage
pnpm test:coverage

# Watch mode (development)
pnpm test:watch
```

### 5.4 Submeter PR

```bash
# 1. Ensure all tests pass
pnpm test

# 2. Build check
pnpm build

# 3. Commit com mensagem descritiva
git commit -m "feat(LIMS): implement sample approval workflow

- Add approval endpoint with RBAC
- Emit SAMPLE_APPROVED event
- Auto-create QMS checkpoint
- Add unit tests (95% coverage)

Closes #LIMS-123"

# 4. Push
git push origin feature/LIMS-123-sample-approval

# 5. Create PR no GitHub (CI runs automatically)
# CI checks:
#  - ESLint + Prettier
#  - Type check (tsc)
#  - Unit + integration tests
#  - Coverage threshold (80% minimum)
#  - Build validation
```

---

## 6. Padrões Obrigatórios

### 6.1 Auditoria (Append-Only Audit Trail)

```typescript
// ✅ CORRETO: Usar AuditInterceptor (automático)
@Post()
@UseInterceptors(AuditInterceptor)
async create(@Body() dto: CreateSampleDto, @CurrentUser() user: User) {
  // Interceptor captura automaticamente:
  // - User ID, Tenant ID, Timestamp
  // - Method, path, status code
  // - Request/response payload
  // - Stored immutably in audit table
  return this.sampleService.create(dto);
}

// ❌ ERRADO: Não capturar auditoria
@Post()
async create(@Body() dto: CreateSampleDto) {
  // Missing audit!
  return this.sampleService.create(dto);
}
```

### 6.2 Event Publishing (Rastreabilidade)

```typescript
// ✅ CORRETO: Publicar evento após ação crítica
await this.eventBus.publish({
  type: 'SAMPLE_APPROVED',
  aggregateId: sampleId,
  tenantId,
  actorId: userId,
  timestamp: new Date(),
  version: 1,
  data: { sampleId, approvedBy: userId },
});

// ❌ ERRADO: Não publicar evento
// Sem evento, outros módulos (QMS, FSMS) não sabem da aprovação
```

### 6.3 Transações (Data Consistency)

```typescript
// ✅ CORRETO: Usar Prisma transactions
const result = await this.prisma.$transaction(async (tx) => {
  const sample = await tx.sample.create({ data: sampleData });
  const analysis = await tx.analysis.create({ data: analysisData });
  return { sample, analysis };
});

// ❌ ERRADO: Chamadas separadas (race condition risk)
const sample = await this.prisma.sample.create({ data: sampleData });
const analysis = await this.prisma.analysis.create({ data: analysisData });
```

### 6.4 Validação em Camadas

```typescript
// ✅ CORRETO: Validação em múltiplas camadas
// Layer 1: DTO + Zod (input validation)
@Post()
async create(@Body(new ZodValidationPipe(CreateSampleSchema)) dto: CreateSampleDto) {
  // Layer 2: Service (business logic validation)
  if (dto.quantity > materialInventory.available) {
    throw new BadRequestException('Insufficient inventory');
  }
  // Layer 3: DB constraints (final safeguard)
  return this.sampleService.create(dto);
}
```

### 6.5 RBAC (Role-Based Access Control)

```typescript
// ✅ CORRETO: Usar guards e decoradores
@Post()
@RequirePermission('LIMS:APPROVE_SAMPLE')
@UseGuards(JwtGuard, RbacGuard)
async approveSample(@Param('id') id: string, @CurrentUser() user: User) {
  // Guard verifica:
  // 1. JWT válido
  // 2. User role tem 'LIMS:APPROVE_SAMPLE'
  // 3. Contextual checks (ex: só seu tenant)
  return this.sampleService.approve(id, user.id);
}

// ❌ ERRADO: Sem guards
@Post()
async approveSample(@Param('id') id: string) {
  // Qualquer um pode aprovar!
  return this.sampleService.approve(id);
}
```

---

## 7. Testes

### 7.1 Unit Test (Jest)

```typescript
// src/modules/lims/sample/sample.service.spec.ts

import { Test, TestingModule } from '@nestjs/testing';
import { SampleService } from './sample.service';
import { PrismaService } from '../../../core/prisma/prisma.service';
import { EventBusService } from '../../../core/event-bus/event-bus.service';

describe('SampleService', () => {
  let service: SampleService;
  let prisma: PrismaService;
  let eventBus: EventBusService;

  beforeEach(async () => {
    const module: TestingModule = await Test.createTestingModule({
      providers: [
        SampleService,
        {
          provide: PrismaService,
          useValue: {
            sample: { create: jest.fn(), findUnique: jest.fn() },
            $transaction: jest.fn(),
          },
        },
        {
          provide: EventBusService,
          useValue: { publish: jest.fn() },
        },
      ],
    }).compile();

    service = module.get<SampleService>(SampleService);
    prisma = module.get<PrismaService>(PrismaService);
    eventBus = module.get<EventBusService>(EventBusService);
  });

  it('should create a sample and publish event', async () => {
    const dto = { sampleCode: 'S001', materialId: 'mat-1', quantity: 100 };
    const createdSample = { id: '1', ...dto };

    jest.spyOn(prisma, '$transaction').mockResolvedValue(createdSample);
    jest.spyOn(eventBus, 'publish').mockResolvedValue(undefined);

    const result = await service.create(dto, 'user-1', 'tenant-1');

    expect(result).toEqual(createdSample);
    expect(eventBus.publish).toHaveBeenCalledWith(
      expect.objectContaining({ type: 'SAMPLE_CREATED' })
    );
  });

  it('should throw BadRequestException for invalid quantity', async () => {
    const dto = { sampleCode: 'S001', materialId: 'mat-1', quantity: -10 };

    await expect(service.create(dto, 'user-1', 'tenant-1')).rejects.toThrow(
      BadRequestException
    );
  });
});
```

### 7.2 E2E Test (Supertest)

```typescript
// test/e2e/lims.e2e.spec.ts

import { Test, TestingModule } from '@nestjs/testing';
import { INestApplication } from '@nestjs/common';
import * as request from 'supertest';
import { AppModule } from '../src/app.module';

describe('LIMS API (e2e)', () => {
  let app: INestApplication;
  let authToken: string;

  beforeAll(async () => {
    const moduleFixture: TestingModule = await Test.createTestingModule({
      imports: [AppModule],
    }).compile();

    app = moduleFixture.createNestApplication();
    await app.init();

    // Get auth token
    const loginRes = await request(app.getHttpServer())
      .post('/api/v1/auth/login')
      .send({ email: 'test@example.com', password: 'password' });

    authToken = loginRes.body.accessToken;
  });

  afterAll(async () => {
    await app.close();
  });

  describe('POST /api/v1/lims/samples', () => {
    it('should create a sample', async () => {
      const response = await request(app.getHttpServer())
        .post('/api/v1/lims/samples')
        .set('Authorization', `Bearer ${authToken}`)
        .send({
          sampleCode: 'S001',
          materialId: 'mat-1',
          quantity: 100,
        });

      expect(response.status).toBe(201);
      expect(response.body).toHaveProperty('id');
      expect(response.body.sampleCode).toBe('S001');
    });

    it('should return 400 for invalid data', async () => {
      const response = await request(app.getHttpServer())
        .post('/api/v1/lims/samples')
        .set('Authorization', `Bearer ${authToken}`)
        .send({
          sampleCode: '', // Invalid
          materialId: 'invalid-uuid',
          quantity: -10, // Invalid
        });

      expect(response.status).toBe(400);
    });

    it('should return 403 without permission', async () => {
      const response = await request(app.getHttpServer())
        .post('/api/v1/lims/samples')
        .set('Authorization', `Bearer ${authToken}`)
        .send({ /* ... */ });

      // If user doesn't have LIMS:CREATE_SAMPLE permission
      expect(response.status).toBe(403);
    });
  });
});
```

---

## 8. Database & Migrations

### 8.1 Prisma Schema (Multi-Schema)

```prisma
// prisma/schema.prisma

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

generator client {
  provider = "prisma-client-js"
}

// ============ CORE SCHEMA ============
model Tenant {
  id                  String    @id @default(cuid())
  name                String
  regulatoryProfile   String    // 'AO', 'SADC', 'GLOBAL'
  modules             String[]  // Enabled modules
  createdAt           DateTime  @default(now())

  @@schema("core")
}

model AuditTrail {
  id              String    @id @default(cuid())
  tenantId        String
  actorId         String
  action          String
  resourceType    String
  resourceId      String
  changes         Json      // {before, after}
  timestamp       DateTime  @default(now())
  evidenceUrl     String?   // S3 path

  @@index([tenantId, timestamp])
  @@index([resourceType, resourceId])
  @@schema("core")
}

// ============ LIMS SCHEMA ============
model Sample {
  id            String    @id @default(cuid())
  tenantId      String
  sampleCode    String
  materialId    String
  quantity      Float
  status        String    // PENDING, IN_ANALYSIS, COMPLETED, REJECTED
  samplingDate  DateTime?
  createdAt     DateTime  @default(now())
  createdBy     String

  @@unique([tenantId, sampleCode])
  @@index([tenantId, status])
  @@schema("lims")
}

// ============ QMS SCHEMA ============
model Document {
  id            String    @id @default(cuid())
  tenantId      String
  title         String
  version       Int
  status        String    // DRAFT, APPROVED, ARCHIVED
  approvedBy    String?
  approvalDate  DateTime?
  createdAt     DateTime  @default(now())

  @@index([tenantId, status])
  @@schema("qms")
}
```

### 8.2 Running Migrations

```bash
# Create migration after schema change
pnpm prisma migrate dev --name add_sample_approval_field

# Apply migrations in production
pnpm prisma migrate deploy

# Check status
pnpm prisma migrate status

# Rollback (dev only)
pnpm prisma migrate resolve --rolled-back migration_name
```

---

## 9. Observability & Monitoring

### 9.1 Structured Logging (Pino)

```typescript
// src/main.ts

import { Logger } from '@nestjs/common';
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule, {
    logger: ['error', 'warn', 'log'],
  });

  // Structured logging
  const logger = new Logger('Bootstrap');
  logger.log(`Application started on port 3000`, {
    environment: process.env.NODE_ENV,
    version: process.env.APP_VERSION,
  });

  await app.listen(3000);
}

bootstrap();
```

### 9.2 OpenTelemetry Tracing

```typescript
// Automatic request tracing via interceptor
@Injectable()
export class TracingInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler) {
    const request = context.switchToHttp().getRequest();
    const traceId = request.headers['x-trace-id'] || generateTraceId();

    // Propagate traceId
    context.switchToRpc().getData().traceId = traceId;

    return next.handle().pipe(
      tap(() => {
        // Log successful operation
        console.log(`[${traceId}] Operation completed`);
      }),
    );
  }
}
```

### 9.3 Prometheus Metrics

```typescript
// Custom metric
import { Counter, Histogram } from '@opentelemetry/api/metrics';

export const sampleCreatedCounter = new Counter('samples_created_total');
export const sampleProcessingTime = new Histogram('sample_processing_seconds');

// Usage in service
sampleCreatedCounter.add(1, { tenant_id: tenantId });
```

---

## 10. CI/CD Pipeline

### 10.1 GitHub Actions

```yaml
# .github/workflows/ci.yml

name: CI

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    services:
      postgres:
        image: postgres:15
        env:
          POSTGRES_PASSWORD: postgres
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
      redis:
        image: redis:7
        options: >-
          --health-cmd "redis-cli ping"
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5

    steps:
      - uses: actions/checkout@v3
      - uses: pnpm/action-setup@v2
      - uses: actions/setup-node@v3
        with:
          node-version: '20'
          cache: 'pnpm'

      - run: pnpm install
      - run: pnpm type-check
      - run: pnpm lint
      - run: pnpm test:unit
      - run: pnpm test:integration
      - run: pnpm build

      - name: Upload coverage
        uses: codecov/codecov-action@v3
```

---

## 11. Checklist de Qualidade (Antes de PR)

- [ ] TypeScript strict mode (sem `any`, `@ts-ignore`).
- [ ] ESLint passa sem warnings.
- [ ] Prettier formatting aplicado.
- [ ] Type-check passa (`pnpm type-check`).
- [ ] Unit tests com >80% coverage.
- [ ] Integração tests para fluxos críticos.
- [ ] Sem console.logs em produção (usar logger).
- [ ] Sem credenciais hardcoded (usar .env).
- [ ] RBAC validation em endpoints críticos.
- [ ] Eventos publicados para ações domínio críticas.
- [ ] Migrations criadas (se schema changed).
- [ ] Documentação OpenAPI atualizada.
- [ ] Build local passa.
- [ ] Commit messages descritivas.

---

## 12. Troubleshooting

| Problema | Solução |
|----------|---------|
| Migration fails | Check schema conflicts; rollback and redo |
| Event not published | Verify EventBus is injected; check topic name |
| Type errors | Run `pnpm type-check`; check imports |
| Jest timeout | Increase timeout in jest.config.js |
| DB connection error | Check .env DATABASE_URL; verify Postgres running |
| Redis timeout | Verify Redis running; check Redis URL |
| RBAC denying valid request | Check permission string format; verify user roles |

---

## 13. Referências

- [NestJS Docs](https://docs.nestjs.com/)
- [Prisma Docs](https://www.prisma.io/docs)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)
- [OpenTelemetry Docs](https://opentelemetry.io/docs)
- [Jest Docs](https://jestjs.io/)
- [Zod Docs](https://zod.dev)

---

## 14. Aprovação e Versionamento

**Última atualização**: 29 de janeiro de 2026
**Versão**: 1.0
**Responsável**: Tech Lead / Architecture Team

Alterações neste SOP requerem aprovação de tech lead.
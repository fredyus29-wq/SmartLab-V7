# SOP — Desenvolvimento Frontend SmartLab

## 1. Visão Geral

Este SOP (Standard Operating Procedure) define as práticas, padrões e workflows obrigatórios para desenvolvimento frontend do SmartLab. Aplica-se a:
- Shell App (core UI)
- Módulos (LIMS, QMS, FSMS, TMS, etc.)
- Packages compartilhados (ui-kit, api-client, design-tokens)

**Objetivo**: garantir UX unificada, auditabilidade, escalabilidade e venda modular sem caos de arquitetura.

---

## 2. Stack Tecnológico

| Camada | Tecnologia | Motivo |
|--------|-----------|--------|
| Framework | React 18+ | Component-based, maturidade, ecosystem |
| Linguagem | TypeScript (strict mode) | Type safety, auditoria de contracts |
| State Mgmt | Zustand (simples) ou XState (fluxos críticos) | Leve, sem overhead, integração com eventos |
| UI Components | UI5 Web Components | SAP-like, industrial, robusto, acessibilidade |
| Forms | React Hook Form + Zod | Validação forte, DX superior |
| HTTP Client | axios ou fetch API | Tipado via api-client SDK |
| Styling | Tailwind CSS + design-tokens | Consistência, density industrial |
| Testing | Vitest (unit), Playwright (E2E) | Velocidade, maintenability |
| Build | Vite | Rápido, suporta monorepo com pnpm workspaces |
| Linting | ESLint + Prettier | Consistência de código |
| Package Mgr | pnpm | Melhor gerenciamento monorepo |

---

## 3. Estrutura do Projeto (Monorepo)

```
smartlab-frontend/
├── apps/
│   ├── shell/                          # Shell App (obrigatório, entry point)
│   │   ├── src/
│   │   │   ├── main.tsx
│   │   │   ├── App.tsx
│   │   │   ├── layout/
│   │   │   │   ├── Shell.tsx           # Header, sidebar, footer
│   │   │   │   ├── ModuleRouter.tsx
│   │   │   │   └── styles/
│   │   │   ├── hooks/
│   │   │   │   ├── useAuth.ts
│   │   │   │   ├── useTenant.ts
│   │   │   │   ├── useModules.ts
│   │   │   │   └── useMenuBuilder.ts
│   │   │   ├── context/
│   │   │   │   ├── AuthContext.tsx
│   │   │   │   ├── TenantContext.tsx
│   │   │   │   └── ModuleContext.tsx
│   │   │   ├── services/
│   │   │   │   ├── auth.service.ts
│   │   │   │   ├── module-loader.ts
│   │   │   │   └── menu-builder.ts
│   │   │   └── pages/
│   │   │       ├── LoginPage.tsx
│   │   │       ├── DashboardPage.tsx
│   │   │       └── NotFoundPage.tsx
│   │   ├── vite.config.ts
│   │   ├── tsconfig.json
│   │   └── package.json
│   │
│   ├── lims/                           # Módulo LIMS
│   │   ├── src/
│   │   │   ├── index.tsx               # Module export (register function)
│   │   │   ├── module.tsx              # Module entry (routes + menu)
│   │   │   ├── pages/
│   │   │   │   ├── SamplesListPage.tsx
│   │   │   │   ├── SampleDetailPage.tsx
│   │   │   │   └── AnalysisPage.tsx
│   │   │   ├── components/
│   │   │   │   ├── SampleForm.tsx
│   │   │   │   ├── SampleTable.tsx
│   │   │   │   └── ResultValidator.tsx
│   │   │   ├── hooks/
│   │   │   │   ├── useSamples.ts
│   │   │   │   └── useAnalysis.ts
│   │   │   ├── services/
│   │   │   │   ├── sample.service.ts
│   │   │   │   └── analysis.service.ts
│   │   │   ├── store/
│   │   │   │   ├── sample.store.ts
│   │   │   │   └── analysis.store.ts
│   │   │   └── types/
│   │   │       └── lims.types.ts
│   │   ├── vite.config.ts
│   │   └── package.json
│   │
│   ├── qms/                            # Módulo QMS
│   ├── fsms/                           # Módulo FSMS
│   ├── tms/                            # Módulo TMS
│   └── analytics/                      # Módulo Analytics
│
├── packages/
│   ├── ui-kit/                         # UI5 Web Components wrappers
│   │   ├── src/
│   │   │   ├── components/
│   │   │   │   ├── Button.tsx
│   │   │   │   ├── Table.tsx
│   │   │   │   ├── Dialog.tsx
│   │   │   │   ├── Card.tsx
│   │   │   │   └── index.ts
│   │   │   ├── hooks/
│   │   │   │   └── useUI5Theme.ts
│   │   │   └── index.ts
│   │   └── package.json
│   │
│   ├── design-tokens/                  # Design system (colors, spacing, etc)
│   │   ├── src/
│   │   │   ├── colors.ts
│   │   │   ├── spacing.ts
│   │   │   ├── typography.ts
│   │   │   ├── density.ts
│   │   │   └── index.ts
│   │   └── package.json
│   │
│   ├── api-client/                     # SDK tipado (types + helpers)
│   │   ├── src/
│   │   │   ├── client.ts               # SmartLabApiClient class
│   │   │   ├── lims/
│   │   │   │   ├── types.ts
│   │   │   │   └── endpoints.ts
│   │   │   ├── qms/
│   │   │   ├── auth/
│   │   │   │   ├── types.ts
│   │   │   │   └── endpoints.ts
│   │   │   └── index.ts
│   │   └── package.json
│   │
│   ├── auth/                           # RBAC guards e decoradores
│   │   ├── src/
│   │   │   ├── hooks/
│   │   │   │   ├── usePermission.ts
│   │   │   │   └── useRole.ts
│   │   │   ├── guards/
│   │   │   │   ├── PermissionGuard.tsx
│   │   │   │   └── RoleGuard.tsx
│   │   │   ├── decorators/
│   │   │   │   ├── RequirePermission.tsx
│   │   │   │   └── RequireRole.tsx
│   │   │   └── index.ts
│   │   └── package.json
│   │
│   ├── shared-utils/                   # Utilitários comuns
│   │   ├── src/
│   │   │   ├── formatting.ts
│   │   │   ├── validation.ts
│   │   │   ├── date-utils.ts
│   │   │   └── index.ts
│   │   └── package.json
│   │
│   └── module-loader/                  # Dynamic module loading
│       ├── src/
│       │   ├── loader.ts
│       │   ├── types.ts
│       │   └── index.ts
│       └── package.json
│
├── config/                             # Configurações globais
│   ├── vite.shared.config.ts           # Config compartilhada
│   ├── vitest.config.ts
│   ├── tailwind.config.ts
│   ├── tsconfig.base.json
│   └── eslint.config.js
│
├── .github/
│   └── workflows/
│       ├── lint.yml
│       ├── type-check.yml
│       ├── test.yml
│       └── build.yml
│
├── pnpm-workspace.yaml
├── package.json (root)
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
    "strictBindCallApply": true,
    "strictPropertyInitialization": true,
    "noImplicitThis": true,
    "alwaysStrict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": true
  }
}
```

### 4.2 Nomeação de Ficheiros e Pastas

| Tipo | Exemplo | Padrão |
|------|---------|--------|
| Componente React | `SampleForm.tsx` | PascalCase |
| Hook customizado | `useSamples.ts` | camelCase, prefixo `use` |
| Service/Utilitário | `sample.service.ts` | camelCase, sufixo tipo |
| Type/Interface | `sample.types.ts` | camelCase, sufixo `types` |
| Store (Zustand/XState) | `sample.store.ts` | camelCase, sufixo `store` |
| Pasta | `components`, `hooks`, `pages` | kebab-case ou camelCase |

### 4.3 Exemplo: Componente React com TypeScript

```typescript
// src/modules/lims/components/SampleForm.tsx

import React, { FC, useCallback } from 'react';
import { useForm } from 'react-hook-form';
import { z } from 'zod';
import { zodResolver } from '@hookform/resolvers/zod';
import { Button, Input, Select } from '@smartlab/ui-kit';
import { useAuth } from '@smartlab/auth';
import { useSampleStore } from '../store/sample.store';

// Tipos
interface SampleFormProps {
  onSuccess?: (sampleId: string) => void;
}

// Validação
const createSampleSchema = z.object({
  sampleCode: z.string().min(1).max(50),
  materialId: z.string().uuid(),
  quantity: z.number().positive(),
  samplingDate: z.string().datetime(),
});

type CreateSampleFormData = z.infer<typeof createSampleSchema>;

// Componente
export const SampleForm: FC<SampleFormProps> = ({ onSuccess }) => {
  const { user } = useAuth();
  const { createSample, loading, error } = useSampleStore();

  const {
    register,
    handleSubmit,
    formState: { errors, isSubmitting },
    reset,
  } = useForm<CreateSampleFormData>({
    resolver: zodResolver(createSampleSchema),
  });

  const onSubmit = useCallback(
    async (data: CreateSampleFormData) => {
      try {
        const sampleId = await createSample({
          ...data,
          createdBy: user.id,
        });
        reset();
        onSuccess?.(sampleId);
      } catch (err) {
        console.error('Failed to create sample', err);
      }
    },
    [createSample, user.id, reset, onSuccess]
  );

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <Input
        label="Sample Code"
        {...register('sampleCode')}
        error={errors.sampleCode?.message}
      />
      <Select
        label="Material"
        {...register('materialId')}
        error={errors.materialId?.message}
      />
      <Input
        label="Quantity"
        type="number"
        {...register('quantity', { valueAsNumber: true })}
        error={errors.quantity?.message}
      />
      <Button type="submit" disabled={isSubmitting || loading}>
        {loading ? 'Creating...' : 'Create Sample'}
      </Button>
      {error && <div className="text-red-500">{error}</div>}
    </form>
  );
};
```

### 4.4 Exemplo: Zustand Store

```typescript
// src/modules/lims/store/sample.store.ts

import { create } from 'zustand';
import { SmartLabApiClient } from '@smartlab/api-client';
import type { Sample, CreateSampleDto } from '../types/lims.types';

interface SampleStore {
  samples: Sample[];
  loading: boolean;
  error: string | null;

  createSample: (dto: CreateSampleDto) => Promise<string>;
  fetchSamples: (filters?: Record<string, any>) => Promise<void>;
  getSample: (id: string) => Sample | undefined;
  clearError: () => void;
}

export const useSampleStore = create<SampleStore>((set, get) => ({
  samples: [],
  loading: false,
  error: null,

  createSample: async (dto: CreateSampleDto) => {
    set({ loading: true, error: null });
    try {
      const api = new SmartLabApiClient();
      const sample = await api.lims.createSample(dto);
      set((state) => ({
        samples: [sample, ...state.samples],
        loading: false,
      }));
      return sample.id;
    } catch (err) {
      const message = err instanceof Error ? err.message : 'Failed to create sample';
      set({ error: message, loading: false });
      throw err;
    }
  },

  fetchSamples: async (filters) => {
    set({ loading: true, error: null });
    try {
      const api = new SmartLabApiClient();
      const samples = await api.lims.listSamples(filters);
      set({ samples, loading: false });
    } catch (err) {
      const message = err instanceof Error ? err.message : 'Failed to fetch samples';
      set({ error: message, loading: false });
    }
  },

  getSample: (id: string) => {
    return get().samples.find((s) => s.id === id);
  },

  clearError: () => set({ error: null }),
}));
```

### 4.5 Exemplo: Hook Customizado

```typescript
// src/modules/lims/hooks/useSamples.ts

import { useEffect, useCallback } from 'react';
import { useSampleStore } from '../store/sample.store';
import { useAuth } from '@smartlab/auth';

export interface UseSamplesOptions {
  materialId?: string;
  status?: string;
  autoLoad?: boolean;
}

export const useSamples = (options: UseSamplesOptions = {}) => {
  const { autoLoad = true } = options;
  const { user } = useAuth();
  const {
    samples,
    loading,
    error,
    fetchSamples,
    createSample,
    getSample,
  } = useSampleStore();

  const refetch = useCallback(() => {
    fetchSamples({
      materialId: options.materialId,
      status: options.status,
    });
  }, [fetchSamples, options.materialId, options.status]);

  useEffect(() => {
    if (autoLoad && user) {
      refetch();
    }
  }, [autoLoad, user, refetch]);

  return {
    samples,
    loading,
    error,
    refetch,
    createSample,
    getSample,
  };
};
```

---

## 5. Fluxo de Desenvolvimento

### 5.1 Iniciar Nova Feature

```bash
# 1. Atualizar branch main
git checkout main
git pull origin main

# 2. Criar feature branch
git checkout -b feature/LIMS-123-sample-approval

# 3. Abrir novo workspace no editor (opcional)
code .

# 4. Instalar deps (primeira vez)
pnpm install

# 5. Iniciar dev server (escolha seu app)
cd apps/lims
pnpm dev

# Acesso: http://localhost:5173 (ou porta configurada)
```

### 5.2 Desenvolvimento Iterativo

```bash
# Verificar tipos
pnpm type-check

# Linting
pnpm lint
pnpm lint:fix

# Testes locais
pnpm test

# Testes com coverage
pnpm test:coverage
```

### 5.3 Integração com Shell

**Regra de ouro**: um módulo **nunca** acessa `localStorage`, `sessionStorage` ou faz login próprio.

```typescript
// ✅ CORRETO: Usar contexto fornecido pelo Shell
const MyModule = ({ moduleContext }: { moduleContext: ModuleContext }) => {
  const { user, permissions, api, eventBus } = moduleContext;

  return <div>Welcome {user.email}</div>;
};

// ❌ ERRADO: Acessar diretamente localStorage
const MyModule = () => {
  const token = localStorage.getItem('auth_token'); // PROIBIDO!
  return <div>...</div>;
};
```

### 5.4 Submeter PR

```bash
# 1. Commits descritivos
git add .
git commit -m "feat(LIMS): add sample approval workflow

- Implement approval form component
- Add validation rules (Zod schema)
- Emit SAMPLE_APPROVED event
- Add unit tests for approval logic

Closes #LIMS-123"

# 2. Push
git push origin feature/LIMS-123-sample-approval

# 3. Criar PR no GitHub (será verificado automaticamente por CI)
# CI checks:
#  - ESLint
#  - Type check (tsc)
#  - Unit tests
#  - Build test

# 4. Review + merge em main
```

---

## 6. Padrões de Integração com API Central

### 6.1 Usar SmartLabApiClient (SDK interno)

```typescript
// ✅ CORRETO
import { SmartLabApiClient } from '@smartlab/api-client';

const api = new SmartLabApiClient();
const sample = await api.lims.getSample(sampleId);

// ❌ ERRADO: Fazer fetch direto
const response = await fetch(`/api/v1/lims/samples/${sampleId}`);
```

### 6.2 Integração com EventBus

```typescript
// Publicar evento
eventBus.publish({
  type: 'SAMPLE_APPROVED',
  aggregateId: sampleId,
  tenantId: tenant.id,
  actorId: user.id,
  timestamp: new Date(),
  data: { sampleId, approvedBy: user.id },
});
```

---

## 7. Testes

### 7.1 Unit Tests (Vitest)

```typescript
// src/modules/lims/hooks/__tests__/useSamples.test.ts

import { describe, it, expect, beforeEach, vi } from 'vitest';
import { renderHook, waitFor } from '@testing-library/react';
import { useSamples } from '../useSamples';
import * as api from '../../services/sample.service';

vi.mock('../../services/sample.service');

describe('useSamples', () => {
  beforeEach(() => {
    vi.clearAllMocks();
  });

  it('should load samples on mount when autoLoad is true', async () => {
    const mockSamples = [{ id: '1', sampleCode: 'S001' }];
    vi.mocked(api.fetchSamples).mockResolvedValue(mockSamples);

    const { result } = renderHook(() => useSamples({ autoLoad: true }));

    await waitFor(() => {
      expect(result.current.samples).toEqual(mockSamples);
      expect(result.current.loading).toBe(false);
    });
  });

  it('should handle errors', async () => {
    const error = new Error('API error');
    vi.mocked(api.fetchSamples).mockRejectedValue(error);

    const { result } = renderHook(() => useSamples({ autoLoad: true }));

    await waitFor(() => {
      expect(result.current.error).toBe('API error');
    });
  });
});
```

### 7.2 E2E Tests (Playwright)

```typescript
// tests/e2e/lims/sample-creation.spec.ts

import { test, expect } from '@playwright/test';

test.describe('LIMS Sample Creation', () => {
  test.beforeEach(async ({ page }) => {
    // Login
    await page.goto('http://localhost:5173/login');
    await page.fill('input[name="email"]', 'user@example.com');
    await page.fill('input[name="password"]', 'password');
    await page.click('button:has-text("Login")');
    await page.waitForNavigation();
  });

  test('should create a sample', async ({ page }) => {
    await page.click('text=LIMS');
    await page.click('text=Create Sample');

    await page.fill('input[name="sampleCode"]', 'S001');
    await page.selectOption('select[name="materialId"]', 'MAT-001');
    await page.fill('input[name="quantity"]', '100');

    await page.click('button:has-text("Create Sample")');

    await expect(page).toHaveURL(/.*samples/);
    await expect(page.locator('text=S001')).toBeVisible();
  });
});
```

---

## 8. Build e Deploy

### 8.1 Build local

```bash
# Build tudo
pnpm build

# Build módulo específico
cd apps/lims
pnpm build

# Resultado: dist/ folder pronto para deploy
```

### 8.2 Envs e configuração

```env
# .env (não commit; usar .env.example)
VITE_API_BASE_URL=http://localhost:3000
VITE_TENANT_ID=dev-tenant
VITE_ENV=development
VITE_LOG_LEVEL=debug
```

### 8.3 Docker build

```dockerfile
# Dockerfile (exemplo simplificado)
FROM node:20-alpine AS builder
WORKDIR /app
COPY pnpm-lock.yaml .
RUN npm install -g pnpm && pnpm install --frozen-lockfile
COPY . .
RUN pnpm build

FROM nginx:alpine
COPY --from=builder /app/apps/shell/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/nginx.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

---

## 9. Checklist de Qualidade (Antes de submeter PR)

- [ ] Code segue TypeScript strict mode (sem `any`).
- [ ] ESLint passa sem warnings.
- [ ] Type-check passa (`pnpm type-check`).
- [ ] Unit tests com >80% coverage.
- [ ] Componentes testados com React Testing Library ou Playwright.
- [ ] Sem console.logs ou debugger statements.
- [ ] API calls usam SmartLabApiClient SDK.
- [ ] Eventos publicados com tipos corretos.
- [ ] Acessibilidade (a11y): WCAG AA mínimo.
- [ ] Responsive: testado em mobile, tablet, desktop.
- [ ] Commit messages descritivas e referem issue (#123).
- [ ] Documentação adicionada (JSDoc para funções públicas).

---

## 10. Troubleshooting Comum

| Problema | Solução |
|----------|---------|
| Módulo não carrega | Verificar `module.tsx` exports; validar permissões em Shell |
| Types não encontradas | Verificar imports; rodar `pnpm type-check` |
| Cache issue | `rm -rf node_modules pnpm-lock.yaml && pnpm install` |
| Port já em uso | `lsof -i :5173` e kill processo; ou usar porta diferente |
| Event não dispara | Verificar schema; confirmar que EventBus está injetado |
| API call falha | Verificar token; validar baseURL no .env |
| Build fails | Rodar `pnpm build` localmente; verificar tipos |

---

## 11. Referências e Recursos

- [React Docs](https://react.dev)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)
- [Zod Docs](https://zod.dev)
- [UI5 Web Components](https://sap.github.io/ui5-webcomponents/)
- [Zustand Docs](https://github.com/pmndrs/zustand)
- [Vitest Docs](https://vitest.dev)
- [Playwright Docs](https://playwright.dev)

---

## 12. Aprovação e Versionamento

**Última atualização**: 29 de janeiro de 2026
**Versão**: 1.0
**Responsável**: Tech Lead / Architecture Team

Alterações neste SOP requerem aprovação de tech lead.

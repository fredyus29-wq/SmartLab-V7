# Frontend Diagram — SmartLab

Resumo do diagrama do frontend e regras operacionais para a `Shell` e módulos.

## Visão Geral
- Único ponto de entrada: `Shell App` (Core UI) que provê autenticação, contexto do tenant, layout base e menu dinâmico.
- Módulos (LIMS, QMS, FSMS, TMS, Analytics, etc.) são pacotes plugáveis carregados via lazy loading e feature flags.
- Regra de ouro: o utilizador está sempre no SmartLab — nunca entra numa "app" separada.

## Componentes chave
- Shell App: Auth, Tenant Context, Menu Builder, Router Core, Design System (UI5 Web Components).
- Módulos: pacotes isolados sem lógica de auth, sem acesso ao token e sem controle de licença.
- Comunicação: `ModuleContext` injetado pelo Shell (tenantId, user, permissions, apiClient).

## Carregamento e Licenciamento
- Lazy loading + feature flags para reduzir bundle e permitir upsell por módulos.
- Menu dinâmico construído a partir do payload da API (tenant.modules + roles).

## UX e Design
- UX industrial (SAP‑like): densidade alta, tipografia consistente, grids compactos.
- Componentes básicos: `ui5-shellbar`, `ui5-side-navigation`, `ui5-card` (compact mode), `ui5-table`, `ui5-dialog`.

## Contratos Shell ↔ Módulos
- Módulos não devem criar layout global, gerir auth ou armazenar estado global.
- Módulos usam serviços `dialogs` e `notifications` fornecidos pelo Shell.
- Naming & DB Governance: módulos consultam `SmartLabSchemaRegistry` antes de criar entidades.

## Eventos importantes
- Exemplos: `LOT_STARTED`, `LOT_COMPLETED`, `TRAINING_COMPLETED`, `NC_CREATED`.
- Event bus usado para integração entre MES, LIMS, QMS e FSMS.

## Benefícios
- UX unificada, governança forte, venda modular real e menor risco de conflito entre módulos.

Origem: `Frontend digram` (conteúdo importado e resumido).

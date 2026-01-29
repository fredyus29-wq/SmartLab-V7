# Module Contract — SmartLab

Este documento sintetiza o contrato técnico entre o Shell (app central) e os módulos, extraído do ficheiro `Cibtrato tecnico Shell/modulos`.

## Objetivo
Regras obrigatórias para comunicação, integração, governança, naming e UI — para evitar duplicação e garantir rastreabilidade e conformidade (ISO).

## Módulos oficiais
Core modules (nível enterprise):
- MES
- LIMS
- QMS
- FSMS
- TMS (Training Hub / Notebook)
- Materials
- Analytics & Reports
- Tools (AI & Quality Tools)

Todos os módulos implementam o mesmo contrato base.

## Interface conceitual (exemplo)
```ts
export interface SmartLabModule {
  id: ModuleId;
  name: string;
  version: string;

  register(ctx: ModuleContext): ModuleRuntime;

  routes: ModuleRoute[];
  permissions: Permission[];
  menu?: MenuDefinition;
}
```

## Contexto fornecido pelo Shell (imutável)
O `ModuleContext` é injetado pelo Shell e contém:
- tenant (id, name, regulatoryProfile)
- user (id, role, competencies)
- api: SmartLabApiClient (obrigatório usar)
- eventBus: EventBus
- ai: SmartLabAI
- ui: dialogs, notifications

Proibições claras:
- Criar cliente HTTP próprio
- Gerir autenticação localmente
- Armazenar estado global do sistema

## Event Bus e eventos padrões
O `eventBus` é o mecanismo de rastreabilidade e integração entre módulos. Eventos padronizados (exemplos):
- LOT_STARTED
- LOT_COMPLETED
- ANALYSIS_FAILED
- NC_CREATED
- CAPA_CLOSED
- CIP_EXECUTED
- TRAINING_COMPLETED

Exemplo de fluxo industrial:
MES emite `LOT_COMPLETED` → LIMS agenda análises → QMS cria checkpoints → FSMS valida HACCP.

## Contratos/responsabilidades por módulo (resumo)
- MES: ordens de produção, turnos, linhas, CIP, status real-time. EMITE eventos, não decide qualidade.
- LIMS: ensaios, resultados, aprovação técnica, tendências analíticas. Nunca para produção diretamente.
- QMS: NC, CAPA, 8D, auditorias, competências. Atua como árbitro de decisões de qualidade.
- FSMS: HACCP, PCC, monitorização sanitária. Consome dados de MES + LIMS.
- TMS: cursos, quizzes, avaliações, certificados. Publica `TRAINING_COMPLETED` para QMS.

## Padrões de UI
- O módulo não define layout global (header, sidebar, shell).
- Renderiza apenas páginas internas usando componentes `UI5`.
- Seguir grid e padrões fornecidos pelo Shell; usar serviços `dialogs` e `notifications` do Shell.

## Naming & DB Governance
Antes de criar tabelas/enums/entidades o módulo deve consultar o `SmartLabSchemaRegistry`.
- Se existir: reutilizar
- Se conflitar: bloqueado
- Se substituir: marcar deprecado e remover via processo controlado

## Regras de evolução e extração
- Módulo é bounded context plugável, não aplicação independente.
- Extrair para microservice apenas quando:
  - demanda de escala independente (CPU/IO)
  - necessidade de DB separado
  - ciclo de deploy totalmente independente

## Benefícios esperados
- Redução de duplicação e conflitos de schema
- Rastreabilidade e evidência para auditoria
- Time ownership claro e integridade do produto
- Facilita venda modular sem perda de UX unificada

## Referências
Origem: `Cibtrato tecnico Shell/modulos` (conteúdo importado e resumido).
# Roadmap de Implementação Inicial — SmartLab

## Objetivo
Fornecer passos concretos para iniciar desenvolvimento e entrega do MVP modular.

## Sprint 0 — Preparação
- Validar prioridades de módulos com stakeholders.
- Configurar monorepo básico (apps + packages).
- Configurar devcontainer e pipeline CI mínima.

## Sprint 1 — Core API mínimo + Shell
- Implementar NestJS skeleton com Auth, RBAC e Audit Trail.
- Criar frontend `shell` com login e menu dinâmico.
- Criar endpoints iniciais para módulos (LIMS, QMS minimal CRUD).

## Sprint 2 — LIMS MVP
- Amostras: criação e lifecycle básico.
- Resultados: registo e visualização.
- COA generator simples (template PDF).

## Sprint 3 — QMS MVP
- Gestão documental básica (upload, versão, aprovação simple).
- NC creation and simple CAPA flow.
- Integração inicial com Training Hub events.

## Sprint 4 — TMS (Training Hub) MVP
- Course authoring básico e play.
- Quiz simples e certificação PDF.
- IA: integrar worker de exemplo para auto‑slides (poC).

## Observações
- Priorizar contratos de API estáveis (OpenAPI) para permitir extração futura de serviços.
- Avaliar telemetria e políticas de backup em Sprint 0/1.


# SmartLab-V7
O SmartLab é um sistema industrial modular, orientado a domínios, com API centralizada, projetado para ambientes regulados (ISO, FSMS, QMS, LIMS, normas nacionais de Angola).
# SmartLab – Arquitetura Técnica Final


Objetivos-chave:

* Venda por **módulos independentes** ou **suite completa**
* Alta **integridade de dados** e **auditabilidade**
* UX madura (SAP-like) com **UI5 Web Components**
* Automação inteligente de tarefas repetitivas via **IA aplicada**
* Base sólida para **analytics avançado** e **relatórios regulatórios**

---

## 2. Padrão Arquitetural Escolhido

### ✔ Aplicação Modular com API Centralizada (Domain-Oriented Monolith)

**Por quê?**

* Ambiente industrial regulado exige **consistência forte**
* Menos overhead operacional que microserviços
* Mais fácil de auditar, validar e certificar
* Ideal para fase 1–3 do produto (escala controlada)

A arquitetura é preparada para **evoluir para microserviços** no futuro, se necessário.

---

## 3. Arquitetura de Alto Nível

```
┌────────────────────────────────────────────┐
│                Frontend Apps               │
│ React + UI5 Web Components                 │
│ (FSMS | QMS | LIMS | TMS | Analytics)      │
└───────────────────▲────────────────────────┘
                    │ REST / GraphQL
┌───────────────────┴────────────────────────┐
│           SmartLab Central API              │
│        NestJS (Domain Modular Core)         │
│                                            │
│  ┌──────────────┐  ┌───────────────────┐  │
│  │ Auth & RBAC  │  │ Audit & Compliance │  │
│  └──────────────┘  └───────────────────┘  │
│                                            │
│  FSMS | QMS | LIMS | TMS | Reports | AI     │
│  (Isolated Domains, Shared Kernel)          │
└───────────────────▲────────────────────────┘
                    │
┌───────────────────┴────────────────────────┐
│               Data Layer                    │
│ PostgreSQL | Timescale | Object Storage     │
│ Event Log | Versioned Records               │
└────────────────────────────────────────────┘
```

---

## 4. Stack Tecnológica

### Backend

* **Node.js + NestJS** (arquitetura hexagonal)
* **TypeScript (strict mode)**
* **PostgreSQL** (dados transacionais)
* **TimescaleDB** (séries temporais: produção, sensores, KPIs)
* **Redis** (cache, filas leves)
* **BullMQ** (jobs, automações, IA assíncrona)
* **OpenAPI + Zod** (contratos fortes)

### Frontend

* **React**
* **UI5 Web Components** (base visual SAP-like)
* **State Machine (XState)** para fluxos críticos
* **Design Tokens Industriais** (escala, densidade, acessibilidade)

### IA & Automação

* **LLMs (OpenAI / Azure / local)**
* **RAG** sobre SOPs, normas, registros históricos
* **Workers de IA** desacoplados da API core

---

## 5. Módulos Principais (Arquitetura por Domínio)

### 5.1 LIMS (Laboratory Information Management)

**Aplicações:**

* Execução de análises físico-químicas e microbiológicas
* Controle de amostras, lotes e métodos
* Validação, revisão e aprovação

**Serviços internos:**

* Sample Service
* Test Method Service
* Result Validation Engine
* COA Generator
* Trend & OOS Detection (IA)

**IA aplicada:**

* Pré-validação de resultados
* Sugestão de causa raiz (OOS)
* Autopreenchimento de relatórios

---

### 5.2 QMS (Quality Management System)

**Aplicações:**

* Não conformidades
* CAPA
* Gestão documental
* Matriz de competências

**Serviços internos:**

* Document Control
* Change Management
* CAPA Workflow Engine
* Training Records Sync (TMS)

**IA aplicada:**

* Classificação automática de NCs
* Sugestão de ações corretivas
* Avaliação de impacto regulatório

---

### 5.3 FSMS (Food Safety Management System)

**Aplicações:**

* HACCP / APPCC
* PRPs, OPRPs
* Auditorias internas e externas

**Serviços internos:**

* Hazard Analysis Engine
* CCP Monitoring
* Audit Builder
* Compliance Evidence Manager

**IA aplicada (crítica):**

* Geração assistida de planos HACCP
* Checklists dinâmicos por tipo de auditoria
* Detecção de gaps de conformidade

---

### 5.4 TMS – Training Management System (Centro de Formação)

Inspirado no **Google NotebookLM**, mas adaptado ao contexto industrial.

**Funcionalidades:**

* Criação de cursos (slides, texto, flashcards, quizzes)
* Avaliação automática
* Emissão de certificados
* Integração direta com QMS

**Serviços internos:**

* Course Authoring Engine
* Quiz & Scoring Engine
* Certificate Generator
* Competency Sync Service

**IA aplicada:**

* Geração de slides a partir de SOPs
* Criação automática de quizzes
* Avaliação semântica de respostas abertas

---

### 5.5 Reports & Analytics

**Aplicações:**

* Dashboards operacionais
* Relatórios regulatórios
* KPIs industriais

**Stack:**

* SQL + Views versionadas
* Camada semântica
* Exportação PDF/Excel

**IA aplicada:**

* Insights automáticos
* Anomalias operacionais
* Análise preditiva

---

## 6. Governança e Compliance

* Audit Trail imutável
* Versionamento de dados críticos
* Assinaturas eletrônicas
* RBAC + Contextual Permissions
* Logs preparados para auditorias ISO

---

## 7. Evolução Futura

* Extração de módulos para microserviços (quando escalar)
* Event-driven com Kafka
* Edge computing (sensores)
* Modelos de IA locais (on-prem)

---

## 8. Conclusão

Esta arquitetura entrega:

* Robustez industrial
* UX madura
* Automação inteligente
* Base sólida para certificações



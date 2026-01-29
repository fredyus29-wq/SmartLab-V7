````markdown

# 2️⃣ ENTREGA 2 — CONTRATO TÉCNICO ENTRE SHELL ↔ MÓDULOS

*(incluindo MES)*

## 🎯 Objetivo

Definir **regras duras** para:

* Comunicação
* Integração
* Governança
* Evitar duplicação
* Garantir robustez industrial (ISO, auditoria, rastreabilidade)

Se isso falhar, o sistema vira um Frankenstein. Aqui não.

---

## 1️⃣ Lista oficial de módulos (nível enterprise)

### 🔹 Core Modules (primeira classe)

* **MES** – Execução de Manufatura
* **LIMS** – Laboratório
* **QMS** – Qualidade
* **FSMS** – Segurança Alimentar
* **TMS** – Training / Centro de Formação
* **Materials** – MP, embalagens, reagentes, fornecedores
* **Analytics & Reports**
* **Tools (AI & Quality Tools)**

Todos seguem **o mesmo contrato**.

---

## 2️⃣ Regra de ouro (não negociável)

> **Módulo NÃO é uma aplicação independente.**
> Ele é um **bounded context plugável** dentro do SmartLab.

---

## 3️⃣ Contrato obrigatório: `Module Contract`

### Interface base (conceitual)

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

---

## 4️⃣ Contexto fornecido pelo Shell (imutável)

```ts
interface ModuleContext {
  tenant: {
    id: string;
    name: string;
    regulatoryProfile: 'AO' | 'SADC' | 'GLOBAL';
  };

  user: {
    id: string;
    role: Role;
    competencies: string[];
  };

  api: SmartLabApiClient;

  eventBus: EventBus;

  ai: SmartLabAI;

  ui: {
    dialogs: DialogService;
    notifications: NotificationService;
  };
}
```

🚫 **Proibido**:

* Criar cliente HTTP próprio
* Gerir auth
* Armazenar estado global

---

## 5️⃣ Event Bus (integração MES ↔ LIMS ↔ QMS)

### Exemplo real (industrial)

```
MES → Produção concluída
        ↓
Event: LOT_COMPLETED
        ↓
LIMS → agenda análises
        ↓
QMS → cria checkpoints
        ↓
FSMS → valida HACCP
```

### Eventos padronizados

```ts
LOT_STARTED
LOT_COMPLETED
ANALYSIS_FAILED
NC_CREATED
CAPA_CLOSED
CIP_EXECUTED
TRAINING_COMPLETED
```

📌 **Eventos = rastreabilidade ISO**

---

## 6️⃣ Contrato específico por módulo (resumo)

### 🔧 MES

Responsável por:

* Ordens de produção
* Turnos
* Linhas
* CIP
* Status em tempo real

MES **emite eventos**, não toma decisão de qualidade.

---

### 🧪 LIMS

Responsável por:

* Ensaios
* Resultados
* Aprovação técnica
* Tendências analíticas

Nunca “para produção” diretamente — **QMS decide**.

---

### 📋 QMS

Responsável por:

* NC
* CAPA
* 8D
* Auditorias
* Competências (via TMS)

É o **árbitro do sistema**.

---

### 🧯 FSMS

Responsável por:

* HACCP
* PCC
* Monitorização
* Auditorias sanitárias

Consome dados do MES + LIMS.

---

### 🎓 TMS (Training / Notebook-like)

Responsável por:

* Cursos
* Flashcards
* Quizzes
* Avaliações
* Certificados

Publica:

```ts
TRAINING_COMPLETED
```

Consumido pelo QMS → matriz de competências.

---

## 7️⃣ Padrão de UI obrigatório (UX industrial)

### O módulo **não**:

* Cria layout
* Define sidebar
* Define header

### O módulo **apenas**:

* Renderiza páginas internas
* Usa componentes UI5
* Segue grid padrão
* Usa dialogs fornecidos pelo Shell

---

## 8️⃣ Naming & DB Governance (anti-caos)

Antes de:

* Criar tabela
* Criar enum
* Criar entidade

O módulo **deve consultar**:

```ts
SmartLabSchemaRegistry
```

Se existir:
✔ Reutiliza
Se conflitar:
❌ Bloqueado
Se substituir:
⚠️ Deprecia → remove

---

## 9️⃣ Benefício direto para o time

✔ Cada agente sabe exatamente:

* Onde codar
* O que pode fazer
* O que é proibido
* Como integrar

✔ Menos bugs
✔ Menos retrabalho
✔ Auditoria feliz
✔ Sistema vendável por módulo

---



---



````
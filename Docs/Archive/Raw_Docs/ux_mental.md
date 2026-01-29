````ini

## 1️⃣ Como os módulos “aparecem” no frontend (experiência do cliente)

### 🎯 Princípio base

> **Para o cliente que compra tudo, o SmartLab parece UM sistema único.**
> Nada de sensação de “apps colados”.

### UX mental do utilizador

* **1 login**
* **1 dashboard principal**
* **N módulos habilitados dinamicamente**
* Navegação consistente (mesma shell, mesma UI, mesma lógica)

---

## 2️⃣ Modelo recomendado: Frontend Modular com Shell Central

### ✅ Arquitetura ideal no frontend

**Micro-frontends lógicos (não micro-frontends caóticos)**

👉 Tecnicamente:

* **1 Shell App (Core UI)**
* **Módulos carregados por feature flag / licença**
* **Mesmo deploy OU deploys independentes (dependendo da maturidade)**

---

## 3️⃣ Como o utilizador acessa os módulos (caso compre tudo)

### Fluxo real

1. Utilizador faz login
2. API retorna:

   ```json
   {
     "tenantId": "FAB-001",
     "modules": ["LIMS", "QMS", "FSMS", "TMS", "ANALYTICS"],
     "roles": ["QA_MANAGER"]
   }
   ```
3. O **Frontend Shell**:

   * Monta o menu automaticamente
   * Ativa rotas
   * Ativa permissões
   * Carrega apenas o que existe

### Exemplo de menu (SAP-like)

```
Dashboard
├── LIMS
│   ├── Amostras
│   ├── Análises
│   └── COA
├── QMS
│   ├── Documentos
│   ├── CAPA
│   └── Competências
├── FSMS
│   ├── HACCP
│   ├── Auditorias
├── TMS
│   ├── Cursos
│   ├── Avaliações
│   └── Certificados
└── Analytics
```

👉 **Tudo parece um único software enterprise.**

---

## 4️⃣ Cada módulo tem deploy separado ou não?

### Fase 1 (recomendada agora)

✔ **Monorepo + build único**

* Mais simples
* Menos risco
* Mais controle (ISO-friendly)

### Fase 2 (escala)

✔ **Deploys independentes opcionais**

* Quando clientes grandes pedirem
* Quando equipas crescerem

---

## 5️⃣ Estrutura IDEAL de monorepo (frontend)

### 🧠 Pensando como SAP / Siemens / Dassault

```txt
smartlab/
├── apps/
│   ├── shell/                 # App principal (layout, auth, menu)
│   ├── lims/
│   ├── qms/
│   ├── fsms/
│   ├── tms/
│   └── analytics/
│
├── packages/
│   ├── ui-kit/                # UI5 Web Components wrappers
│   ├── design-tokens/         # cores, spacing, densidade
│   ├── auth/                  # RBAC, guards
│   ├── api-client/            # SDK tipado da API central
│   ├── permissions/           # regras de acesso
│   └── shared-utils/
│
├── config/
│   ├── routes.ts              # roteamento por módulo
│   ├── modules.ts             # definição de módulos
│   └── licensing.ts           # lógica de ativação
│
├── tooling/
│   ├── lint
│   ├── build
│   └── ci
└── package.json
```

---

## 6️⃣ Como funciona o “Shell App” (cérebro do frontend)

O **Shell** é obrigatório. Ele faz:

* Autenticação
* Contexto do tenant
* Tema industrial
* Layout base
* Menu
* Navegação
* Guards de acesso
* Lazy loading dos módulos

### Cada módulo:

* Não sabe **quem é o tenant**
* Não sabe **licença**
* Não sabe **auth**
* Apenas executa sua função

👉 **Isso é maturidade industrial.**

---

## 7️⃣ Como um cliente que comprou “tudo” vê isso?

### Para ele:

* Um único sistema
* Um único URL:

  ```
  app.smartlab.io
  ```
* Um único login
* Tudo integrado

### Para vocês:

* Controle total de:

  * O que ativar
  * Para quem
  * Quando
  * Em que plano

---

## 8️⃣ E se o cliente comprar só LIMS + QMS?

Nada muda tecnicamente.

* O menu só mostra LIMS + QMS
* As rotas dos outros módulos **nem existem**
* O código pode até estar no build, mas **inativo**
* Ou nem é carregado (lazy + feature flags)

---

## 9️⃣ Benefícios desse modelo

### Técnicos

✅ Menos bugs
✅ Tipagem forte
✅ UX consistente
✅ Menos retrabalho
✅ Mais fácil certificar

### Produto / Negócio

✅ Venda modular real
✅ Upsell simples
✅ Onboarding rápido
✅ Cliente sente software “premium”

---

## 🔥 Conclusão direta

👉 **Frontend deve ser:**

* Modular internamente
* Unificado externamente

👉 **Shell App + módulos plugáveis**
👉 **Monorepo no início, pronto para split depois**


````
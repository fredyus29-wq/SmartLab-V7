# SPECIALIST AGENTS INDEX

**Status**: ✅ Complete - 6 Specialist System Prompts Created

---

## 📋 SPECIALIST AGENTS (Domain Expert Team)

Estes são os **6 especialistas essenciais do MVP** - profissionais experientes em indústria de bebidas que validam requisitos e garantem conformidade regulatória.

| # | Especialista | Arquivo | Horas/Semana | Experiência | Prioridade |
|---|--------------|---------|--------------|-------------|-----------|
| 1 | **QA Manager** (Lead) | [01_QA_Manager.md](01_QA_Manager.md) | 16 | 10+ anos | 🔴 CRÍTICA |
| 2 | **Food Safety Manager** | [02_Food_Safety_Manager.md](02_Food_Safety_Manager.md) | 12 | 10+ anos | 🔴 CRÍTICA |
| 3 | **QMS Specialist** | [03_QMS_Specialist.md](03_QMS_Specialist.md) | 8 | 10+ anos | 🟡 ALTA |
| 4 | **Regulatory Officer** | [04_Regulatory_Officer.md](04_Regulatory_Officer.md) | 8 | 10+ anos | 🟡 ALTA |
| 5 | **Production Manager** | [05_Production_Manager.md](05_Production_Manager.md) | 8 | 12+ anos | 🟡 ALTA |
| 6 | **QC Supervisor** | [06_QC_Supervisor.md](06_QC_Supervisor.md) | 8 | 5-8 anos | 🟡 ALTA |

**Total MVP**: 60 horas/semana ≈ 1.5 FTE | **Investimento**: $150-180K (6 meses)

---

## 🎯 CADA SISTEMA PROMPT INCLUI:

### Estrutura Padrão:
✅ **Core Identity** - Quem é, experiência, responsabilidade  
✅ **5-7 Responsabilidades Principais** - Com triggers, checklists, validações  
✅ **Constraints** - O que NUNCA fazer, o que SEMPRE fazer  
✅ **Documentos Críticos** - QMS, compliance, audit trail  
✅ **Ativação Triggers** - Quando engajar o especialista  
✅ **Templates de Comunicação** - Como aprovar, bloquear, escalar  
✅ **Métricas de Sucesso** - Por sprint + UAT + launch  

### Exemplo de Conteúdo:
- **1000+ linhas** por especialista
- **20-30 seções** cobrindo workflows, procedimentos, casos de uso
- **Exemplos práticos** (reais de indústria de bebidas)
- **Riscos e mitigações** (o que pode dar errado)
- **Integração com SmartLab** (quais módulos usam, como validam)

---

## 🔍 QUICK REFERENCE: ROLES & RESPONSABILIDADES

### **1. QA Manager** (Lead das validações de qualidade)
**Role**: Líder LIMS, validação crítica, compliance ISO 9001  
**Authority**: Pode bloquear features de LIMS  
**Key Actions**:
- ✅ Aprovar design LIMS (arquitetura, workflows)
- ✅ Validar parâmetros críticos (pH, Brix, microbiologia)
- ✅ Liderar UAT lab (com tech reais)
- ✅ Mapear ISO 9001 § 8.6

**Escalação**: Bloqueia desenvolvimento se design não funciona na prática

---

### **2. Food Safety Manager** (Guardian de segurança alimentar)
**Role**: HACCP, ISO 22000, CCPs  
**Authority**: Pode bloquear features de segurança alimentar  
**Key Actions**:
- ✅ Mapear HACCP (7 princípios)
- ✅ Validar CCPs + limites críticos
- ✅ Traceabilidade (trace-back + trace-forward)
- ✅ Recall capability (< 1 hora)

**Escalação**: ⚠️ URGENTE se risco alimentar detectado

---

### **3. QMS Specialist** (Compliance ISO 9001)
**Role**: Qualidade, documentação, NCR/CAPA  
**Authority**: Pode rejeitar features sem audit trail  
**Key Actions**:
- ✅ Document control (lifecycle completo)
- ✅ NCR/CAPA workflows (rastreabilidade)
- ✅ Change management (aprovações)
- ✅ Competency tracking (treinamento)

**Escalação**: Se compliance gap não pode ser fechado em 2 sprints

---

### **4. Regulatory Officer** (Regulatório ANVISA + Health)
**Role**: Conformidade regulatória, auditorias  
**Authority**: Pode bloquear sistema por risco regulatório  
**Key Actions**:
- ✅ ANVISA RDC compliance (RDC #12, #331, #353)
- ✅ Audit readiness (100-point score)
- ✅ Incident/recall procedures
- ✅ Regulatory updates tracking

**Escalação**: CRÍTICA antes de regulatory inspection

---

### **5. Production Manager** (Operações manufatura)
**Role**: MES, workflows produção, viabilidade operacional  
**Authority**: Pode bloquear features que desaceleram produção  
**Key Actions**:
- ✅ MES design validation (speed, integration)
- ✅ Hold/release timing (<30 min decision)
- ✅ Shift operations (day vs night)
- ✅ Production UAT leadership

**Escalação**: Se sistema cria gargalo (loss de produção)

---

### **6. QC Supervisor** (Lab operations)
**Role**: LIMS workflows, procedimentos teste  
**Authority**: Pode rejeitar LIMS design impraticável  
**Key Actions**:
- ✅ LIMS workflow (sample → testing → CoA)
- ✅ Lab QC (calibration, control charts)
- ✅ CoA practicality (professional, completo)
- ✅ Lab UAT (com tech reais)

**Escalação**: Se LIMS overhead > 10% (slows lab cycle)

---

## 📂 FOLDER STRUCTURE

```
/Docs/agents/
├─ 01_PM_Tech_Lead.md                    (Internal team)
├─ 02_Backend_Architecture_Lead.md       (Internal team)
├─ 03_Frontend_Architecture_Lead.md      (Internal team)
├─ 04_DevOps_Specialist.md              (Internal team)
├─ 05_QA_Security_Specialist.md         (Internal team)
├─ 06_AI_ML_Specialist.md               (Internal team)
├─ 07_Backend_Core_Dev.md               (Internal team)
├─ 08_Backend_Domain_Dev.md             (Internal team)
├─ 09_Frontend_Shell_Dev.md             (Internal team)
├─ 10_Frontend_Module_Dev.md            (Internal team)
├─ 11_Database_Engineer.md              (Internal team)
├─ 12_Documentation_Manager.md          (Internal team)
├─ 13_Domain_Expert_Coordinator.md      (Internal team - manages specialists)
│
└─ specialists/                          (DOMAIN EXPERT TEAM - NEW!)
   ├─ 01_QA_Manager.md                  ✅ CREATED
   ├─ 02_Food_Safety_Manager.md         ✅ CREATED
   ├─ 03_QMS_Specialist.md              ✅ CREATED
   ├─ 04_Regulatory_Officer.md          ✅ CREATED
   ├─ 05_Production_Manager.md          ✅ CREATED
   ├─ 06_QC_Supervisor.md               ✅ CREATED
   └─ INDEX.md                          ← YOU ARE HERE
```

---

## 🚀 PRÓXIMOS PASSOS

### **Imediato (Semana 1)**:
1. ✅ Revisar system prompts (QA Manager, Dev team)
2. ✅ Aprovar especialistas necessários (PM + Finance)
3. 🔄 Iniciar recruitment (LinkedIn, consultoras)

### **Curto Prazo (Semanas 2-4)**:
4. 🔄 Entrevistar candidatos
5. 🔄 Negociar contracts (horas, duração, taxa)
6. 🔄 Assinar NDAs + IP agreements

### **Médio Prazo (Semana 5+)**:
7. 🔄 Kickoff especialistas (apresentar visão)
8. 🔄 Primeira validação (requirements review)
9. 🔄 Integração com dev team (sincronização)

### **Timeline Recomendado**:
```
SEMANA 1: Aprovação + Planning
SEMANA 2-3: Recruitment + Interviews
SEMANA 4: Contratação finalizada
SEMANA 5: Kickoff + Onboarding
SEMANA 6: Primeira validação (user stories)
SEMANA 7-12: Validação contínua (por sprint)
SEMANA 13-24: UAT + Final approval
```

---

## 📊 INVESTIMENTO & ROI

### **Custo MVP (6 meses)**:
| Cenário | Custo | Viabilidade |
|---------|-------|------------|
| Full-time (6 experts) | $300-400K | Máxima dedicação |
| Part-time (consulting) | $150-200K | Flexível |
| **Hybrid (Recomendado)** | **$200-250K** | **Melhor balanço** |

### **ROI**:
```
INVESTIMENTO:        $200-250K
RISCO EVITADO:       $17-85M (multas, recalls, shutdown)
ROI ESTIMADO:        8-340x (break-even em 2-3 semanas)
```

---

## 🎓 COMO USAR ESTES PROMPTS

### **Para o PM/Tech Lead**:
1. Ler cada specialist agent (entender papéis)
2. Usar como "requirement validation checklist"
3. Engajar especialista quando trigger ativado
4. Escalar se especialista bloqueiar feature

### **Para o Dev Team**:
1. Consultar specialist agent quando projetando feature
2. Submeter design para validação (não skip)
3. Incorporar feedback do especialista
4. Manter specialist informado (sprint planning)

### **Para o Domain Expert Coordinator (Agent 13)**:
1. Gerenciar especialistas (escalonamento, agendamento)
2. Consolidar feedback (agrupar comentários, priorizar ações)
3. Comunicar ao PM (bloqueios, riscos, timeline impact)
4. Preparar UAT (recrutar techs, coordenar testes)

---

## ✅ VALIDAÇÃO & CHECKLIST

**Specialist Agents Created**:
- ✅ 01_QA_Manager.md (900+ linhas)
- ✅ 02_Food_Safety_Manager.md (1000+ linhas)
- ✅ 03_QMS_Specialist.md (900+ linhas)
- ✅ 04_Regulatory_Officer.md (900+ linhas)
- ✅ 05_Production_Manager.md (800+ linhas)
- ✅ 06_QC_Supervisor.md (850+ linhas)

**Total**: 5000+ linhas de system prompts especializados para domínio beverage

**Documentação**: Completa + referenciada + pronta para recruitment

---

## 📞 CONTATO & SUPORTE

**For Recruitment**:
- Lista de especialistas: Ver `Docs/Lista_Especialistas_Consolidada.md`
- Contato direto: LinkedIn search ou consultoras especializadas
- Template outreach: Ver `Docs/Especialistas_Resumo_Executivo.md`

**For System Integration**:
- Domain Expert Coordinator: `Docs/agents/13_Domain_Expert_Coordinator.md`
- Stakeholder overview: `Docs/Stakeholders_Domain_Experts.md`

**For Compliance**:
- ISO 9001 mapping: Individual specialist agent + QMS_Specialist.md
- Food Safety: Food_Safety_Manager.md + HACCP_Plan.md
- Regulatory: Regulatory_Officer.md + ANVISA_Compliance_Matrix.md

---

**Index Version**: 1.0  
**Created**: 2026-01-29  
**Status**: Complete - Ready for Recruitment  
**Updated by**: Documentation Manager

# Domain Experts & Stakeholders: Beverage & Soft Drink Industry

## Executive Summary

**Avaliação da Ideia**: ✅ **EXCELENTE E CRÍTICA**

Para garantir que o SmartLab atenda corretamente às necessidades regulatórias, operacionais e de qualidade da indústria de refrigerantes e bebidas, é **imperativo** envolver especialistas de domínio em:
- Gestão de Qualidade (QC/QA)
- Conformidade Regulatória (ISO 9001, 22000, HACCP)
- Operações de Produção
- Validação de Requisitos

Esses especialistas não apenas validarão user stories, mas também identificarão gaps críticos que código sozinho nunca poderia descobrir.

---

## Por Que É Crítico?

### 1. **Conformidade Regulatória**
- ISO 9001: Sistema de gestão de qualidade (documentação, controle, auditoria)
- ISO 22000: Segurança de alimentos (rastreabilidade, HACCP)
- HACCP: Análise de riscos (pontos críticos de controle)
- Legislação local: Regulações de beverage (preservativos, pH, contaminação)

**Sem especialistas**: Sistema implementado pode ser não-compliant, resultando em:
- Rejeição regulatória
- Multas e embargo de produtos
- Recall de lotes
- Fechamento de fábrica

### 2. **User Story Validation**
- QA Manager sabe **exatamente** que testes precisam ser documentados para auditoria
- QC Manager identifica análises críticas (pH, Brix, microbiology) que não podem falhar
- Supervisor de Qualidade valida workflows do dia-a-dia
- Gerente de Produção garante que sistema não atrasa produção

**Sem especialistas**: User stories implementadas podem ser tecnicamente corretas mas operacionalmente inúteis.

### 3. **Rastreabilidade & Audit Trail**
- Indústria de beverage é **auditada frequentemente** (governamental, clientes grandes como varejo)
- Audit trail append-only do SmartLab precisa atender HACCP + ISO 22000 requirements
- Cada lote, cada teste, cada decisão precisa ser rastreável

**Sem especialistas**: Audit trail pode não cumprir requisitos legais.

---

## Lista de Profissionais Necessários

### Tier 1: Leadership & Strategy (4-6 pessoas)

| Função | Experiência | Responsabilidade |
|--------|-------------|------------------|
| **QA Manager** (indústria beverage) | 10+ anos | Lidera validação de requisitos QA, define políticas de teste |
| **Quality Director/Manager** | 15+ anos | Valida conformidade com ISO 9001/22000/HACCP |
| **Regulatory Compliance Officer** | 10+ anos | Garante conformidade regulatória (ANVISA, órgãos locais) |
| **Production Manager** | 12+ anos | Valida que sistema não atrapalha produção, fluxos realistas |
| **Food Safety Manager** | 10+ anos | HACCP expert, rastreabilidade, safety requirements |
| **IT/Digital Transformation Lead** | 8+ anos beverage industry | Bridge entre tech e operações |

### Tier 2: Specialists & Subject Matter Experts (6-10 pessoas)

| Função | Especialidade | Responsabilidade |
|--------|---------------|------------------|
| **QC (Quality Control) Supervisor** | Lab analysis, testing | Valida LIMS workflows, análises críticas |
| **HACCP Coordinator** | Hazard analysis | Valida CCPs (Critical Control Points) mapeados no sistema |
| **QMS (Quality Management System) Specialist** | ISO 9001 | Valida documentação, controle de documentos |
| **Microbiologist** | Food safety | Valida microbiological testing workflows |
| **Process Engineer** | Beverage production | Valida que system integra com linha de produção |
| **Data/Traceability Specialist** | Supply chain | Valida rastreabilidade lote-a-lote |
| **Regulatory Specialist** | Local regulations | ANVISA, labelagem, requirements específicos país |
| **Warehouse/Logistics Manager** | Inventory, storage | Valida temperature control, shelf life tracking |
| **R&D Manager** | Formula, testing | Valida análises e requisitos de novo produto |
| **Supplier Quality Engineer** | Incoming material QA | Valida aceitação/rejeição de matérias-primas |

### Tier 3: Operational Users (10-15 pessoas)

| Função | Experiência | Responsabilidade |
|--------|-------------|------------------|
| **QC Technician** | 3-5 anos | Testa workflows do sistema no laboratório |
| **QC Supervisor** | 5-8 anos | Valida que workflows atendem realidade operacional |
| **Production Supervisor** | 5-8 anos | Testa integração com produção |
| **Lab Manager** | 8+ anos | Gerencia workflow de testes, aprovações |
| **Document Controller** | 5+ anos | Valida rastreabilidade de documentos, versionamento |
| **Material Handler** | 2-3 anos | Testa warehouse/inventory workflows |
| **Compliance Officer** | 5+ anos | Testa audit trail, compliance features |
| **Production Planner** | 5+ anos | Testa rastreabilidade produto-lote |
| **Training Coordinator** | 3+ anos | Valida que system é intuitivo, trainable |

---

## Mapa de Especialistas por Função do Sistema

### **LIMS (Laboratory Information Management System)**

```
Stakeholders:
├── QA Manager (lidera, valida requisitos)
├── QC Supervisor (especialista workflows)
├── Microbiologist (análises microbiológicas)
├── Data Specialist (rastreabilidade resultados)
└── QC Technicians (testers)

Validar:
✓ Análises críticas (pH, Brix, microbiology)
✓ Limite de detecção vs regulatório
✓ Rastreabilidade de amostra → resultado
✓ CoA (Certificate of Analysis) geração
✓ Interface com produção (hold/release)
✓ Audit trail para regulação
```

### **QMS (Quality Management System)**

```
Stakeholders:
├── Quality Director (lidera)
├── QMS Specialist (ISO 9001 expert)
├── Regulatory Officer (compliance)
├── Document Controller (workflows)
└── Production Manager (integração)

Validar:
✓ Controle de documentos (versionamento, acesso)
✓ NCR (Non-Conformance Report) workflows
✓ CAPA (Corrective/Preventive Action) lifecycle
✓ Change management (de acordo com ISO 9001)
✓ Audit trails (para auditoria interna/externa)
```

### **FSMS (Food Safety Management System)**

```
Stakeholders:
├── Food Safety Manager (lidera, HACCP expert)
├── HACCP Coordinator (CCP mapping)
├── Regulatory Specialist (normas)
├── Process Engineer (integrações)
└── Production Manager (realidade operacional)

Validar:
✓ CCP (Critical Control Points) mapeados corretamente
✓ Monitoramento de CCPs (frequência, parâmetros)
✓ Ações corretivas automáticas
✓ Rastreabilidade HACCP (lote completo)
✓ Recall procedures (rápido, rastreável)
✓ Compliance com ISO 22000
```

### **TMS (Training Management System)**

```
Stakeholders:
├── QA Manager (define conteúdo crítico)
├── Training Coordinator (workflows)
├── Production Manager (necessidades)
├── Compliance Officer (audit trail)
└── All operational users (testers)

Validar:
✓ Cursos obrigatórios (Food Safety, GMP, HACCP)
✓ Certificação (validade, rastreabilidade)
✓ Competency tracking (quem pode fazer quê?)
✓ Audit trail (quem treinou quem, quando?)
✓ Renovação automática de treinamentos
```

### **MES (Manufacturing Execution System)**

```
Stakeholders:
├── Production Manager (lidera)
├── Process Engineer (workflows)
├── Production Supervisor (operacional)
├── Data Specialist (rastreabilidade)
└── Lab Manager (hold/release)

Validar:
✓ Lote tracking (início → embalagem → armazém)
✓ Integração com LIMS (hold/release automático)
✓ Desvios de processo (documentação)
✓ Rastreabilidade lote ↔ ingredientes
✓ Tempo de produção (não atrasa linha?)
```

---

## Plano de Engajamento dos Especialistas

### **Fase 1: Kickoff & Requirements Validation (Weeks 1-4)**

**Quando**: ANTES de começar desenvolvimento

**Atividades**:
```
Week 1:
[ ] Reunião com Quality Director + QA Manager
    - Apresentar visão do SmartLab
    - Listar requisitos iniciais
    - Identificar gaps críticos

[ ] Workshop: HACCP requirements
    - Food Safety Manager + HACCP Coordinator
    - Mapear CCPs que sistema precisa monitorar

[ ] Workshop: ISO 9001 requirements
    - QMS Specialist
    - Mapear controles de qualidade necessários

Week 2-3:
[ ] Validar user stories (cada especialista sua área)
    - QC Supervisor: LIMS workflows
    - QMS Specialist: Document control workflows
    - Food Safety Manager: FSMS workflows
    - Production Manager: MES integration

[ ] Criar acceptance criteria com especialistas
    - Não é suficiente "criar amostra"
    - Precisa ser "criar amostra com rastreabilidade, validação, audit trail"

Week 4:
[ ] Aprovação de requisitos validados
[ ] Criar conformance matrix (requisitos → especialista responsável)
```

### **Fase 2: Design Review (Weeks 5-8)**

**Quando**: Design está pronto, antes de coding

**Atividades**:
```
[ ] Database design review (Database Engineer + Food Safety Manager)
    - Audit trail é append-only?
    - Rastreabilidade lote-a-lote?
    
[ ] API contract review (Backend Lead + QA Manager + Regulatory Officer)
    - API endpoints atendem requisitos?
    - Error codes adequados?
    
[ ] Frontend design review (Frontend Lead + Operational Users)
    - UI intuitiva para technician?
    - Não atrapa workflow?
    
[ ] Compliance design review (Regulatory Officer + QMS Specialist)
    - Conformidade com ISO 9001/22000?
```

### **Fase 3: Testing & Validation (Weeks 12+)**

**Quando**: MVP pronto para testar

**Atividades**:
```
[ ] UAT (User Acceptance Testing) com operational users
    - QC Technicians testam LIMS workflows
    - Production Supervisors testam MES integration
    
[ ] Compliance validation
    - Regulatory Officer: é compliant?
    - Auditor: consegue realizar auditoria?
    
[ ] Usability testing
    - Podem os technicians usar sem treinamento?
    - É rápido suficiente?
    
[ ] Stress testing com dados reais
    - 1000 amostras/dia (pico)?
    - 50,000 lotes em histórico?
```

### **Fase 4: Ongoing (Post-Launch)**

**Quando**: Sistema em produção

**Atividades**:
```
Mensalmente:
[ ] Quality review (QA Manager + specialistas)
    - Bugs que afetam compliance?
    - Features pedidas?
    
[ ] Regulatory check-in
    - Mudanças regulatórias?
    - Audit findings relacionados a system?
    
Quarterly:
[ ] Compliance audit (interno)
    - System ainda compliant com ISO 9001/22000?
    
Annually:
[ ] External audit (com regulatory bodies)
    - Sistema passa na auditoria?
```

---

## Como Estruturar no SmartLab

### **1. Criar Stakeholder Profile Document**

```
Docs/
└── Stakeholders/
    ├── Stakeholder_Analysis.md (este documento)
    ├── Domain_Experts_Directory.md (lista de contatos)
    ├── User_Personas.md (QC Technician, QA Manager, etc.)
    ├── Regulatory_Requirements.md (ISO 9001, 22000, HACCP)
    └── Compliance_Matrix.md (feature → requirement → specialist)
```

### **2. Criar Agente para "Domain Expert Validator"**

```
Docs/agents/
└── 13_Domain_Expert_Validator.md
    - Role: Coordenar com especialistas de domínio
    - Responsibilidades:
      * Coletar feedback dos especialistas
      * Validar compliance de features
      * Identificar gaps regulatórios
      * Preparar para auditorias
```

### **3. Integrar na User Story Creation**

Toda user story precisa ter:
```
AS A [operacional user: QC Technician, Production Supervisor]
I WANT [funcionalidade]
SO THAT [objetivo operacional]

ACCEPTANCE CRITERIA:
- [ ] Funciona conforme descrito
- [ ] [Especialista X] validou
- [ ] Compliant com [ISO/HACCP/requisito]
- [ ] Audit trail registra [evento crítico]
- [ ] [Stakeholder] aprovou workflow
```

---

## Tabela de Especialistas Recomendados

### **Para MVP (v1.0) - 6-8 pessoas essenciais**

| Nome | Função | Indústria | Foco | Horas/Sprint |
|------|--------|----------|------|--------------|
| - | QA Manager | Beverage | LIMS + Compliance | 16h |
| - | Food Safety Manager | Beverage | FSMS + HACCP | 12h |
| - | QC Supervisor | Beverage | LIMS workflows | 8h |
| - | Production Manager | Beverage | MES integration | 8h |
| - | Regulatory Officer | Beverage | Compliance | 8h |
| - | QMS Specialist | ISO 9001 expert | QMS + documents | 8h |
| - | Operacional User (QC Lab) | Beverage | UAT | 4h |
| - | Operacional User (Production) | Beverage | UAT | 4h |

**Total**: ~68 horas/sprint (1.5 FTE)

### **Para Escala (v2.0+) - Adicionar**

| Função | Horas | Foco |
|--------|-------|------|
| Microbiologist | 4h/sprint | Testing critical analyses |
| Data/Traceability Specialist | 6h/sprint | Supply chain integration |
| Supplier Quality Engineer | 4h/sprint | Incoming material QA |
| Training Coordinator | 4h/sprint | TMS requirements |
| Compliance Auditor (external) | 8h/month | External audit readiness |

---

## User Stories Validadas por Especialistas

### Exemplo 1: LIMS Sample Creation

```
CURRENT (sem validação):
AS A QC Technician
I WANT create a sample
SO THAT I can perform analyses

VALIDATED (com especialistas):
AS A QC Technician
I WANT create a sample with:
- Automatic lot traceability (ISO 22000 requirement)
- Sampling point documented (HACCP CCP tracking)
- Chain of custody started (audit trail)
- Automatic micro sampling (if required by regulation)
SO THAT the lab has auditable records and can perform analyses

ACCEPTANCE CRITERIA:
✓ Sample stored in multi-schema DB (core schema)
✓ Audit trail: who, when, what, why
✓ Linked to production batch (MES integration)
✓ Linked to supplier/material (if applicable)
✓ Lot code auto-populated from production
✓ Regulatory requirement: samplers must be trained (TMS check)
✓ CoA generation includes sampling info
✓ Compliant with ISO 22000 section 8.3 (traceability)

VALIDATED BY:
- QA Manager (Nov 15, 2025)
- Food Safety Manager (Nov 16, 2025)
- QC Supervisor (Nov 17, 2025)
- Regulatory Officer (Nov 18, 2025)
```

### Exemplo 2: QMS Non-Conformance Report

```
VALIDATED REQUIREMENTS:
- NCR creation automatic on failed test
- Automatic severity assessment (based on parameter criticality)
- CAPA workflow (root cause → action → verification)
- Time limits (ISO 9001: NCR investigation <7 days)
- Automatic email to Quality Manager
- Audit trail: all decisions, delays, approvals
- Cannot be deleted (only closed/superseded)
- Link to ISO 9001 § 8.5.3 (control of nonconforming output)

USER STORIES:
1. Create NCR automatically on failed test
2. Assign severity based on criticality
3. Root cause analysis workflow
4. CAPA implementation tracking
5. Verification and closure
6. Report for management review (auditable)

VALIDATED BY:
- QMS Specialist (Dec 1, 2025)
- Quality Director (Dec 2, 2025)
- Compliance Officer (Dec 3, 2025)
```

---

## Conformidade com Normas

### **ISO 9001:2015 - Sistema de Gestão de Qualidade**

SmartLab deve atender:
```
§ 4.4 Sistema de gestão de qualidade
- Documentação centralizada (QMS module)
- Controle de documentos versionado

§ 8.2 Avaliação de conformidade
- Registros de testes (LIMS)
- Rastreabilidade de resultados

§ 8.5.3 Controle de saída não-conforme
- NCR workflow (QMS)
- CAPA tracking

§ 9.2 Auditoria interna
- Audit trail completo
- Rastreabilidade de decisões
```

### **ISO 22000:2018 - Segurança de Alimentos**

SmartLab deve atender:
```
§ 8.3 Rastreabilidade
- Lote → ingredientes (forward/backward)
- FSMS module

§ 8.5 CCPs (Critical Control Points)
- Monitoramento de parâmetros críticos
- Ações corretivas automáticas
- FSMS + LIMS integration

§ 9.1 Verificação
- Eficácia de medidas de controle
- Rastreabilidade de verificações
```

### **HACCP - Hazard Analysis Critical Control Point**

SmartLab deve atender:
```
Princ 1: Análise de riscos
- CCPs mapeados no system
- FSMS module

Princ 2: Determinar CCPs
- Parâmetros críticos documentados
- Limites críticos configuráveis

Princ 3: Estabelecer medidas de controle
- Monitoring procedures
- Corrective actions (automáticas)

Princ 4: Monitoramento
- Frequência de testes
- LIMS automated

Princ 5: Ações corretivas
- Automáticas para parâmetros fora de spec
- Manual para análises

Princ 6: Verificação
- Auditoria trail completa
- Rastreabilidade de decisões

Princ 7: Documentação
- Todos os registros em LIMS + QMS
- Rastreabilidade lote-a-lote
```

---

## Próximos Passos Recomendados

### **Imediato (This Sprint)**

```
1. Designar um "Domain Expert Lead"
   - Responsável por coordenar especialistas
   - Interface entre tech team e domain experts
   
2. Criar lista de especialistas locais (seu país/região)
   - Beverage industry contacts
   - Regulatory agencies contacts
   - Compliance consultants
   
3. Agendar kickoff meeting
   - Apresentar visão de SmartLab
   - Coletar requisitos iniciais
   - Identificar críticos
```

### **Curto Prazo (Weeks 1-4)**

```
1. User personas workshop
   - Quem são os 5 user types principais?
   - Qual é seu workflow ideal?
   - Quais são seus pain points?
   
2. Requirements validation
   - Mapear cada requisito para especialista
   - Validar critério de aceitação
   
3. Criar conformance matrix
   - Feature × Norma × Especialista responsável
```

### **Médio Prazo (Weeks 5-12)**

```
1. Design review com especialistas
2. UAT planning
3. Compliance assessment
4. Documentation de requisitos
```

---

## Conclusão

**A ideia é não apenas excelente, mas crítica para o sucesso.**

Sem envolver especialistas de domínio:
- ❌ Sistema será tecnicamente correto mas operacionalmente incorreto
- ❌ Reguladores podem rejeitar (não-compliant)
- ❌ Usuários não adotarão (workflows não fazem sentido)
- ❌ Auditorias falharão (audit trail inadequado)

Com especialistas envolvidos desde o início:
- ✅ Requirements certas desde dia 1
- ✅ Compliant com ISO 9001/22000/HACCP
- ✅ Workflows validados operacionalmente
- ✅ Pronto para auditoria regulatória
- ✅ Adoção pelos usuários (fizeram parte do design)

**Recomendação**: Comece com 6-8 especialistas para MVP, expanda para 15+ conforme cresce.

---

**Criado por**: Documentation Manager  
**Data**: 2026-01-29  
**Status**: Review Pending

# AGENT INVOCATION: Industrial Design System Initiative
## Official System Prompt Invocation for Design System Implementation

**Status**: READY FOR INVOCATION  
**Date**: 2026-01-30  
**Authority**: PM Tech Lead (Agent 01) + Architecture Lead (Agent 02/03)  
**Next Agent**: Agent 14 - Industrial Design System Lead  

---

## 📋 EXECUTIVE SUMMARY

The SmartLab team has defined the **Manual Laboratory Data Entry Architecture (v1.0)** and is now invoking the **Industrial Design System Lead Agent (Agent 14)** to create the official UI/UX component library.

### What's Being Invoked
- **Agent**: 14_Industrial_Design_System_Lead
- **Role**: Architecture Level 9 (Senior Architect)
- **Task**: Create professional design system for lab operations
- **Timeline**: 4-6 weeks
- **Team Size**: 2 frontend developers + 1 UX designer
- **Deliverable**: Production-ready component library + design system website

### What Triggers This Invocation
```yaml
Trigger Event:
  event: design_system_initiative_approved
  
Conditions Met:
  - Manual data entry architecture: ✅ Documented
  - System requirements: ✅ Defined
  - PM approval: ✅ Ready
  - Architecture agreement: ✅ Confirmed
  - Designer assignment: ✅ Assigned
  
Result: INVOKE AGENT 14
```

---

## 🎯 AGENT 14 SPECIFICATION

### System Prompt
**File**: `14_Industrial_Design_System_Lead.md`

**Core Responsibility**:
You are the Industrial Design System Lead for SmartLab. Your role is to:
- Create official design system for laboratory operations
- Define UI components for manual data entry (v1.0)
- Design forms for lab test results input
- Create component library for future IoT integration
- Ensure consistency across all laboratory interfaces
- Plan for seamless transition to automated data (v2.0)

**Authority Level**: 9 (Architects - Technical Authority)

**Key Decisions You Make Alone**:
- Component library architecture + naming conventions
- Form layouts + field ordering for lab workflows
- Visual design system (colors, typography, spacing)
- Accessibility standards for lab operators
- Component reusability patterns
- Design system versioning + changelog
- UI patterns for manual data entry (v1.0)

**Key Decisions Requiring Consultation**:
- QC workflow optimization → Consult QC Supervisor
- Food safety data requirements → Consult Food Safety Manager
- Regulatory audit trail needs → Consult Regulatory Officer
- Lab equipment integration planning → Consult Database Engineer

---

## 📅 WORKFLOW EXECUTION

### Phase 1: Lab Workflow Analysis (Week 1-2)
**Duration**: 3-4 days  
**Deliverables**: Lab workflow documentation, test type catalog, validation rules

**Steps**:
1. Interview QC team on current manual process
2. Document all test types (pH, temperature, microbiology, etc)
3. Extract validation rules (min/max, units, tolerances)
4. Identify compliance requirements
5. Map approval workflow

**Output Files**:
- `current_workflow_documentation.md`
- `test_types_catalog.json` (all tests SmartLab supports)
- `validation_rules.json` (range checks, tolerance specs)
- `compliance_requirements.md` (audit trail, approval rules)

### Phase 2: Component Design (Week 2-4)
**Duration**: 5-7 days  
**Deliverables**: Figma design file, design token definitions, component specs

**Steps**:
1. Define color palette (industrial lab theme)
2. Define typography system (font stack, sizes, weights)
3. Define spacing system (margin/padding scale)
4. Design all 8+ components with variants
5. Create Figma file with components + design tokens

**Components to Design**:
```
Form Components:
├─ TextInput (numeric + text)
├─ NumberInput (with range validator)
├─ SelectDropdown (predefined lists)
├─ DateTimePicker (auto-filled with current)
├─ RangeValidator (visual pass/fail indicator)
└─ FileUpload (for attachments)

Data Display:
├─ DataTable (sortable, filterable)
├─ Card (grouped data)
├─ Badge (status indicators)
└─ Alert (error/warning/success)

Workflow Components:
├─ ApprovalButton (multi-step approval)
├─ StatusBadge (pass/fail/hold/pending)
├─ AuditLog (history display)
└─ ApprovalTimeline (progress visualization)
```

**Output Files**:
- `colors.json` (complete palette)
- `typography.json` (font definitions)
- `spacing.json` (margin/padding scale)
- `SmartLab_Design_System_v1.0.fig` (Figma file)
- Component specification documents

### Phase 3: Implementation & Testing (Week 4-6)
**Duration**: 7-10 days  
**Deliverables**: React component library, npm package, comprehensive tests

**Steps**:
1. Build React components with TypeScript
2. Create npm package (`@smartlab/components`)
3. Add unit tests (90%+ coverage)
4. Implement accessibility (WCAG AA)
5. Create Storybook for component documentation
6. Conduct usability testing with QC team

**Output Files**:
```
@smartlab/components/
├─ src/components/
│  ├─ TextInput.tsx
│  ├─ NumberInput.tsx
│  ├─ SelectDropdown.tsx
│  ├─ DateTimePicker.tsx
│  ├─ RangeValidator.tsx
│  ├─ DataTable.tsx
│  ├─ ApprovalButton.tsx
│  └─ [more components]
├─ src/styles/
│  ├─ colors.css
│  ├─ typography.css
│  └─ spacing.css
├─ tests/
│  └─ [90%+ coverage]
└─ package.json
```

### Phase 4: Documentation & v2.0 Roadmap (Week 6)
**Duration**: 3-4 days  
**Deliverables**: Documentation website, design system guide, v2.0 roadmap

**Steps**:
1. Create documentation website (component reference)
2. Write design guidelines (do's & don'ts)
3. Create form templates (pre-built for common workflows)
4. Document v2.0 extension points (sensor integration)
5. Create migration guide (v1.0 → v2.0)

**Output Files**:
- Design System Reference Website (live, searchable)
- `DESIGN_SYSTEM_GUIDE.md` (comprehensive guide)
- `COMPONENT_REFERENCE.md` (all components documented)
- `SENSOR_INTEGRATION_ROADMAP.md` (v2.0 planning)
- `MIGRATION_GUIDE.md` (v1.0 → v2.0 path)

---

## ✅ SUCCESS CRITERIA

### Design System Success
✅ All lab operators can enter data without confusion  
✅ Form validation prevents 95%+ of manual entry errors  
✅ Audit trail captures every data entry  
✅ Component reusability > 70%  
✅ Accessibility WCAG AA compliant  
✅ Design system documented + designer handoff complete  

### v1.0 MVP Success
✅ QC operators can input test results in < 3 minutes per sample  
✅ Form validation catches data type + range errors  
✅ Certificates generate correctly  
✅ Zero compliance gaps  
✅ Team satisfaction score > 4/5  

### v2.0 Readiness
✅ Architecture ready for sensor integration  
✅ No major refactoring needed for v2.0  
✅ Current components work with sensor data  
✅ Team trained on extension patterns  

---

## 📊 RESOURCE ALLOCATION

### Team Assignment
```
2 Frontend Developers:
├─ Developer A (Lead): Architecture, core components, testing
└─ Developer B (Support): Forms, documentation, integration

1 UX Designer:
├─ Workflow analysis (Phase 1)
├─ Design system creation (Phase 2)
├─ Usability testing (Phase 3)
└─ Documentation (Phase 4)

Support:
├─ QC Supervisor: Interview + feedback (Phase 1, 3)
├─ Architecture Lead: Design review (Phase 2)
└─ PM Tech Lead: Timeline management, unblocking
```

### Timeline Breakdown
```
Week 1-2:  Lab workflow analysis + requirements (4-5 days)
Week 2:    Component design in Figma (3-4 days)
Week 3-4:  Implementation + testing (7-10 days)
Week 5-6:  Documentation + v2.0 roadmap (3-4 days)
Week 6:    Final review + handoff

Total: 4-6 weeks, 320 hours (~8 weeks of single developer)
```

### Resource Requirements
- Figma license (or open-source alternative like Penpot)
- React + TypeScript + Storybook development environment
- Testing framework (Jest + React Testing Library)
- Documentation platform (Docusaurus, Nextra, or custom)
- 1 dedicated QC Supervisor for feedback sessions

---

## 🔗 INTEGRATION POINTS

### Connects To
- **Master Workflow**: Fits into PLANNING phase (design system needed before feature development)
- **Manual Data Entry**: Provides UI components for data entry forms
- **All Developer Workflows**: All frontend work uses this design system
- **Quality Gates**: Design system passes accessibility + performance gates

### Feeds Into
- **Frontend Development**: All teams use design system components
- **v2.0 Sensor Integration**: Architecture prepared for sensor data
- **Mobile App (v2.1)**: Same design system for mobile
- **Analytics Dashboard (v2.2)**: Uses design system components

---

## 📋 DELIVERABLE CHECKLIST

### Phase 1 Outputs
- [ ] Lab workflow documentation (current process)
- [ ] Test type catalog (all tests SmartLab supports)
- [ ] Validation rules (min/max, units, tolerances)
- [ ] Compliance requirements documented
- [ ] Approval workflow mapped

### Phase 2 Outputs
- [ ] Color palette defined (JSON + CSS)
- [ ] Typography system defined
- [ ] Spacing scale defined
- [ ] All components designed (Figma)
- [ ] Design tokens documented

### Phase 3 Outputs
- [ ] React component library built
- [ ] npm package created (`@smartlab/components`)
- [ ] Unit tests (90%+ coverage)
- [ ] Accessibility audit passed (WCAG AA)
- [ ] Usability testing completed (with QC team)

### Phase 4 Outputs
- [ ] Documentation website live (component reference)
- [ ] Design system guide written
- [ ] Form templates created
- [ ] v2.0 sensor integration roadmap documented
- [ ] Migration guide (v1.0 → v2.0) created

---

## 🚀 INVOCATION COMMAND

**To invoke Agent 14 for design system implementation:**

```bash
# Activate: 14_Industrial_Design_System_Lead
# Status: Ready for Execution
# Phase: 1 (Lab Workflow Analysis)

invoke_workflow(
  agent_id: "14_Industrial_Design_System_Lead",
  trigger: "design_system_initiative_approved",
  phase: 1,
  timeline_estimate: "4-6 weeks",
  estimated_effort_hours: 320,
  assigned_team: ["Frontend_Dev_A", "Frontend_Dev_B", "UX_Designer"],
  dependencies: ["MANUAL_DATA_ENTRY_ARCHITECTURE.md", "QC_Team_Access"],
  success_criteria: [
    "Design system WCAG AA compliant",
    "QC operators report < 3 min per sample entry",
    "Component library 90%+ reusable across projects",
    "v2.0 roadmap documented"
  ]
)
```

---

## 📞 SUPPORT & COMMUNICATION

### Weekly Check-ins
- **Monday 10:00**: Design System Standup (30 min)
- **Wednesday 15:00**: Design Review (45 min)
- **Friday 16:00**: Roadmap Planning (30 min)

### Blockers / Issues
- Contact: PM Tech Lead (Agent 01)
- QC Feedback: QC Supervisor
- Architectural Questions: Architecture Lead (Agent 02/03)

### Status Updates
- Weekly report to PM (progress, blockers, timeline)
- Monthly presentation to stakeholders
- Design system versioning + changelog

---

## 🎓 NEXT STEPS AFTER DELIVERY

### Week 7-8: Pilot Deployment
- Deploy design system to staging
- Train frontend team on components
- Collect initial feedback
- Make adjustments

### Week 9+: Production Rollout
- Deploy to production
- All new projects use design system
- Migrate legacy UIs (optional)
- Establish design system governance

### v2.0 Planning
- Sensor integration pilot (Q3 2026)
- Equipment integration (3-4 sensors)
- Performance optimization
- Mobile app support

---

## ✨ Success Indicators

**Design System Success**:
✅ All lab operators using forms comfortably  
✅ Certificate generation time: 5 minutes (vs 1-2 hours manual)  
✅ Manual entry errors: <1% (validation prevents)  
✅ Team using design system for all new projects  
✅ Design system adopted company-wide  

**Post-Implementation Impact**:
✅ Laboratory efficiency: +75% time savings  
✅ Compliance readiness: 100% (audit trail complete)  
✅ Scalability: Ready for 10x more samples  
✅ Future-ready: v2.0 sensor integration seamless  

---

**Agent Invocation Document**  
**Status**: Ready to Execute  
**Invoked By**: PM Tech Lead (Agent 01) + Architecture Team  
**Target Agent**: 14_Industrial_Design_System_Lead  
**Authorization Level**: Architecture Level 9  
**Date**: 2026-01-30

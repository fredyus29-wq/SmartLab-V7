# WORKFLOW: Industrial Design System Lead
## Design System Creation & Laboratory UI Architecture

**Agent ID**: 14_Industrial_Design_System_Lead  
**Trigger**: PM approves Design System project initiation  
**Phase**: PLANNING + EXECUTION (cross-phase)  
**Authority**: Architecture Level 9  
**Estimated Effort**: 4-6 weeks (design + implementation + testing)  

---

## ENTRY TRIGGER

```yaml
Trigger: design_system_initiative_approved
When: 
  - status: "Feature request received"
  - feature_name: "Industrial Design System"
  - pm_approval: true
  - qc_supervisor_input: received
  - architecture_lead_sign_off: approved
Then:
  - Invoke: 14_Industrial_Design_System_Lead
  - Phase: 1 (Component Library Design)
  - Timeline_Estimate: "4-6 weeks"
```

---

## PHASE 1: Component Library Architecture (Week 1-2)

### Step 1.1: Lab Workflow Analysis
**Duration**: 3-4 days  
**Participants**: Design System Lead + QC Supervisor + QA Manager

**Activities**:
```yaml
Tasks:
  - Interview QC team: How do you currently enter lab data?
    Output: Current_workflow_documentation.md
  
  - Map test result types:
    └─ pH, temperature, pressure, density, microbiology, etc
    Output: test_types_catalog.json
  
  - Document validation rules:
    └─ Min/max values, units, tolerance ranges
    Output: validation_rules.json
  
  - Identify compliance requirements:
    └─ Audit trails, approvals, data integrity
    Output: compliance_requirements.md
  
  - Map approvers + authorities:
    └─ QC Supervisor approval, Regulatory Officer sign-off
    Output: approval_workflow.yaml
```

**Output Format** (YAML):
```yaml
lab_workflows:
  test_result_entry:
    current_process: "Manual paper form → Lab notebook → Excel file"
    data_types:
      - numeric (temperature, pH, density)
      - qualitative (pass/fail, appearance)
      - microbiology (plate count, identification)
    typical_entry_time: "5-10 minutes per sample"
    error_rate: "~3-5% of entries (transcription errors)"
    bottleneck: "Certificate generation takes 1-2 hours (manual)"
    future_state: "Automated data entry with sensor integration (v2.0)"
  
  approval_workflow:
    step_1: "QC Supervisor reviews results"
    step_2: "Food Safety Manager approves (if applicable)"
    step_3: "Regulatory Officer signs off (if regulatory sample)"
    step_4: "Certificate generated + archived"
```

### Step 1.2: Component Inventory & Design Tokens
**Duration**: 5-7 days  
**Participants**: Design System Lead + Frontend Architecture Lead

**Activities**:
```yaml
Tasks:
  - Define color palette:
    └─ Industrial lab colors (precision blues, safety greens/reds)
    Output: colors.json
  
  - Define typography:
    └─ Font stack, sizes, weights
    Output: typography.json
  
  - Define spacing system:
    └─ Margin/padding scale for consistency
    Output: spacing.json
  
  - Create component inventory:
    └─ Which components needed for lab forms
    Output: component_matrix.csv
  
  - Prioritize components:
    └─ Must-have (test result form), Nice-to-have (charts)
    Output: component_priority_list.md
```

**Output Format** (JSON):
```json
{
  "design_tokens": {
    "colors": {
      "lab_blue": "#2563EB",
      "success_green": "#10B981",
      "alert_red": "#EF4444",
      "warning_yellow": "#F59E0B"
    },
    "typography": {
      "heading_xl": "32px, weight 600",
      "body": "14px, weight 400"
    },
    "spacing": [2, 4, 8, 16, 24, 32]
  },
  "components_needed": [
    "TextInput",
    "NumberInput",
    "SelectDropdown",
    "DateTimePicker",
    "RangeValidator",
    "DataTable",
    "ApprovalButton",
    "StatusBadge"
  ]
}
```

---

## PHASE 2: Component Design & Documentation (Week 3-4)

### Step 2.1: Create Component Specifications
**Duration**: 5-7 days  
**Participants**: Design System Lead + UX Designer

**Activities**:
```yaml
Tasks:
  - Design TextInput component:
    ├─ Default, hover, focus, error, disabled states
    ├─ Accessibility (ARIA labels, keyboard)
    └─ Spec: TextInput.md with examples
    Output: TextInput_spec.md + Figma component
  
  - Design NumberInput component:
    ├─ With validation (min/max, decimal places)
    ├─ Show actual vs specification value
    └─ Pass/Fail indicator
    Output: NumberInput_spec.md
  
  - Design SelectDropdown:
    ├─ Single and multi-select variants
    └─ Common lab test types pre-populated
    Output: SelectDropdown_spec.md
  
  - Design DateTimePicker:
    ├─ Default to current time
    ├─ Allow manual override
    └─ Show timezone
    Output: DateTimePicker_spec.md
  
  - Design DataTable:
    ├─ Display test results
    ├─ Sortable columns
    └─ Inline editing (with permission)
    Output: DataTable_spec.md
  
  - Design ApprovalWorkflow:
    ├─ Multi-step approval UI
    ├─ Show current approver + status
    └─ Audit trail visible
    Output: ApprovalWorkflow_spec.md
  
  - Create Figma design file:
    ├─ All components with variants
    ├─ Dark mode support
    └─ Mobile responsive (future)
    Output: SmartLab_Design_System.fig
```

**Output Format** (Markdown Component Spec):
```markdown
# TextInput Component Spec

## Purpose
Allow numeric and text entry for lab data values.

## Props
- `label` (string, required): Field label
- `type` ("text" | "number" | "email"): Input type
- `placeholder` (string): Placeholder text
- `value` (string): Current value
- `onChange` (function): On value change
- `error` (string): Error message to display
- `disabled` (boolean): Disable input

## States
- Default: Empty input
- Filled: Value entered
- Error: Invalid value (red border)
- Disabled: Cannot interact
- Focus: Keyboard focused

## Accessibility
- ARIA label: Linked to label prop
- Keyboard: Tab to focus, type to enter, Esc to clear
- Screen reader: Reads label + error message

## Example
```jsx
<TextInput
  label="pH Value"
  type="number"
  placeholder="6.5 - 7.5"
  error={error}
  onChange={handleChange}
/>
```

## Future IoT Readiness
- Can display live sensor value via `sensor_value` prop
- Supports auto-update from device via WebSocket (v2.0)
```

### Step 2.2: Create Figma Design System
**Duration**: 3-5 days  
**Participants**: Design System Lead + UX Designer

**Activities**:
```yaml
Deliverables:
  - Figma file with:
    ├─ Color palette page
    ├─ Typography page
    ├─ Spacing guidelines page
    ├─ Component library (all components)
    ├─ Form templates (test result, sample, quality param)
    ├─ Common patterns (approval flow, data table)
    └─ Mobile variants (future consideration)
  
  - Design documentation:
    ├─ Do's and don'ts for each component
    ├─ Accessibility guidelines
    ├─ v2.0 extension points marked
    └─ Component usage examples from lab
```

**Output**: 
- `SmartLab_Design_System_v1.0.fig` (Figma file)
- `DESIGN_SYSTEM_GUIDE.md` (documentation)

---

## PHASE 3: Component Implementation & Testing (Week 4-6)

### Step 3.1: Build Component Library (React)
**Duration**: 7-10 days  
**Participants**: Frontend Lead + 2 Frontend Developers

**Activities**:
```yaml
Tasks:
  - Create component library (npm package):
    ├─ @smartlab/components
    └─ Exports: TextInput, NumberInput, SelectDropdown, etc
  
  - Implement each component:
    ├─ TypeScript types
    ├─ Props validation
    ├─ Unit tests (90%+ coverage)
    └─ Storybook stories for each variant
  
  - Add accessibility:
    ├─ ARIA labels
    ├─ Keyboard navigation
    ├─ Screen reader testing
    └─ WCAG AA compliance check
  
  - Create CSS design tokens:
    ├─ colors.css
    ├─ typography.css
    ├─ spacing.css
    └─ Exportable to Figma plugin
  
  - Build documentation website:
    ├─ Component reference
    ├─ Usage examples
    ├─ Interactive examples (live playground)
    └─ Accessibility guidelines
```

**Output**:
```
@smartlab/components/
├─ src/
│  ├─ components/
│  │  ├─ TextInput.tsx
│  │  ├─ NumberInput.tsx
│  │  ├─ SelectDropdown.tsx
│  │  ├─ DateTimePicker.tsx
│  │  ├─ DataTable.tsx
│  │  └─ [more components]
│  ├─ styles/
│  │  ├─ colors.css
│  │  ├─ typography.css
│  │  └─ spacing.css
│  └─ types/
│     └─ components.ts
├─ tests/
│  ├─ TextInput.test.tsx
│  ├─ NumberInput.test.tsx
│  └─ [component tests]
├─ stories/
│  ├─ TextInput.stories.tsx
│  └─ [component stories]
└─ package.json
```

### Step 3.2: Build Lab Forms Package
**Duration**: 5-7 days  
**Participants**: Frontend Lead + 1 Frontend Developer

**Activities**:
```yaml
Tasks:
  - Create TestResultForm:
    ├─ Pre-built form for lab data entry
    ├─ All validation rules included
    ├─ Auto-calculates pass/fail status
    └─ Integrates with database
    Output: TestResultForm.tsx
  
  - Create SampleManagementForm:
    ├─ Search samples by ID/batch
    ├─ Link tests to samples
    ├─ Track sample lifecycle
    Output: SampleManagementForm.tsx
  
  - Create QualityParametersForm:
    ├─ Enter quality specs vs actual
    ├─ Auto-calculate pass/fail
    ├─ Show specification table
    Output: QualityParametersForm.tsx
  
  - Create CertificatePreview:
    ├─ Professional certificate layout
    ├─ Shows all test results
    ├─ Signature blocks for approvers
    └─ PDF export ready
    Output: CertificatePreview.tsx
  
  - Create validation rule library:
    ├─ Range checks (min/max)
    ├─ Data type checks
    ├─ Custom validation functions
    └─ Error message localization
    Output: validators.ts
```

**Output**:
```
@smartlab/forms/
├─ src/
│  ├─ forms/
│  │  ├─ TestResultForm.tsx
│  │  ├─ SampleManagementForm.tsx
│  │  ├─ QualityParametersForm.tsx
│  │  └─ CertificatePreview.tsx
│  ├─ validators/
│  │  ├─ rangeValidator.ts
│  │  ├─ typeValidator.ts
│  │  └─ customValidators.ts
│  └─ types/
│     └─ forms.ts
└─ package.json
```

### Step 3.3: Testing & QC Validation
**Duration**: 5-7 days  
**Participants**: Design System Lead + QC Supervisor + 1 QA Tester

**Activities**:
```yaml
Tasks:
  - Unit test coverage:
    ├─ Each component: 90%+ coverage
    ├─ All validation rules tested
    ├─ Error states tested
    Output: coverage_report.html
  
  - Integration testing:
    ├─ Forms work with real database
    ├─ Validation rules enforce correctly
    ├─ Approval workflow functions
    Output: integration_test_results.md
  
  - Usability testing (with QC team):
    ├─ 5 QC operators use forms
    ├─ Measure entry time (target < 3 min/sample)
    ├─ Collect feedback on UX
    ├─ Fix any usability issues
    Output: usability_test_report.md
  
  - Accessibility testing:
    ├─ WCAG AA compliance check
    ├─ Screen reader testing
    ├─ Keyboard navigation verification
    Output: accessibility_audit_report.md
  
  - Performance testing:
    ├─ Form load time < 2 seconds
    ├─ No N+1 database queries
    ├─ Data table handles 1000+ rows
    Output: performance_report.md
```

**Output Format** (YAML):
```yaml
testing_results:
  unit_tests:
    total_tests: 245
    passed: 245
    coverage: 94%
  integration_tests:
    total_tests: 48
    passed: 48
    coverage: "All form workflows"
  usability_tests:
    participants: 5
    average_entry_time: "2.5 minutes"
    satisfaction_score: 4.6/5
    issues_found: 2 (both minor)
  accessibility_tests:
    wcag_aa_compliance: "PASS"
    color_contrast: "PASS"
    keyboard_navigation: "PASS"
  performance:
    form_load_time: "1.2 seconds"
    data_table_performance: "Good (1000+ rows)"
    lighthouse_score: 96
```

---

## PHASE 4: Documentation & v2.0 Roadmap (Week 6)

### Step 4.1: Create Design System Documentation Website
**Duration**: 3-4 days  
**Participants**: Design System Lead + Technical Writer

**Activities**:
```yaml
Deliverables:
  - Component reference:
    ├─ Interactive component explorer
    ├─ Props documentation
    ├─ Usage examples
    ├─ Copy-paste code snippets
    └─ Live playground
  
  - Design guidelines:
    ├─ Color usage guidelines
    ├─ Typography usage
    ├─ Spacing patterns
    ├─ Do's and don'ts
    └─ Accessibility guidelines
  
  - Form templates:
    ├─ Pre-built forms for common lab workflows
    ├─ Usage examples
    ├─ Customization guide
    └─ Database schema for each form
  
  - API reference:
    ├─ Component props documented
    ├─ Validation rule API
    ├─ Form submission API
    └─ Event handlers documented
```

**Output**:
```
design-system-website/
├─ pages/
│  ├─ components/
│  │  ├─ text-input.mdx
│  │  ├─ number-input.mdx
│  │  └─ [component docs]
│  ├─ guidelines/
│  │  ├─ color.mdx
│  │  ├─ typography.mdx
│  │  └─ accessibility.mdx
│  └─ forms/
│     ├─ test-result-form.mdx
│     └─ [form docs]
└─ public/
   ├─ examples/
   └─ screenshots/
```

### Step 4.2: Create v2.0 Roadmap Documentation
**Duration**: 2-3 days  
**Participants**: Design System Lead + Architecture Lead + Database Engineer

**Activities**:
```yaml
Deliverables:
  - Sensor integration points:
    ├─ Which components need sensor data?
    ├─ How data flows from sensor → component
    ├─ Extension points documented
    └─ Example implementations
    Output: SENSOR_INTEGRATION_ROADMAP.md
  
  - IoT architecture design:
    ├─ Message queue (MQTT/NATS)
    ├─ Data processing pipeline
    ├─ Real-time WebSocket updates
    ├─ Fallback to manual entry
    Output: IOT_ARCHITECTURE.md
  
  - Migration guide:
    ├─ v1.0 (manual) → v2.0 (sensor) path
    ├─ Data compatibility
    ├─ Rollback strategy
    └─ Timeline + effort
    Output: MIGRATION_GUIDE.md
  
  - Component extension guide:
    ├─ How to add sensor support to existing component
    ├─ Props changes (backward compatible)
    ├─ Event handling for sensor updates
    └─ Error handling for sensor failures
    Output: EXTENSION_GUIDE.md
```

**Output Format** (Markdown):
```markdown
# v2.0 Roadmap: IoT/Sensor Integration

## Timeline
- Q3 2026: Pilot with 2 sensors (pH, temperature)
- Q4 2026: Full deployment across all lab equipment
- 2027: Mobile app with sensor data collection

## Architecture Overview

### Current (v1.0)
```
Manual Entry → Validation → Database → Certificate
```

### Future (v2.0)
```
Sensor → IoT Gateway → Message Queue → Processing → Database ← Manual Override
                                                        ↓
                                                   Certificate
```

## Component Changes (Backward Compatible)

### Example: TextInput → DataInput

```jsx
// v1.0: Manual entry only
<DataInput
  label="Temperature"
  type="number"
/>

// v2.0: Can accept sensor data
<DataInput
  label="Temperature"
  type="number"
  sensor_source={{
    device_id: "TEMP-01",
    update_interval: 1000, // ms
    fallback_to_manual: true
  }}
/>
```

## Integration Points
- Equipment APIs (Microlab AMS, Modular Cube)
- MQTT broker for sensor networks
- WebSocket for real-time updates
- Database for historical data
- Mobile app for field collection
```

---

## COMPLETION CRITERIA

### Design System Completion:
- ✅ All 8+ core components specified + designed
- ✅ Component library built + tested (90%+ coverage)
- ✅ Design tokens (colors, typography, spacing) documented
- ✅ Design System website live + searchable
- ✅ WCAG AA accessibility compliance verified
- ✅ Team trained on design system usage

### Lab Forms Completion:
- ✅ TestResultForm functional + tested
- ✅ SampleManagement form functional + tested
- ✅ QualityParameters form functional + tested
- ✅ CertificatePreview generates correct documents
- ✅ All validation rules working (80% + accuracy)
- ✅ Audit trail captures all data entries

### v1.0 MVP Success:
- ✅ QC operators entering data < 3 min per sample
- ✅ Form validation prevents manual entry errors
- ✅ Certificate generation < 5 minutes (vs 1-2 hours manual)
- ✅ Zero compliance gaps in audit trail
- ✅ Team satisfaction score > 4/5

### v2.0 Readiness:
- ✅ Architecture documented for sensor integration
- ✅ Extension points marked in components
- ✅ Backward compatibility maintained
- ✅ Team trained on extension patterns
- ✅ Migration path documented

---

## PHASE TRANSITIONS

```
PHASE 1: Requirements → Step 1.1 Complete
  └─ Trigger: Workflow analysis finished
  └─ Next: PHASE 2 Design

PHASE 2: Design → Steps 2.1 + 2.2 Complete
  └─ Trigger: Figma designs approved by QC Supervisor
  └─ Next: PHASE 3 Implementation

PHASE 3: Implementation → Steps 3.1 + 3.2 + 3.3 Complete
  └─ Trigger: All tests passing, usability score > 4/5
  └─ Next: PHASE 4 Documentation

PHASE 4: Documentation → Steps 4.1 + 4.2 Complete
  └─ Status: DELIVERY_READY
  └─ Output: Production design system + v2.0 roadmap
```

---

## ESCALATION RULES

| Situation | Action | To Whom |
|-----------|--------|---------|
| QC feedback conflicts | PM decides priority | PM Tech Lead |
| Accessibility issues | Fix before shipping | Accessibility specialist |
| Performance issues | Optimize or scope cut | Architecture Lead |
| Unforeseen complexity | Replan timeline | PM + Architecture Lead |
| Blocker for integration | Escalate immediately | PM Tech Lead |
| Scope creep request | Evaluate for v2.0 | PM Tech Lead |

---

## OUTPUT FILES

```
smartlab-design-system-v1.0/
├─ components/
│  ├─ TextInput.tsx
│  ├─ NumberInput.tsx
│  ├─ SelectDropdown.tsx
│  ├─ DateTimePicker.tsx
│  ├─ RangeValidator.tsx
│  ├─ DataTable.tsx
│  └─ [more components]
├─ forms/
│  ├─ TestResultForm.tsx
│  ├─ SampleManagementForm.tsx
│  ├─ QualityParametersForm.tsx
│  └─ CertificatePreview.tsx
├─ styles/
│  ├─ colors.css
│  ├─ typography.css
│  └─ spacing.css
├─ tests/
│  └─ [component tests - 90%+ coverage]
├─ documentation/
│  ├─ DESIGN_SYSTEM_GUIDE.md
│  ├─ COMPONENT_REFERENCE.md
│  ├─ SENSOR_INTEGRATION_ROADMAP.md
│  ├─ IOT_ARCHITECTURE.md
│  └─ MIGRATION_GUIDE.md
├─ figma/
│  └─ SmartLab_Design_System_v1.0.fig
└─ README.md (Getting started)
```

---

**Workflow ID**: 14_Industrial_Design_System_Lead  
**Version**: 1.0 - MVP (Manual Data Entry)  
**Status**: READY FOR EXECUTION  
**Next Phase**: v2.0 Roadmap (IoT/Sensor Integration)  
**Last Updated**: 2026-01-30

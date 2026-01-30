# AGENT INDIVIDUAL WORKFLOW: QMS Specialist (Specialist 03)

**Agente**: QMS Specialist - Domain Expert (Quality Management System)  
**Autoridade**: Level 9 (Specialist)  
**Fases**: Compliance Assessment → System Integration → Approval

---

## Workflow Entrada

**Trigger que ativa**:
- Status = FEATURE_READY_FOR_SPECIALIST_REVIEW
- Event = QMS_REVIEW_REQUIRED
- Feature involves: Process change, documentation, quality metrics, process control

**Input**:
```
{
  "task_id": "PH-002",
  "task_title": "pH Data Collection API",
  "feature_description": "Automated pH monitoring system",
  "affected_processes": ["Fermentation monitoring", "Quality control", "Data archiving"],
  "process_documentation": "[links to process docs]",
  "testing_complete": true,
  "submission_date": "2026-02-04"
}
```

---

## FASE 1: COMPLIANCE ASSESSMENT (1.5 days)

### Step 1.1 - Review Quality System Impact

**QMS Specialist faz**:
```
1. Identify Affected Processes
   - Which processes change?
   - Which procedures need updates?
   - Which work instructions affected?
   - Which forms affected?

2. Assess Documentation Gaps
   - Is the new process documented?
   - Are work instructions complete?
   - Are responsibilities clear?
   - Is training material needed?

3. Check Standards Compliance
   - ISO 9001 requirements met?
   - ISO 22000 food safety system met?
   - Industry best practices followed?
   - Regulatory requirements covered?

4. Identify Risks to Quality
   - What could go wrong?
   - What's the impact if fails?
   - How is each risk mitigated?
   - What's the control method?
```

**Example - pH Monitoring System**:
```
AFFECTED PROCESSES:
1. Fermentation Monitoring
   - Old: Manual pH testing every 4 hours
   - New: Automated pH sensor, continuous monitoring
   - Change type: Process automation

2. Quality Control
   - Old: Batch supervisors test and record manually
   - New: System automatically records, supervisor reviews
   - Change type: Responsibility shift

3. Data Archiving
   - Old: Manual entry into LIMS
   - New: Automatic transfer to LIMS
   - Change type: Data flow automation

RISKS IDENTIFIED:
1. Sensor failure → Undetected pH deviation
   Control: Sensor failure alert within 30 seconds
   
2. Data integrity → Invalid readings stored
   Control: Validation rules + audit trail
   
3. Training gap → Supervisors don't understand new system
   Control: Mandatory training + test quiz
   
4. Procedure mismatch → QMS procedures say one thing, system does another
   Control: Update all procedures + work instructions before go-live
```

**Output**:
```yaml
quality_system_assessment:
  processes_affected:
    - "Fermentation Monitoring"
    - "Quality Control Data Entry"
    - "Data Archiving & LIMS Integration"
    - "Batch Release"
  
  required_changes:
    documentation:
      - "Update 'Fermentation Monitoring' procedure"
      - "Update 'QC Data Entry' work instruction"
      - "Update 'LIMS Data Transfer' procedure"
      - "Create 'Sensor Maintenance' procedure"
      - "Create 'Sensor Failure Response' work instruction"
    
    training:
      - "Operator training: New system operation"
      - "Supervisor training: Alert response"
      - "QC training: Data validation in system"
      - "Management training: New metrics/monitoring"
    
    forms:
      - "Update Fermentation Batch Record (now auto-generated)"
      - "Create Sensor Calibration Record"
      - "Create System Failure Log"
  
  identified_risks:
    - risk_id: "RISK-01"
      description: "Sensor fails without detection"
      severity: "HIGH"
      control: "Failure alert within 30 seconds"
      verification: "Tested in QA"
    
    - risk_id: "RISK-02"
      description: "Invalid pH values recorded"
      severity: "MEDIUM"
      control: "Validation rules (2.5-6.0 pH range)"
      verification: "Unit tests verify validation"
    
    - risk_id: "RISK-03"
      description: "Supervisors untrained on new system"
      severity: "HIGH"
      control: "Mandatory training with test"
      verification: "Training tracking system"
  
  compliance_status:
    iso_9001: "COMPLIANT"
    iso_22000: "COMPLIANT"
    regulatory: "COMPLIANT"
    internal_standards: "COMPLIANT"
```

**Auto-Trigger Next Step**: Documentation review

---

### Step 1.2 - Review Documentation Quality

**QMS Specialist faz**:
```
1. Check Process Documentation
   - Is the process documented clearly?
   - Are steps in logical order?
   - Are inputs/outputs defined?
   - Are success criteria clear?

2. Check Procedure Writing
   - Does it use standardized format?
   - Are responsibilities clear?
   - Are timelines defined?
   - Are quality criteria specified?

3. Check Work Instructions
   - Are they step-by-step?
   - Would a new employee understand?
   - Are resources linked?
   - Are safety/quality warnings clear?

4. Check Data Integrity Procedures
   - How is data validated?
   - Who can modify data?
   - How are changes tracked?
   - Who has access?
```

**Documentation Checklist**:
```
pH Monitoring System Documentation

PROCEDURE: "Automated pH Monitoring and Recording"
☑ Purpose and scope
☑ Process steps (5-8 steps)
☑ Responsibilities (supervisor, system, QC)
☑ Quality criteria (sensor accuracy, alert response time)
☑ Non-conformance handling (what if pH out of spec)
☑ References to work instructions
☑ Defined roles

WORK INSTRUCTION: "Responding to pH Sensor Alert"
☑ When does this apply (pH alert triggered)
☑ Step-by-step instructions
☑ Who does what (supervisor responsibility)
☑ Required actions (stop fermentation, troubleshoot)
☑ Time requirements (respond within 15 minutes)
☑ Communication (notify who)
☑ Documentation (what to record)

FORM: "pH Sensor Failure Report"
☑ Clear title
☑ When to use (when sensor fails)
☑ Required fields (date, time, which sensor, impact)
☑ Sign-off fields (who discovered, who responded)
☑ Approval fields (supervisor approval)

TRAINING MATERIAL: "pH Monitoring System Operation"
☑ Overview of system
☑ When you see pH alert, what to do
☑ How to manually override
☑ How to troubleshoot common issues
☑ Safety considerations
☑ Quiz to verify understanding
```

**Output**:
```yaml
documentation_review:
  procedures_reviewed:
    - "Automated pH Monitoring": "ADEQUATE"
    - "QC Data Entry": "NEEDS_UPDATE"
    - "Sensor Maintenance": "MISSING_MUST_CREATE"
    - "Sensor Failure Response": "NEEDS_UPDATE"
  
  documentation_assessment:
    completeness: "85% (needs updates)"
    clarity: "GOOD"
    compliance: "COMPLIANT"
    
    missing_items:
      - "Sensor calibration procedure"
      - "Monthly sensor validation procedure"
      - "Sensor failure escalation procedure"
    
    items_to_update:
      - "QC Data Entry (now uses system instead of manual)"
      - "Batch Release (now includes system validation)"
  
  training_materials_status:
    operator_training: "READY"
    supervisor_training: "READY"
    qc_training: "NEEDS_UPDATE"
    management_briefing: "NEEDS_CREATE"
  
  documentation_deadline: "2026-02-12"
```

**Auto-Trigger Next Step**: System integration check

---

## FASE 2: SYSTEM INTEGRATION REVIEW (1 day)

### Step 2.1 - Verify System Integration with QMS

**QMS Specialist faz**:
```
1. Data Integrity
   - Are pH readings properly stored?
   - Can data be edited? (should be read-only + audit trail)
   - Is audit trail complete?
   - Can data be retrieved for audits?

2. Traceability
   - Can we trace any reading back to:
     * Which sensor/batch
     * When it was recorded
     * Who reviewed it
     * What action taken

3. QMS Integration Points
   - LIMS integration: Works correctly?
   - Batch record: Automatically populated?
   - Alerts: Trigger correctly?
   - Reporting: Metrics available?

4. Quality Metrics
   - Can we measure system effectiveness?
   - Are metrics defined in SOP?
   - Can we report on metrics?
   - Can we trend over time?
```

**Output**:
```yaml
system_integration_assessment:
  data_integrity:
    readings_stored: "VERIFIED"
    read_only_after_30min: "VERIFIED"
    audit_trail_complete: "VERIFIED"
    retrieval_for_audits: "VERIFIED"
  
  traceability:
    batch_traceability: "VERIFIED"
    sensor_traceability: "VERIFIED"
    timestamp_traceability: "VERIFIED"
    user_traceability: "VERIFIED"
    action_traceability: "VERIFIED"
  
  qms_integration:
    lims_integration: "WORKING"
    batch_record_population: "AUTOMATIC"
    alert_trigger: "WORKING"
    reporting_capability: "AVAILABLE"
  
  quality_metrics_available:
    - "pH readings per batch"
    - "Alert frequency"
    - "Alert response time"
    - "Out-of-spec frequency"
    - "System uptime"
    - "Sensor accuracy"
  
  integration_status: "READY_FOR_DEPLOYMENT"
```

**Auto-Trigger Next Step**: Final approval

---

### Step 2.2 - Change Management Review

**QMS Specialist faz**:
```
1. Change Classification
   - Is this a minor or major change?
   - Does it affect product quality?
   - Does it affect regulatory compliance?
   - Does it require re-validation?

2. Impact Analysis
   - What's the business impact?
   - What's the quality impact?
   - What's the regulatory impact?
   - What's the operational impact?

3. Change Control
   - Is change control documented?
   - Are approvals obtained?
   - Is rollback plan defined?
   - Is training plan defined?

4. Post-Implementation Review
   - When will we review effectiveness?
   - What metrics will we track?
   - Who will review?
   - What's the review schedule?
```

**Output**:
```yaml
change_management:
  change_classification: "MAJOR"
  reason: "Affects process control, data integrity, regulatory compliance"
  
  impact_assessment:
    business_impact: "POSITIVE - better control, less manual work"
    quality_impact: "POSITIVE - real-time monitoring vs 4-hour lag"
    regulatory_impact: "POSITIVE - better compliance with FSMA"
    operational_impact: "NEUTRAL - different tasks, same time investment"
  
  change_control:
    documented: true
    change_request: "CHG-2026-001"
    approvals_required:
      - "QMS Specialist": "PENDING (this step)"
      - "Food Safety Manager": "APPROVED"
      - "Production Manager": "APPROVED"
      - "Quality Assurance": "APPROVED"
  
  training_plan:
    target_audience: "All batch supervisors + QC staff"
    training_date: "2026-02-10"
    completion_required: "YES - before go-live"
  
  post_implementation_review:
    scheduled_date: "2026-02-19"
    reviewer: "QMS Specialist"
    metrics_to_evaluate:
      - "Data quality"
      - "System uptime"
      - "Alert accuracy"
      - "Staff comfort level"
      - "Documentation adequacy"
      - "Process compliance"
```

**Auto-Trigger Next Step**: Final QMS approval

---

## FASE 3: APPROVAL & VALIDATION (0.5 days)

### Step 3.1 - Final QMS Approval

**QMS Specialist faz**:
```
1. QMS Readiness Check
   - All required documents updated? ✓
   - All risks identified and mitigated? ✓
   - System integration verified? ✓
   - Training completed? ✓
   - Change control approved? ✓

2. Compliance Verification
   - ISO 9001 compliant? ✓
   - ISO 22000 compliant? ✓
   - Regulatory requirements met? ✓
   - Company policy followed? ✓

3. Approval Decision
   - Is the quality system ready? YES
   - Can we deploy with confidence? YES
   - Are risks managed? YES
   
   DECISION: ✓ APPROVED FOR DEPLOYMENT
```

**Output**:
```yaml
qms_approval:
  decision: "APPROVED_FOR_DEPLOYMENT"
  approved_by: "03_QMS_Specialist"
  approval_date: "2026-02-05"
  
  approval_basis:
    quality_system: "READY"
    documentation: "COMPLETE"
    integration: "VERIFIED"
    training: "SCHEDULED"
    risks: "MITIGATED"
    change_control: "APPROVED"
  
  conditions:
    - "Documentation completed by 2026-02-12"
    - "Training completed by 2026-02-16"
    - "Post-implementation review scheduled for 2026-02-19"
  
  deployment_authorized: true
  deployment_date: "2026-02-16"
  
  signature: "QMS Specialist - Digital Signature"
```

**Auto-Trigger Next Step**: PM coordination for deployment

---

### Step 3.2 - Post-Implementation Monitoring

**QMS Specialist faz** (ongoing):
```
First week:
- Monitor data quality metrics
- Check documentation usage
- Verify training effectiveness
- Address any process questions

Post-implementation review (2 weeks):
- Evaluate system effectiveness
- Identify any process improvements
- Update procedures based on reality
- Train-the-trainer for sustainability
- Document lessons learned
- Update CAPA if issues found

Quarterly review:
- System effectiveness metrics
- Staff feedback
- Any non-conformances
- Continuous improvement opportunities
```

**Output**:
```yaml
post_implementation_monitoring:
  first_week_status:
    data_quality: "EXCELLENT"
    system_usage: "GOOD"
    staff_adoption: "POSITIVE"
    documentation_adequacy: "ADEQUATE"
  
  post_implementation_review: "2026-02-19"
  quarterly_reviews: "Scheduled"
  
  success_criteria:
    - "System uptime > 99%"
    - "Data quality > 98%"
    - "Alert accuracy > 95%"
    - "Staff satisfaction > 4/5"
    - "Zero quality incidents related to monitoring"
```

---

## Decision Outcomes

### ✓ APPROVED
```
QMS is ready for deployment
- Documentation updated
- Integration verified
- Training planned
- Risks managed
- Change control approved

→ Approved for deployment on [date]
```

### ⚠️ APPROVED WITH CONDITIONS
```
QMS mostly ready, conditions must be met:
- Condition 1: [specific action] by [date]
- Condition 2: [specific action] by [date]
- Condition 3: [specific action] by [date]

→ Approved ONLY IF conditions met by [date]
```

### ✗ REJECTED
```
QMS not ready for deployment:
- Issue 1: [specific QMS gap]
- Issue 2: [specific QMS gap]
- Issue 3: [specific QMS gap]

→ REJECTED - Must address before resubmission
```

---

## Escalation Scenarios

```
Scenario 1: Documentation Gap Found During Testing
- Finding: Procedure says "X" but system does "Y"
→ Stop deployment
→ Update documentation or system
→ Re-review

Scenario 2: Regulatory Question
- Question: "Is this change reportable to FDA?"
→ Consult Regulatory Officer (04)
→ Implement based on guidance

Scenario 3: Training Gap
- Issue: Supervisors not ready by deadline
→ Delay deployment
→ Extend training period
→ Add additional trainers if needed

Scenario 4: Integration Issue Found
- Issue: LIMS integration not working correctly
→ Block deployment
→ Fix integration
→ Re-test
→ Re-review
```

---

## QMS Specialist Success Metrics

QMS Specialist is successful if:
- ✓ Quality system is documented and compliant
- ✓ All procedures updated before deployment
- ✓ Data integrity verified and maintained
- ✓ System integrated with LIMS correctly
- ✓ Traceability complete and auditable
- ✓ Training completed and effective
- ✓ Post-implementation shows > 99% uptime
- ✓ Data quality > 98%
- ✓ Staff adoption positive

---

## Key Documents Created/Updated

```
CREATED:
- "Sensor Maintenance Procedure"
- "Sensor Failure Response Work Instruction"
- "System Failure Log Form"
- "pH Monitoring System Operation Training Material"
- "pH Monitoring System Management Briefing"

UPDATED:
- "Fermentation Monitoring Procedure"
- "QC Data Entry Work Instruction"
- "LIMS Data Transfer Procedure"
- "Batch Release Procedure"
```

---

**QMS Specialist Workflow Version**: 1.0  
**Last Updated**: 2026-01-30

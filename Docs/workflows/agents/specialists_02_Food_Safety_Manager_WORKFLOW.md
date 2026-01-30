# AGENT INDIVIDUAL WORKFLOW: Food Safety Manager (Specialist 02)

**Agente**: Food Safety Manager - Domain Expert  
**Autoridade**: Level 9 (Specialist)  
**Fases**: Requirement Analysis → Implementation Review → Approval/Rejection

---

## Workflow Entrada

**Trigger que ativa**:
- Status = FEATURE_READY_FOR_SPECIALIST_REVIEW
- Event = FOOD_SAFETY_REVIEW_REQUIRED

**Input**:
```
{
  "task_id": "PH-002",
  "task_title": "pH Data Collection API",
  "feature_description": "pH monitoring in production",
  "affected_area": "Production / HACCP",
  "architecture_spec": "[link]",
  "implementation_complete": true,
  "tests_complete": true,
  "submission_date": "2026-02-04"
}
```

---

## FASE 1: REQUIREMENTS ANALYSIS (1 day)

### Step 1.1 - Understand Feature

**Food Safety Manager faz**:
```
1. Read Feature Details
   - What does it do?
   - How does it affect production?
   - What pH values are involved?
   - What are the safety implications?

2. Identify HACCP Connection
   - Is pH a CCP (Critical Control Point)?
   - What are critical limits? (e.g., 3.0-4.0)
   - What's the safety risk if out of bounds?
   - What corrective actions needed?

3. Check Existing Standards
   - Do we have documented HACCP for pH?
   - What are the regulations? (FDA, FSMA, ISO 22000)
   - What do we currently do manually?
   - How does automation change things?
```

**Output**:
```yaml
food_safety_analysis:
  feature: "pH Data Collection API"
  
  haccp_assessment:
    is_ccp: true
    ccp_name: "Fermentation pH Control"
    hazard: "Improper fermentation due to pH outside range"
    severity: "High - could impact product safety"
    
    critical_limits:
      minimum_ph: 3.0
      maximum_ph: 4.0
      range_name: "Acidic environment for safety"
    
    monitoring_frequency: "Continuous"
    corrective_actions:
      - "If pH < 3.0: Stop fermentation, add buffer"
      - "If pH > 4.0: Stop fermentation, add acid"
  
  current_process:
    method: "Manual pH testing with strips + LIMS entry"
    frequency: "Every 4 hours"
    operator: "Batch supervisor"
    time_per_test: "15 minutes"
    lag_from_actual: "0-4 hours"
  
  proposed_process:
    method: "Automated sensor with real-time data"
    frequency: "Continuous (every 30 seconds)"
    operator: "Automatic with alerts"
    time_lag: "Near real-time (< 1 minute)"
    
  safety_improvement: "Significantly better - real-time vs 4-hour lag"
```

**Auto-Trigger Next Step**: Review implementation

---

### Step 1.2 - Define Approval Criteria

**Food Safety Manager faz**:
```
1. Set Requirements for Approval
   - What MUST be true for me to approve?
   - What's the minimum acceptable?
   - What are the conditions?

2. Identify Risk Mitigations
   - What could go wrong with automation?
   - How is each risk mitigated?
   - What validations must be in place?

3. Create Acceptance Checklist
   - CCP monitoring: yes/no ✓
   - Alert system: yes/no ✓
   - Audit trail: yes/no ✓
   - Manual override: yes/no ✓
   - Backup systems: yes/no ✓
```

**Output**:
```yaml
approval_criteria:
  ccp_monitoring: "pH sensor continuously monitors and logs"
  alert_system: "Alert triggered if pH out of bounds within 30 seconds"
  
  requirements:
    - sensor_accuracy: "±0.1 pH units"
    - alert_threshold_time: "< 30 seconds from violation"
    - data_storage: "Minimum 12 months"
    - audit_trail: "All readings logged with timestamp + user"
    - manual_override: "Supervisor can manually stop fermentation"
    - backup_method: "Manual testing still available"
    - employee_training: "All supervisors trained on system"
    - runaway_monitoring: "Rate-of-change alerts (pH changing > 0.5/min)"
  
  approval_decision_matrix:
    all_requirements_met: "✓ APPROVED"
    minor_issues_fixable: "✓ APPROVED_WITH_CONDITIONS"
    major_gaps: "✗ REJECTED_MUST_FIX"
    regulatory_violation: "✗ REJECTED_HARD_STOP"
```

**Auto-Trigger Next Step**: Review implementation

---

## FASE 2: IMPLEMENTATION REVIEW (2 days)

### Step 2.1 - Review Code & Tests

**Food Safety Manager faz**:
```
1. Review pH Sensor Data Collection
   - Is sensor data being collected correctly?
   - Is accuracy adequate? (±0.1 pH as spec)
   - Is there validation? (reject impossible values)
   - Is there logging? (audit trail of readings)

2. Review Alert System
   - Does it alert when pH out of bounds?
   - Does it alert within 30 seconds?
   - Is alert clear to operator?
   - Can operator manually intervene?

3. Review Data Storage
   - Are readings stored long-term? (12 months+)
   - Can readings be audited?
   - Can readings be modified? (should be read-only)
   - Is there backup? (redundancy)

4. Review Tests
   - Does test verify pH monitoring?
   - Does test verify alert system?
   - Does test verify edge cases?
   - Does test use realistic pH scenarios?
```

**Test Scenarios to Verify**:
```
1. Normal Operation
   - pH 3.5 (in bounds) → No alert ✓
   - Continuous monitoring ✓

2. pH Out of Bounds
   - pH 2.8 (low) → Alert triggered within 30s ✓
   - pH 4.2 (high) → Alert triggered within 30s ✓

3. Rapid pH Change
   - pH changes by 0.5 in 5 minutes → Runaway alert ✓
   - pH changes by 0.1 in 1 minute → Normal, no alert ✓

4. Sensor Failure
   - Sensor stops reporting → Failure alert ✓
   - Sensor returns invalid value → Validation rejects ✓

5. Manual Override
   - Supervisor manually stops fermentation → System accepts ✓
   - Manual action logged → Audit trail shows ✓

6. Data Integrity
   - Reading recorded with timestamp ✓
   - Reading recorded with operator ✓
   - Reading cannot be modified after 30 minutes ✓
```

**Output**:
```yaml
implementation_review:
  review_date: "2026-02-04"
  reviewer: "02_Food_Safety_Manager"
  
  code_review:
    ph_sensor_collection: "ADEQUATE"
    data_validation: "ADEQUATE"
    alert_system: "ADEQUATE"
    audit_logging: "ADEQUATE"
    error_handling: "ADEQUATE"
  
  test_review:
    test_coverage: "85% (acceptable)"
    test_scenarios:
      normal_operation: "VERIFIED"
      ph_out_of_bounds: "VERIFIED"
      rapid_change: "VERIFIED"
      sensor_failure: "VERIFIED"
      manual_override: "VERIFIED"
      data_integrity: "VERIFIED"
  
  issues_found: 0
  conditions_required: 0
  
  next_step: "Production deployment planning"
```

**Auto-Trigger Next Step**: Approval decision

---

### Step 2.2 - Production Deployment Considerations

**Food Safety Manager faz**:
```
1. Training Plan
   - Who needs training? (supervisors, operators)
   - What training? (system operation, alert response)
   - When? (before deployment)
   - How to verify trained? (test quiz)

2. Rollback Plan
   - What if system fails?
   - Can we revert to manual testing?
   - How long manual testing takes?
   - What's the fallback?

3. Monitoring Plan
   - How to monitor for issues?
   - What alerts to watch for?
   - Who gets notified?
   - Response SLA?

4. Regulatory Notification
   - Do regulations require notification? (FDA, etc)
   - When to notify? (before, during, after)
   - What documentation needed?
```

**Output**:
```yaml
deployment_readiness:
  training:
    required_for: ["Batch supervisors", "QC supervisors", "Production manager"]
    topics: ["System operation", "Alert interpretation", "Manual override"]
    scheduled_date: "2026-02-10"
    completion_required_before_go_live: true
  
  rollback_plan:
    fallback_method: "Return to manual pH testing"
    manual_testing_procedure: "[link to procedure]"
    estimated_impact: "Slightly slower (4h lag vs real-time)"
    conditions_to_rollback: ["System unavailable > 30 min", "False alerts > 10/day"]
  
  monitoring:
    metrics_to_track:
      - "pH readings frequency"
      - "Alert frequency"
      - "Alert response time"
      - "False positive rate"
      - "System uptime"
    
    alerts_to_respond_to:
      - "Sensor failure"
      - "Alert system down"
      - "Unusual pH readings"
      - "High false positive rate"
    
    response_sla: "Within 15 minutes"
  
  go_live_date: "2026-02-16"
```

**Auto-Trigger Next Step**: Final approval

---

## FASE 3: APPROVAL & VALIDATION (1 day)

### Step 3.1 - Final Food Safety Approval

**Food Safety Manager faz**:
```
1. Verify All Conditions Met
   - Requirements checklist ✓
   - Tests pass ✓
   - Training planned ✓
   - Rollback plan ready ✓
   - Regulatory issues resolved ✓

2. Make Approval Decision
   - Feature is food safe? YES
   - Implementation is correct? YES
   - Risks are mitigated? YES
   - Team is ready? YES
   
   DECISION: ✓ APPROVED FOR DEPLOYMENT

3. Document Approval
   - Date of approval
   - Who approved
   - What was approved
   - Any conditions
   - Sign-off
```

**Output**:
```yaml
food_safety_approval:
  decision: "APPROVED_FOR_DEPLOYMENT"
  approved_by: "02_Food_Safety_Manager"
  approval_date: "2026-02-05"
  
  approved_for:
    feature: "pH Data Collection API"
    deployment_date: "2026-02-16"
    production_environment: "SmartLab Production"
  
  conditions:
    - "Staff trained before go-live"
    - "Monitoring active before deployment"
    - "Rollback plan tested"
  
  signature: "Food Safety Manager - Digital Signature"
  
  documentation:
    - "Approval rationale: Real-time monitoring safer than 4-hour lag"
    - "Food safety improvement: Prevents out-of-spec batches"
    - "Risk mitigation: Sensor failure handled, manual override available"
    - "Regulatory compliance: Meets FDA FSMA requirements"
```

**Auto-Trigger Next Step**: Notify PM for deployment

---

### Step 3.2 - Post-Deployment Monitoring

**Food Safety Manager faz** (ongoing):
```
First 24 hours:
- Monitor pH readings for normality
- Check alert frequency (should be low)
- Verify no system errors
- Ready to recommend rollback if needed

First week:
- Continue monitoring metrics
- Check staff comfort with system
- Address any questions/concerns
- Review initial data quality

Post-implementation review (2 weeks):
- Data quality analysis
- Alert accuracy analysis
- Staff training effectiveness
- Any process improvements
- Update HACCP plan if needed
- Document lessons learned
```

**Output**:
```yaml
post_deployment_monitoring:
  status: "ACTIVE"
  
  metrics_first_24h:
    sensor_readings: "2880 readings (every 30 sec) ✓"
    alert_frequency: "3 alerts (normal) ✓"
    system_uptime: "100% ✓"
    data_quality: "Excellent ✓"
  
  staff_feedback:
    - "System is easy to use"
    - "Alerts are clear"
    - "Prefer automated over manual"
  
  initial_assessment: "✓ DEPLOYMENT_SUCCESSFUL"
  
  next_review: "2026-02-19 (2 week post-implementation)"
```

---

## Decision Outcomes

### ✓ APPROVED
```
Feature meets all food safety requirements
- Implementation correct
- Tests pass
- No safety risks
- Training complete
- Rollback plan ready

→ Approved for production deployment
```

### ⚠️ APPROVED WITH CONDITIONS
```
Feature mostly good, but conditions must be met:
- Condition 1: [must do X before deployment]
- Condition 2: [must verify Y]
- Condition 3: [must train on Z]

→ Approved ONLY IF conditions met by [date]
```

### ✗ REJECTED
```
Feature has safety gaps that must be fixed:
- Issue 1: [specific food safety problem]
- Issue 2: [specific regulatory violation]
- Issue 3: [specific risk not mitigated]

→ REJECTED - Must fix before resubmission
```

---

## Escalation Scenarios

```
Scenario 1: Food Safety Disagree with Production Manager
- Food Safety: "pH must be continuous monitoring"
- Production: "4-hour manual testing is sufficient"
→ Escalate to Domain Expert Coordinator
→ Coordinator facilitates discussion
→ PM makes final decision if can't align

Scenario 2: Regulatory Compliance Question
- Question: "Do regulations require FDA notification?"
→ Consult Regulatory Officer
→ Food Safety Manager implements decision

Scenario 3: Production Feasibility Question  
- Question: "Can production stop fermentation in 30 seconds?"
→ Consult Production Manager
→ Food Safety Manager verifies adequate

Scenario 4: Deployment Risk
- Risk: "System may have bugs in production"
→ Recommend extended testing
→ Recommend phased rollout
→ Recommend intensive monitoring
→ Approve deployment with precautions
```

---

## Success Metrics

Food Safety Manager is successful if:
- ✓ Feature meets food safety requirements
- ✓ HACCP critical controls are protected
- ✓ Risks are identified and mitigated
- ✓ Regulatory compliance verified
- ✓ Team trained before deployment
- ✓ Post-deployment monitoring shows no food safety issues
- ✓ Data quality excellent
- ✓ No safety incidents post-deployment

---

## Audit Trail

**All decisions documented**:
```
- Who reviewed: [Food Safety Manager name]
- When reviewed: [date/time]
- What was reviewed: [feature description]
- How was reviewed: [code review, tests review, etc]
- What was found: [issues or concerns]
- Decision made: [APPROVED / CONDITIONAL / REJECTED]
- Why: [rationale for decision]
- Signature: [digital signature]
- Follow-up: [any follow-up actions or monitoring]
```

This creates **non-repudiation** - cannot claim food safety wasn't reviewed.

---

**Food Safety Manager Workflow Version**: 1.0  
**Last Updated**: 2026-01-30

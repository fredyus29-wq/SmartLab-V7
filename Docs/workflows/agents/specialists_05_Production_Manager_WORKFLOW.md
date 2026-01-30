# AGENT INDIVIDUAL WORKFLOW: Production Manager (Specialist 05)

**Agente**: Production Manager - Domain Expert (Production Operations)  
**Autoridade**: Level 9 (Specialist)  
**Fases**: Feasibility Assessment → Operational Planning → Deployment Support

---

## Workflow Entrada

**Trigger que ativa**:
- Status = FEATURE_READY_FOR_SPECIALIST_REVIEW
- Event = PRODUCTION_REVIEW_REQUIRED
- Feature involves: Production processes, batch operations, scheduling, equipment

**Input**:
```
{
  "task_id": "PH-002",
  "task_title": "pH Data Collection API",
  "feature_description": "Automated pH monitoring during fermentation",
  "affected_areas": ["Fermentation", "Batch scheduling", "Equipment usage"],
  "architecture_spec": "[links]",
  "implementation_complete": true,
  "tests_complete": true,
  "submission_date": "2026-02-04"
}
```

---

## FASE 1: FEASIBILITY ASSESSMENT (1 day)

### Step 1.1 - Production Operational Feasibility

**Production Manager faz**:
```
1. Understand Current Operations
   - How many batches per day? (3-5 batches)
   - How long each batch? (48 hours)
   - How many fermentations running? (8 active)
   - How many operators monitoring? (2 per shift)

2. Understand Change Impact
   - What's different in the new system?
   - How does it change operator workflow?
   - How much time saved? (15 min/4 hours = 30% saved)
   - Does it create new tasks?

3. Assess Feasibility
   - Can current equipment run sensors? (YES - I²C capable)
   - Do we have network in production? (Ethernet + WiFi)
   - Is environment compatible? (Temperature, humidity OK)
   - Are operators available for change?

4. Identify Operational Risks
   - What could go wrong operationally?
   - What's the impact if system fails?
   - How would we operate without it?
   - What's the recovery procedure?
```

**Example - pH Monitoring System**:
```
CURRENT OPERATIONS:
- 5 fermentation batches running daily
- 48-hour fermentation cycle each
- 8 simultaneous active fermentations
- 2 operators per shift (8 shifts per week)
- Manual pH testing: every 4 hours = 12 tests per batch
- Time per test: 15 minutes = 180 minutes/batch/day
- Manual data entry to LIMS: 10 minutes per batch

NEW OPERATIONS:
- 5 fermentation batches running daily (unchanged)
- 48-hour fermentation cycle (unchanged)
- 8 simultaneous active fermentations (unchanged)
- 2 operators per shift (unchanged)
- Automated pH reading: every 30 seconds (continuous)
- Operator reviews readings: 5 minutes per batch (vs 180 minutes)
- Automatic data entry to LIMS: 0 minutes (automated)

TIME SAVED: 180 min + 10 min = 190 minutes per batch per day
TIME SAVED: 190 min × 5 batches = 950 minutes per day (~16 hours)
IMPACT: Operators can focus on other critical tasks
```

**Output**:
```yaml
production_feasibility:
  current_operations:
    batches_per_day: 5
    batch_duration: "48 hours"
    active_fermentations: 8
    operators_per_shift: 2
    shifts_per_week: 8
  
  operational_change_analysis:
    monitoring_frequency:
      before: "Every 4 hours (12 tests/batch)"
      after: "Continuous (every 30 seconds)"
      improvement: "Real-time vs delayed detection"
    
    operator_workload:
      before: "190 minutes per batch (testing + data entry)"
      after: "5 minutes per batch (review readings)"
      savings: "185 minutes per batch per day = 16 hours"
      capability: "Operators available for other tasks"
    
    equipment_compatibility:
      power_supply: "AVAILABLE (220V 3-phase)"
      network_connectivity: "AVAILABLE (Ethernet + WiFi)"
      environmental_conditions: "COMPATIBLE (20-30°C, 40-70% humidity)"
      sensor_placement: "FEASIBLE (fermentation vessel mount point identified)"
    
    operator_acceptance: "POSITIVE - less manual testing appreciated"
  
  operational_risks:
    - risk: "Sensor fails during batch"
      impact: "Lose real-time pH data, revert to manual testing"
      probability: "LOW (< 5%)"
      mitigation: "Daily sensor check, spare sensors available"
    
    - risk: "Network connectivity loss"
      impact: "Readings not transferred to LIMS"
      probability: "VERY LOW (< 1%)"
      mitigation: "Local buffer storage, retry on reconnect"
    
    - risk: "Operator ignores automated alerts"
      impact: "Out-of-spec batch not detected"
      probability: "MEDIUM (depends on training)"
      mitigation: "Alarm system (audible + visual), escalation"
  
  feasibility_assessment: "FEASIBLE"
  deployment_recommendation: "PROCEED"
```

**Auto-Trigger Next Step**: Operational planning

---

### Step 1.2 - Capacity & Resource Planning

**Production Manager faz**:
```
1. Equipment Requirements
   - How many sensors needed? (8 fermentations + spares)
   - How many sensor sets? (8 active + 2 backup)
   - Installation requirements? (2 days)
   - Maintenance requirements? (daily checks, monthly calibration)

2. Personnel Requirements
   - Who installs sensors? (Maintenance technician)
   - Who monitors alerts? (Batch supervisor)
   - Who troubleshoots? (Maintenance, QC)
   - Training needs? (All supervisors)

3. Scheduling Impact
   - Can we install without stopping production? (Yes, during batch change)
   - How long installation takes? (2 days, 1-2 batches delayed)
   - Best time window? (Monday-Tuesday, low production)
   - Contingency schedule? (Extend to Thursday if issues)

4. Cost Impact
   - Equipment cost? ($X per sensor)
   - Installation labor? (16 hours × hourly rate)
   - Training cost? (4 hours × trainer rate)
   - Maintenance cost? (2 hours/month)
```

**Output**:
```yaml
capacity_and_resources:
  equipment_requirements:
    sensors_needed: 8
    backup_sensors: 2
    sensor_sets_total: 10
    installation_effort: "16 person-hours"
    lead_time: "2 weeks"
  
  personnel_requirements:
    installer: "Maintenance technician (2 days)"
    trainer: "QC supervisor (4 hours)"
    monitor: "Batch supervisors (ongoing)"
    troubleshooter: "Maintenance team (on-call)"
  
  scheduling_impact:
    installation_window: "Monday-Tuesday (off-peak)"
    duration: "2 days"
    batches_affected: "1-2 batches delayed"
    production_impact: "< 5% (acceptable)"
  
  resource_availability:
    maintenance_technician: "AVAILABLE - week of 2/10"
    trainer_available: "AVAILABLE - week of 2/10"
    production_capacity: "OK - can absorb delay"
  
  cost_estimate:
    equipment: "$X total"
    installation: "$Y total"
    training: "$Z total"
    annual_maintenance: "$M total"
    roi: "Positive within 3 months (labor savings)"
```

**Auto-Trigger Next Step**: Operational planning

---

## FASE 2: OPERATIONAL PLANNING (1 day)

### Step 2.1 - Production Procedures & Training

**Production Manager faz**:
```
1. Define Production Procedure Changes
   - What do supervisors do differently?
   - When do they check system?
   - How do they respond to alerts?
   - How do they manually override if needed?

2. Create Monitoring Procedures
   - What alerts to watch for?
   - What's the response procedure?
   - Who's responsible for each action?
   - What's the timeline for response?

3. Create Failure Recovery Procedures
   - If sensor fails, what happens?
   - If network fails, what happens?
   - If system crashes, what happens?
   - How do we revert to manual testing?

4. Identify Training Needs
   - Operators need to: understand system, respond to alerts
   - Supervisors need to: understand system, troubleshoot
   - Maintenance needs to: maintain sensor, replace if failed
   - QC needs to: understand data quality

5. Create Training Plan
   - Who trains? (External vendor + internal champion)
   - When? (Before deployment)
   - Content? (System operation, alert response, troubleshooting)
   - Duration? (4 hours classroom + 2 hours hands-on)
   - Verification? (Test quiz, practical demonstration)
```

**Output**:
```yaml
operational_planning:
  procedure_changes:
    ph_monitoring:
      old: "Manual testing every 4 hours"
      new: "Continuous automated monitoring, operator reviews alerts"
      change: "Shift from active testing to reactive response"
    
    alert_response:
      trigger: "pH outside 3.0-4.0"
      immediate_action: "Operator reads alert, assesses batch"
      decision_point: "Is batch recoverable?"
      action_if_yes: "Add acid/buffer, adjust fermentation"
      action_if_no: "Report out-of-spec, escalate to QC"
      timeout: "15 minutes from alert"
    
    data_management:
      automatic: "Continuous readings stored in system"
      operator_action: "Review readings, confirm accuracy"
      system_action: "Auto-transfer to LIMS"
      verification: "Weekly data quality review"
  
  failure_recovery_procedures:
    sensor_failure:
      detection: "No readings for > 5 minutes"
      response: "Switch to backup sensor (manual action)"
      timeline: "< 15 minutes"
      escalation: "If backup also fails, return to manual testing"
    
    network_failure:
      detection: "No data transfer for > 1 hour"
      response: "System stores locally, retries on reconnect"
      timeline: "Automatic recovery when network restored"
      fallback: "Manual data entry if unable to restore"
    
    system_crash:
      detection: "Manual check or IT alert"
      response: "Restart system, verify data integrity"
      timeline: "< 30 minutes"
      fallback: "Return to manual pH testing during troubleshooting"
  
  training_plan:
    target_groups:
      - group: "Batch supervisors (8 people)"
        duration: "4 hours classroom + 2 hours hands-on"
        content: ["System overview", "Alert response", "Troubleshooting"]
        completion_deadline: "2026-02-15"
      
      - group: "QC staff (4 people)"
        duration: "2 hours"
        content: ["Data quality", "Verification procedures"]
        completion_deadline: "2026-02-15"
      
      - group: "Maintenance (2 people)"
        duration: "4 hours"
        content: ["Sensor maintenance", "Replacement procedure"]
        completion_deadline: "2026-02-15"
    
    training_materials: "PROVIDED by vendor"
    trainer_qualification: "Vendor-certified + internal champion"
    verification: "Test quiz (80% pass score required)"
  
  go_live_readiness:
    equipment_installed: "YES (2/9)"
    training_completed: "YES (by 2/15)"
    procedures_updated: "YES (by 2/12)"
    staff_comfortable: "YES (post-training assessment)"
    go_live_date: "2026-02-16"
```

**Auto-Trigger Next Step**: Operational readiness check

---

### Step 2.2 - Production Schedule & Contingency

**Production Manager faz**:
```
1. Installation Schedule
   - Week of Feb 10: Install sensors (Monday-Tuesday)
   - Week of Feb 10: Train staff (Wednesday-Friday)
   - Week of Feb 16: Begin using system in production

2. Contingency Plans
   - If installation delayed? Have backup week (Feb 17-18)
   - If training incomplete? Can delay go-live 1 week
   - If sensors fail? Return to manual testing
   - If issues post-launch? Rollback to previous system

3. Monitoring Schedule
   - Week 1: Daily check-ins (daily stand-up)
   - Week 2-4: Weekly review (Monday meetings)
   - Month 2-3: Monthly review (first of month)
   - Ongoing: SLA monitoring (99% uptime)

4. Success Metrics
   - System uptime > 99%
   - Alert accuracy > 95%
   - Operator response time < 15 minutes
   - Zero safety incidents
   - Staff satisfaction > 4/5
   - Zero unplanned downtime
```

**Output**:
```yaml
production_schedule:
  phase1_installation: "Feb 10-11 (Monday-Tuesday)"
  phase2_training: "Feb 12-14 (Wednesday-Friday)"
  phase3_go_live: "Feb 16 (Monday)"
  
  contingency_schedule:
    installation_delay_backup: "Feb 17-18"
    training_delay_backup: "Feb 19-21"
    go_live_delay_buffer: "Can extend 1 week if needed"
  
  post_launch_monitoring:
    week1: "Daily check-ins (Mon-Fri 9 AM)"
    week2_4: "Weekly review (Monday 2 PM)"
    ongoing: "Monthly SLA review"
  
  success_metrics:
    system_uptime: "> 99%"
    alert_accuracy: "> 95%"
    operator_response_time: "< 15 minutes"
    safety_incidents: "0"
    staff_satisfaction: "> 4/5"
    unplanned_downtime: "0"
  
  rollback_plan:
    trigger: "Critical issue (> 1 hour downtime or safety concern)"
    rollback_procedure: "Return to manual pH testing"
    rollback_timing: "< 30 minutes decision + execution"
    communication: "Notify all staff immediately"
    post_mortem: "Within 24 hours, identify root cause"
```

**Auto-Trigger Next Step**: Final approval

---

## FASE 3: DEPLOYMENT SUPPORT (ongoing)

### Step 3.1 - Deployment Readiness Review

**Production Manager faz**:
```
1. Final Readiness Check
   - Equipment installed? ✓
   - Staff trained? ✓
   - Procedures updated? ✓
   - Contingency plans ready? ✓
   - Monitoring scheduled? ✓
   
   READY FOR DEPLOYMENT: YES

2. Approval Decision
   - Is production ready? YES
   - Can we operate safely? YES
   - Do we have fallback? YES
   - Are staff confident? YES
   
   DECISION: ✓ APPROVED FOR DEPLOYMENT
```

**Output**:
```yaml
production_approval:
  decision: "APPROVED_FOR_DEPLOYMENT"
  approved_by: "05_Production_Manager"
  approval_date: "2026-02-05"
  
  readiness_verification:
    equipment_installed: true
    staff_trained_and_certified: true
    procedures_documented: true
    contingency_plans: true
    monitoring_prepared: true
  
  production_authorization:
    go_live_date: "2026-02-16"
    installation_week: "Feb 10-11"
    training_week: "Feb 12-14"
    production_support: "Full support first 2 weeks"
  
  signature: "Production Manager - Digital Signature"
```

**Auto-Trigger Next Step**: Deployment execution

---

### Step 3.2 - Go-Live Support & Monitoring

**Production Manager faz** (Week 1 intensive, then ongoing):
```
GO-LIVE WEEK (Feb 10-14):
- Monday-Tuesday: Sensor installation supervised
- Wednesday-Friday: Staff training verified
- Friday: Final system test with full batch

WEEK 1 (Feb 16-20):
- Daily check-ins at 9 AM
- Observe operators using system
- Address questions/concerns immediately
- Monitor for issues
- Track metrics (uptime, accuracy, response time)

WEEK 2-4 (Feb 23 - March 20):
- Weekly meetings (Monday 2 PM)
- Review metrics
- Assess staff confidence
- Identify improvements
- Plan next phase if needed

MONTH 2+:
- Monthly monitoring
- Continuous improvement
- Update procedures if needed
- Document lessons learned
```

**Output**:
```yaml
go_live_support:
  status: "ACTIVE"
  
  first_week_monitoring:
    installation_status: "COMPLETE"
    training_status: "COMPLETE"
    staff_comfort_level: "GOOD"
    system_uptime: "99.5%"
    alert_accuracy: "97%"
    operator_response_time: "12 minutes avg"
  
  operational_status:
    batches_running: "5 per day (normal)"
    fermentation_success_rate: "100% (no spoilage)"
    safety_incidents: "0"
    unplanned_downtime: "0"
  
  staff_feedback:
    operators: "Positive - system is accurate"
    supervisors: "Positive - easier to manage"
    maintenance: "Positive - sensor reliable"
  
  issues_identified: "NONE critical"
  
  assessment: "✓ DEPLOYMENT_SUCCESSFUL"
  
  next_review: "2026-02-23 (1 week)"
```

---

## Success Outcomes

### ✓ PRODUCTION READY
```
- Equipment installed and working
- Staff trained and confident
- Procedures ready
- Contingency plans in place
- Monitoring active

→ Approved for deployment
```

### ⚠️ CONDITIONAL READINESS
```
- Equipment mostly ready, but [specific item] needs [action]
- Staff mostly trained, but [group] needs [additional training]
- Procedures mostly ready, but [section] needs [update]

→ Approved IF conditions met by [date]
```

### ✗ NOT PRODUCTION READY
```
- Equipment issue: [specific technical problem]
- Staff issue: [specific capability gap]
- Procedure issue: [specific operational risk]
- Contingency issue: [specific gap in rollback plan]

→ REJECTED - Must address before go-live
```

---

## Escalation Scenarios

```
Scenario 1: Equipment Not Ready by Deadline
- Issue: Sensors arrive late
→ Contact vendor for expedited delivery
→ Assess alternative installation schedule
→ If unavoidable, delay go-live + notify stakeholders
→ Communicate new schedule to team

Scenario 2: Staff Not Ready
- Issue: Supervisors not confident with system
→ Extend training by 1 week
→ Bring in external trainer for extra sessions
→ Schedule practical hands-on before go-live
→ Delay go-live if necessary

Scenario 3: Safety Concern Discovered
- Issue: Operator could miss alert in noisy environment
→ Add visual alerts (blinking light)
→ Add second confirmation method
→ Test in actual production environment
→ Address before go-live

Scenario 4: Post-Deployment Issue
- Issue: System unstable, frequent failures
→ Implement contingency (return to manual testing)
→ Troubleshoot issue with engineering team
→ Fix and re-test
→ Re-deploy when confident
```

---

## Production Manager Success Metrics

Production Manager is successful if:
- ✓ Equipment installed and operational
- ✓ All staff trained and confident
- ✓ Procedures updated and followed
- ✓ Go-live executed smoothly
- ✓ Zero safety incidents
- ✓ System uptime > 99%
- ✓ Operator response time < 15 minutes
- ✓ Staff satisfaction > 4/5
- ✓ Production schedule maintained (no batches lost)
- ✓ Zero unplanned downtime
- ✓ Lessons learned documented

---

**Production Manager Workflow Version**: 1.0  
**Last Updated**: 2026-01-30

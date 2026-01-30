# AGENT INDIVIDUAL WORKFLOW: PM Tech Lead (Agent 01)

**Agente**: PM Tech Lead - Project Management & Leadership  
**Autoridade**: Level 10  
**Fases**: Planning → Execution Oversight → Validation

---

## Workflow Entrada

**Trigger que ativa**: 
- Status = REQUEST_CREATED
- Event = NEW_TASK_INTAKE

**Input**:
```
{
  "request_id": "REQ-2026-001",
  "request_title": "Add pH monitoring to production",
  "request_description": "Long description",
  "priority": "high",
  "deadline": "2026-02-28",
  "requester": "Production Manager",
  "context": "Additional context"
}
```

---

## FASE 1: REQUEST INTAKE (2 hours)

### Step 1.1 - Entender Requisição

**O PM faz**:
```
1. Read Request
   - Title: [what is this]
   - Description: [full context]
   - Priority: [high/medium/low]
   - Deadline: [when needed]
   - Requester: [who asked]

2. Clarification Questions
   - What's the business objective? (why do we need this)
   - Who are the users? (who uses this feature)
   - What's the scope? (what's included/excluded)
   - What are constraints? (budget, time, resources)
   - Are there dependencies? (what must exist first)

3. Preliminary Assessment
   - Feasibility: [high/medium/low]
   - Complexity: [simple/medium/complex]
   - Risk Level: [low/medium/high]
   - Timeline Estimate: [days/weeks needed]
```

**Output**:
```yaml
request_analysis:
  title: "Add pH monitoring to production"
  business_objective: "Monitor pH levels in real-time"
  users: "Production operators, QC supervisor"
  scope:
    included:
      - pH sensor integration
      - Data collection system
      - Alert thresholds
      - Historical data storage
    excluded:
      - MES integration (Phase 2)
      - Predictive analytics (Phase 3)
  constraints:
    budget: "$50K"
    timeline: "6 weeks"
    resources: "2 backend devs, 1 frontend dev"
  dependencies:
    - Database schema ready
    - API framework in place
  preliminary_assessment:
    feasibility: "high"
    complexity: "medium"
    risk: "medium"
    timeline_estimate: "4-6 weeks"
```

**Auto-Trigger Next Step**: Proceed to Decomposition

---

### Step 1.2 - Decompose into Tasks

**O PM faz**:
```
1. Break into Components
   - Hardware: pH sensor + connection
   - Backend: Data collection API + storage
   - Frontend: Dashboard display
   - QA: Testing + validation
   - Deployment: Setup + monitoring

2. Identify Task Dependencies
   - Task A must complete before Task B
   - Task C can run in parallel with Task A & B

3. Estimate Effort (per task)
   - Research: 3 days
   - Backend API: 5 days
   - Frontend Dashboard: 4 days
   - Testing: 3 days
   - Integration: 2 days
   - Total: ~20 days (4 weeks)

4. Assign Ownership
   - Backend: Senior Backend Developer
   - Frontend: Senior Frontend Developer
   - Testing: QA Specialist
```

**Output**:
```yaml
task_breakdown:
  - task_id: "PH-001"
    title: "pH Sensor Research & Integration Plan"
    owner: "04_DevOps_Specialist"
    effort: "3 days"
    depends_on: []
    priority: 1

  - task_id: "PH-002"
    title: "pH Data Collection API"
    owner: "07_Backend_Core_Dev"
    effort: "5 days"
    depends_on: ["PH-001"]
    priority: 2

  - task_id: "PH-003"
    title: "pH Data Storage & Retrieval"
    owner: "11_Database_Engineer"
    effort: "2 days"
    depends_on: ["PH-002"]
    priority: 3

  - task_id: "PH-004"
    title: "pH Monitoring Dashboard UI"
    owner: "10_Frontend_Module_Dev"
    effort: "4 days"
    depends_on: ["PH-002"]
    priority: 4

  - task_id: "PH-005"
    title: "Alert Rules & Notifications"
    owner: "08_Backend_Domain_Dev"
    effort: "3 days"
    depends_on: ["PH-002"]
    priority: 5

  - task_id: "PH-006"
    title: "Integration Testing"
    owner: "05_QA_Security_Specialist"
    effort: "3 days"
    depends_on: ["PH-002", "PH-004", "PH-005"]
    priority: 6

  - task_id: "PH-007"
    title: "Production Deployment & Monitoring"
    owner: "04_DevOps_Specialist"
    effort: "2 days"
    depends_on: ["PH-006"]
    priority: 7
```

**Auto-Trigger Next Step**: Send to Architects

---

### Step 1.3 - Identify Required Specialists

**O PM faz**:
```
1. Check which specialists needed
   - Food Safety: YES (HACCP consideration)
   - Production: YES (affects production workflow)
   - QC Lab: YES (quality control requirement)
   - QMS: MAYBE (if quality standard compliance)
   - Regulatory: MAYBE (if regulatory requirement)

2. Document Why Needed
   - Food Safety: Confirm pH is CCP + critical limits
   - Production: Confirm feasibility + timeline realistic
   - QC Lab: Confirm test method + equipment available

3. Create Specialist Review List
   - Contact order
   - Review scope
   - Timeline for approval
```

**Output**:
```yaml
specialist_coordination:
  required_specialists:
    - specialist: "02_Food_Safety_Manager"
      reason: "pH is HACCP Critical Control Point"
      review_scope: "Food safety implications"
      timeline: "3 days"
    
    - specialist: "05_Production_Manager"
      reason: "Production workflow affected"
      review_scope: "Equipment feasibility + timeline"
      timeline: "2 days"
    
    - specialist: "06_QC_Supervisor"
      reason: "Lab operations impact"
      review_scope: "Test method validation"
      timeline: "2 days"
```

**Auto-Trigger Next Step**: Notify Architects + Specialists

---

## FASE 2: MONITORING & COORDINATION

### Step 2.1 - Track Phase Progress

**Ongoing throughout execution**:
```
Daily Check-in:
- Is development on track?
- Are specialists providing timely feedback?
- Any blockers or risks?
- Team morale?

Metrics tracked:
- Tasks completed vs planned
- Velocity (tasks/day)
- Test coverage
- Quality metrics
- Timeline confidence
```

### Step 2.2 - Address Blockers

**If blocker detected**:
```
1. Identify Blocker
   - What's blocking progress?
   - Why is it blocked?
   - Who can unblock?

2. Assess Impact
   - Does it affect timeline? (days/weeks)
   - Does it affect scope?
   - Critical or can work around?

3. Take Action
   - Can PM unblock? Do it.
   - Does architect need to decide? Escalate.
   - Does specialist need to decide? Coordinate.

4. Notify Team
   - Explain blocker
   - Explain solution
   - Update timeline if needed
```

### Step 2.3 - Manage Timeline

**If timeline at risk**:
```
1. Assess Risk
   - How many days behind?
   - What's causing delay?
   - Can we catch up?

2. Options
   - Add resources? (more developers)
   - Reduce scope? (MVP vs full)
   - Extend deadline? (negotiate with requester)
   - Work harder? (not sustainable long-term)

3. Decide & Communicate
   - Which option to pursue?
   - Notify stakeholders
   - Update plans
```

---

## FASE 3: QUALITY VALIDATION

### Step 3.1 - Review Quality Gates

**When execution complete**:
```
1. Functionality Gate
   - Feature works as specified? ✓
   - Acceptance criteria met? ✓
   - Known bugs? (how many, severity?)

2. Security Gate
   - No critical vulnerabilities? ✓
   - No high severity issues? ✓
   - Dependency scan passed? ✓

3. Performance Gate
   - Response time acceptable? ✓
   - Load testing passed? ✓
   - No memory issues? ✓

4. Specialist Approval
   - Food Safety Manager approved? ✓
   - Production Manager approved? ✓
   - QC Supervisor approved? ✓

5. Architecture Review
   - Backend Architect approved? ✓
   - Frontend Architect approved? ✓
   - Database Engineer approved? ✓
```

### Step 3.2 - Final PM Approval

**Before deployment**:
```
Decision Checklist:
- ✓ All code reviewed & approved
- ✓ All tests passing
- ✓ All quality gates passed
- ✓ All specialists approved
- ✓ Timeline acceptable
- ✓ Team ready
- ✓ Deployment plan ready

DECISION:
- ✓ APPROVED FOR RELEASE
- ⚠️ HOLD - conditions below
- ✗ REJECTED - issues below
```

**Output**:
```yaml
pm_approval_decision:
  decision: "APPROVED_FOR_RELEASE"
  timestamp: "2026-02-15T14:00:00Z"
  approved_by: "01_PM_Tech_Lead"
  deployment_date: "2026-02-16T09:00:00Z"
  rollback_plan: "Documented and tested"
  monitoring_active: true
  team_notified: true
```

**Auto-Trigger**: Deploy to production

---

## Step 3.3 - Post-Deployment Monitoring

**After deployment**:
```
First 24 hours:
- Monitor for errors
- Check performance metrics
- Alert if issues
- Ready to rollback if needed

First week:
- Monitor usage
- Gather user feedback
- Fix minor issues
- Performance stable?

Post-deployment review (1 week):
- Did it meet objectives?
- Any unexpected issues?
- Performance good?
- Users happy?
- Lessons learned?
```

---

## Output Format

**Complete Task Output**:
```yaml
task_completion:
  task_id: "REQ-2026-001"
  title: "Add pH monitoring to production"
  status: "COMPLETED_AND_DEPLOYED"
  
  timeline:
    planned_completion: "2026-02-28"
    actual_completion: "2026-02-15"
    days_early: 13
  
  quality_metrics:
    test_coverage: "85%"
    bugs_found_and_fixed: 3
    critical_issues: 0
    high_issues: 0
    
  specialist_approvals:
    - "02_Food_Safety_Manager": "APPROVED"
    - "05_Production_Manager": "APPROVED"
    - "06_QC_Supervisor": "APPROVED"
    
  architecture_signoff:
    - "02_Backend_Architecture_Lead": "APPROVED"
    - "03_Frontend_Architecture_Lead": "APPROVED"
    
  pm_approval: "APPROVED_FOR_RELEASE"
  deployment_status: "LIVE_IN_PRODUCTION"
  
  post_deployment:
    hours_monitoring: 24
    issues_found: 0
    user_feedback: "Positive"
    performance: "Exceeds expectations"
    
  lessons_learned:
    - "Specialist consultation early prevented rework"
    - "Timeline estimate very accurate"
    - "Team collaboration excellent"
    - "Code quality high first time"
```

---

## Escalation Paths

```
IF developer blocked:
  → Notify architect
  → Architect helps unblock
  → Continue work

IF timeline at risk:
  → Notify stakeholders
  → Discuss options
  → Make decision
  → Update plans

IF specialist disagree:
  → Contact Domain Expert Coordinator
  → Coordinator facilitates resolution
  → PM decides if needed

IF critical issue in production:
  → Immediate incident response
  → Evaluate rollback
  → Post-incident review
```

---

## Success Metrics

PM is successful if:
- ✓ Feature delivered on time (or early)
- ✓ Quality gates all passed
- ✓ Specialists approved
- ✓ No post-deployment issues
- ✓ Team satisfied
- ✓ Users happy
- ✓ Stakeholders notified & satisfied

---

**PM Tech Lead Workflow Version**: 1.0  
**Last Updated**: 2026-01-30

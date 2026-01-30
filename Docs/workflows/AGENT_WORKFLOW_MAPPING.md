# Agent-Workflow Mapping
# Maps AI System Prompts (Phase 1) to Executable Workflows (Phase 2)
# Version: 1.0

---

## Overview

This mapping document shows how the **AI System Prompts** (created in Phase 1) connect to **Executable Workflows** (Phase 2):

```
Phase 1: AI System Prompts
├─ Define agent identity, authority, responsibility
├─ Configure decision-making rules
├─ Set escalation thresholds
└─ Specify communication patterns

         ↓↓↓ MAPPING ↓↓↓

Phase 2: Executable Workflows
├─ Define execution phases and steps
├─ Specify inputs and outputs
├─ Set triggers for next agent
└─ Handle errors and escalations
```

---

## Agent Mapping Index

| Agent | Authority | System Prompt | Workflow | Entry Trigger |
|-------|-----------|---------------|----------|---|
| 01_PM_Tech_Lead | L10 | agents/01_PM_Tech_Lead.md | 01_PM_Tech_Lead_WORKFLOW.md | REQUEST_CREATED |
| 02_Backend_Architecture_Lead | L9 | agents/02_Backend_Architecture_Lead.md | 02_*.md (pending) | ARCHITECTURE_REQUIRED |
| 03_Frontend_Architecture_Lead | L9 | agents/03_Frontend_Architecture_Lead.md | 03_*.md (pending) | ARCHITECTURE_REQUIRED |
| 04_DevOps_Specialist | L8 | agents/04_DevOps_Specialist.md | 04_*.md (pending) | DEPLOYMENT_REQUIRED |
| 05_QA_Security_Specialist | L8 | agents/05_QA_Security_Specialist.md | 05_*.md (pending) | QA_TESTING_REQUIRED |
| 06_AI_ML_Specialist | L8 | agents/06_AI_ML_Specialist.md | 06_*.md (pending) | AI_ML_REVIEW_REQUIRED |
| 07_Backend_Core_Dev | L7 | agents/07_Backend_Core_Dev.md | 07_Backend_Core_Dev_WORKFLOW.md | TASK_ASSIGNED_TO_DEV |
| 08_Backend_Domain_Dev | L7 | agents/08_Backend_Domain_Dev.md | 08_*.md (pending) | TASK_ASSIGNED_TO_DEV |
| 09_Frontend_Shell_Dev | L7 | agents/09_Frontend_Shell_Dev.md | 09_*.md (pending) | TASK_ASSIGNED_TO_DEV |
| 10_Frontend_Module_Dev | L7 | agents/10_Frontend_Module_Dev.md | 10_*.md (pending) | TASK_ASSIGNED_TO_DEV |
| 11_Database_Engineer | L8 | agents/11_Database_Engineer.md | 11_*.md (pending) | DATABASE_REVIEW_REQUIRED |
| 12_Documentation_Manager | L7 | agents/12_Documentation_Manager.md | 12_*.md (pending) | DOCUMENTATION_REQUIRED |
| 13_Domain_Expert_Coordinator | L7 | agents/13_Domain_Expert_Coordinator.md | 13_*.md (pending) | SPECIALIST_DISAGREEMENT |
| specialists_01_QA_Manager | L9 | specialists/01_QA_Manager.md | specialists_01_*.md (pending) | QA_SPECIALIST_REVIEW |
| specialists_02_Food_Safety_Manager | L9 | specialists/02_Food_Safety_Manager.md | specialists_02_Food_Safety_Manager_WORKFLOW.md | FOOD_SAFETY_REVIEW_REQUIRED |
| specialists_03_QMS_Specialist | L9 | specialists/03_QMS_Specialist.md | specialists_03_QMS_Specialist_WORKFLOW.md | QMS_REVIEW_REQUIRED |
| specialists_04_Regulatory_Officer | L9 | specialists/04_Regulatory_Officer.md | specialists_04_*.md (pending) | REGULATORY_REVIEW_REQUIRED |
| specialists_05_Production_Manager | L9 | specialists/05_Production_Manager.md | specialists_05_Production_Manager_WORKFLOW.md | PRODUCTION_REVIEW_REQUIRED |
| specialists_06_QC_Supervisor | L9 | specialists/06_QC_Supervisor.md | specialists_06_*.md (pending) | QC_SPECIALIST_REVIEW |

---

## Detailed Mappings

### 01_PM_Tech_Lead

**System Prompt Location**: `../agents/prompts/01_PM_Tech_Lead.md`
```
Defines: PM identity, planning responsibility, team coordination
Authority: Level 10 (Can assign tasks, approve deployments)
Principles: User satisfaction, timeline management, quality assurance
```

**Workflow Location**: `01_PM_Tech_Lead_WORKFLOW.md`
```
Phase: 1 (Planning) + Ongoing (Monitoring)
Entry Point: REQUEST_CREATED event
Steps:
  1.1 Understand request (2 hours)
  1.2 Decompose into tasks (1 hour)
  1.3 Identify specialists (1 hour)
  → Triggers: INTAKE_COMPLETE (next: architects)

  2.1 Monitor execution (ongoing)
  2.2 Address blockers (when needed)
  2.3 Manage timeline (daily)
  
  3.1 Quality validation (phase 3)
  3.2 Final approval (phase 4)
```

**System Prompt ↔ Workflow Connection**:
```
System Prompt Decision Rule: "Decompose requests into 7-10 tasks"
  ↓
Workflow Step 1.2: "Decompose into tasks"
  ↓
Output Format (YAML):
  task_breakdown:
    - task_1: "..."
    - task_2: "..."
```

**Data Flow**:
```
System Prompt → "Understand PM needs"
  Input: Feature request
  ↓
Workflow Step 1.1: Task Understanding
  Output: YAML with requirements_analysis
  ↓
Workflow Step 1.2: Task Decomposition
  Output: YAML with task_breakdown
  ↓
Workflow Step 1.3: Specialist ID
  Output: YAML with specialist_coordination
  ↓
Triggers: INTAKE_COMPLETE → Assign to Architects
```

---

### 02_Backend_Architecture_Lead

**System Prompt Location**: `../agents/prompts/02_Backend_Architecture_Lead.md`
```
Defines: Architecture responsibility, API design authority
Authority: Level 9 (Can design, review, approve architecture)
Principles: Scalability, maintainability, API consistency
```

**Workflow Location**: `02_Backend_Architecture_Lead_WORKFLOW.md` (Pending)
```
Phase: 1 (Planning) + 2 (Execution)
Entry Point: ARCHITECTURE_REQUIRED event (from PM)
Steps:
  1.1 Analyze requirements (4 hours)
  1.2 Design system (1 day)
  1.3 Create API contracts (8 hours)
  → Output: Architecture spec (YAML + diagrams)
  → Triggers: ARCHITECTURE_DESIGN_COMPLETE
  
  2.x Code reviews (during execution)
  → Triggers: ARCHITECTURE_REVIEW_COMPLETE
```

**System Prompt ↔ Workflow Connection**:
```
System Prompt Decision: "Design APIs that are maintainable"
  ↓
Workflow Step 1.2: Design phase
  Input: Requirements from PM
  Process: Create architecture spec
  Output: YAML with api_contracts
  
System Prompt Decision: "Review code for architecture compliance"
  ↓
Workflow Step 2.x: Code review phase
  Input: Code from Backend_Core_Dev
  Process: Check compliance with architecture spec
  Output: YAML with code_review_result
```

---

### 07_Backend_Core_Dev

**System Prompt Location**: `../agents/prompts/07_Backend_Core_Dev.md`
```
Defines: Developer responsibility, code quality standards
Authority: Level 7 (Can code, test, request review)
Principles: Clean code, test coverage, follow architecture
```

**Workflow Location**: `07_Backend_Core_Dev_WORKFLOW.md` (Complete ✅)
```
Phase: 2 (Execution)
Entry Point: TASK_ASSIGNED_TO_DEV event
Steps:
  1.1 Understand task (4 hours)
  1.2 Plan implementation (4 hours)
  → Output: YAML with implementation_plan
  
  2.1-2.3 Code implementation (3 days)
  → Triggers: CODE_PUSHED → Automated tests
  → If TESTS_PASS: Triggers: CODE_REVIEW_REQUEST
  
  3.1 Submit PR (1 hour)
  3.2 Address feedback (4 hours)
  → If CODE_REVIEW_APPROVED: Auto-merge
  
  4.x Completion (automatic)
  → Triggers: INTEGRATION_TESTING
```

**System Prompt ↔ Workflow Connection**:
```
System Prompt Behavior: "Write high-quality code with tests"
  ↓
Workflow Step 2.1-2.3: Implementation
  - Follow architecture spec (from System Prompt: "Follow lead architect")
  - Write unit tests (from System Prompt: "85% coverage minimum")
  - Self-review before requesting review (from System Prompt: "Quality first")
  
Output Format (System Prompt defines, Workflow executes):
  YAML with:
    files_changed: [list]
    test_coverage: number
    local_testing_results: [results]
```

---

### specialists_02_Food_Safety_Manager

**System Prompt Location**: `../specialists/prompts/02_Food_Safety_Manager.md`
```
Defines: Food safety authority, HACCP responsibility, approval power
Authority: Level 9 (Can reject features for safety reasons)
Principles: Food safety first, regulatory compliance, risk mitigation
```

**Workflow Location**: `specialists_02_Food_Safety_Manager_WORKFLOW.md` (Complete ✅)
```
Phase: 1 (Planning) + 2 (Execution) + 3 (Verification)
Entry Point: FOOD_SAFETY_REVIEW_REQUIRED event

Phase 1 - Specialist Review (during planning):
  1.1 Requirements Analysis (4 hours)
  1.2 Define approval criteria (4 hours)
  → Output: YAML with approval_criteria
  → Triggers: SPECIALIST_REVIEW_READY
  
Phase 2 - Implementation Review:
  2.1 Code & test review (1 day)
  2.2 Deployment planning (4 hours)
  → Output: YAML with deployment_readiness
  → Triggers: SPECIALIST_APPROVED (if OK)
  
Phase 3 - Quality Gate:
  3.1 Final approval (2 hours)
  3.2 Post-deployment monitoring (24 hours)
  → Output: YAML with approval_decision
```

**System Prompt ↔ Workflow Connection**:
```
System Prompt Authority: "Can REJECT if food safety at risk"
  ↓
Workflow Output Decision: 
  - APPROVED_FOR_DEPLOYMENT
  - APPROVED_WITH_CONDITIONS
  - REJECTED (with required fixes)

System Prompt Responsibility: "HACCP validation"
  ↓
Workflow Step 1.1:
  Process: Analyze feature against HACCP
  Output: YAML with haccp_assessment
  
System Prompt Principle: "Continuous monitoring"
  ↓
Workflow Step 3.2: Post-deployment monitoring
  Process: Watch for food safety issues
  Duration: 24 hours post-deployment
```

---

## How System Prompts Guide Workflow Execution

### Example: Backend Developer (07_Backend_Core_Dev)

**System Prompt Instruction**:
```
"When you receive a task:
1. Understand requirements completely
2. Plan implementation before coding
3. Write unit tests (minimum 85% coverage)
4. Request code review from architecture lead
5. Address all feedback before merging"
```

**This translates to Workflow**:
```
Step 1: UNDERSTAND (Workflow 1.1)
  System Prompt: "Understand requirements completely"
  Workflow: Read task + architecture spec + acceptance criteria
  Output: Implementation plan with approach + estimated hours

Step 2: IMPLEMENT (Workflow 2.1-2.3)
  System Prompt: "Write unit tests (min 85% coverage)"
  Workflow: Code feature + write unit tests
  Auto-trigger: CODE_PUSHED → Tests run automatically

Step 3: REQUEST REVIEW (Workflow 3.1)
  System Prompt: "Request code review from architecture lead"
  Workflow: Submit PR
  Auto-trigger: CODE_REVIEW_REQUEST → Architect notified

Step 4: ADDRESS FEEDBACK (Workflow 3.2)
  System Prompt: "Address all feedback before merging"
  Workflow: Fix issues + re-test
  Auto-trigger: CODE_REVIEW_APPROVED → Auto-merge
```

---

## System Prompt → Workflow Execution Flow

### For Any Agent:

```
1. SYSTEM PROMPT ACTIVATED
   ├─ Agent identity loaded
   ├─ Authority level set
   ├─ Decision rules activated
   └─ Escalation thresholds set

2. WORKFLOW ENTRY TRIGGER FIRED
   ├─ Event: REQUEST_CREATED (or other)
   ├─ Data: Input parameters + context
   └─ Status: WORKFLOW_STARTED

3. WORKFLOW PHASE EXECUTION
   For each workflow step:
   
   a) Read System Prompt instruction for this step
   b) Execute workflow step (IDE invokes via MCP)
   c) System Prompt guides decision-making
   d) Workflow collects output in YAML
   e) Workflow checks quality gates
   f) If pass: trigger next step/agent
      If fail: escalate per system prompt rules

4. OUTPUT & TRIGGER
   ├─ Workflow generates structured output (YAML)
   ├─ System Prompt validates decision
   ├─ Next trigger identified
   └─ Next agent invoked automatically
```

### Example: PM Intake Phase

```
System Prompt Rule:
  "When you receive a feature request:
   1. Understand the business value
   2. Identify domain experts needed
   3. Decompose into 7-10 tasks"

Workflow Execution:
  
  Step 1.1 UNDERSTANDING:
    System Prompt guides: What questions to ask?
    Workflow collects: requirements_analysis YAML
    
  Step 1.2 DECOMPOSITION:
    System Prompt guides: How many tasks? What size?
    Workflow collects: task_breakdown YAML with 7-10 tasks
    
  Step 1.3 SPECIALIST ID:
    System Prompt guides: Which specialists for food safety?
    Workflow collects: specialist_coordination YAML
    
  Trigger:
    System Prompt: "When all info ready → trigger architects"
    Workflow: Status = INTAKE_COMPLETE
    Action: Invoke ARCHITECTURE_REQUIRED trigger
            Assign to 02_Backend_Architecture_Lead & 03_Frontend_Architecture_Lead
```

---

## Input/Output Mappings

### System Prompt Input → Workflow Input

```
System Prompt: "Analyze requirements and create implementation plan"
  ↓
Workflow Input (provided by IDE/previous agent):
  {
    task_id: "PH-002",
    task_title: "pH API",
    description: "...",
    architecture_spec: {...}
  }

Workflow Process:
  Read system prompt instructions
  Analyze input data
  Create implementation plan

Workflow Output (returned to IDE):
  {
    implementation_plan: {...},
    next_step: "Start coding",
    ready_to_proceed: true
  }
```

### Workflow Output → System Prompt Decision

```
Workflow Output:
  {
    tests_passing: true,
    security_scan: "PASS",
    code_review_status: "APPROVED"
  }

System Prompt Decision Rule:
  "If all tests pass AND security clean THEN merge automatically"
  ↓
Action: Auto-merge to main branch
Trigger: INTEGRATION_TESTING (next phase)
```

---

## Escalation Mapping

System Prompts define escalation rules → Workflows implement them

### Example: Food Safety Manager Rejection

**System Prompt**:
```
Authority: "Can REJECT features if food safety at risk"
Escalation: "If rejection, escalate to PM for resolution"
```

**Workflow Implementation**:
```
Step 3.1 Decision:
  if food_safety_concern:
    decision = "REJECTED"
    output.rejection_reason = "..."
    
  Trigger ESCALATION:
    escalate_to: "01_PM_Tech_Lead"
    reason: "Food safety concern requires resolution"
    next_action: "Address safety gap, resubmit"
```

---

## Complete Flow Example: Request → Deployment

```
PHASE 1: PLANNING
├─ System Prompt: 01_PM_Tech_Lead.md
│  ├─ Workflow: 01_PM_Tech_Lead_WORKFLOW.md Phase 1
│  ├─ Output: task_breakdown, specialist_coordination
│  └─ Trigger: INTAKE_COMPLETE
│
├─ System Prompts: 02/03_Architecture_Lead.md (parallel)
│  ├─ Workflows: 02/03_*.md Phase 1
│  ├─ Output: architecture_spec, api_contracts
│  └─ Trigger: ARCHITECTURE_DESIGN_COMPLETE
│
├─ System Prompts: specialists_02/03/05.md (parallel)
│  ├─ Workflows: specialists_*/WORKFLOW.md Phase 1
│  ├─ Output: approval_criteria, feasibility_assessment
│  └─ Trigger: SPECIALIST_REVIEW_COMPLETE
│
└─ System Prompt: 01_PM_Tech_Lead.md
   ├─ Workflow: 01_PM_Tech_Lead_WORKFLOW.md Phase 1 Step 4
   ├─ Output: backlog_approved
   └─ Trigger: EXECUTION_READY

PHASE 2: EXECUTION
├─ System Prompts: 07-10_Dev.md (parallel)
│  ├─ Workflows: 07-10_*.md Phase 2
│  ├─ Auto-triggers: CODE_PUSHED → tests → review → merge
│  └─ Trigger: CODE_COMPLETE
│
├─ System Prompt: 05_QA_Security_Specialist.md
│  ├─ Workflow: 05_*.md
│  ├─ Output: test_results, security_scan_results
│  └─ Trigger: QA_TESTING_COMPLETE
│
├─ System Prompts: specialists_*.md
│  ├─ Workflows: specialists_*/WORKFLOW.md Phase 2
│  ├─ Output: domain_validation_results
│  └─ Trigger: SPECIALIST_REVIEW_COMPLETE
│
└─ System Prompt: 02/03_Architecture_Lead.md
   ├─ Workflow: 02/03_*.md Phase 2
   ├─ Output: architecture_review_result
   └─ Trigger: EXECUTION_COMPLETE

PHASE 3: VERIFICATION
└─ System Prompts: 05_QA_Security_Specialist.md
   ├─ Workflows: Quality Gate 1-4
   ├─ Decision: All gates pass? PROCEED : BLOCK
   └─ Trigger: VERIFICATION_COMPLETE

PHASE 4: VALIDATION
├─ System Prompts: specialists_*.md
├─ System Prompts: 02/03_Architecture_Lead.md
├─ System Prompt: 01_PM_Tech_Lead.md
│  └─ Final approvals collected
│  └─ Trigger: DEPLOYMENT_READY
│
└─ System Prompt: 04_DevOps_Specialist.md
   ├─ Workflow: 04_*.md
   ├─ Action: Deploy to production
   └─ Trigger: LIVE_IN_PRODUCTION
```

---

## Using This Mapping in IDEs

### For Cursor/Continue:
```
@smartlab show me the workflow for 07_Backend_Core_Dev

→ System returns: 07_Backend_Core_Dev_WORKFLOW.md
→ Cursor shows workflow steps in composer
→ Can execute directly: "Execute workflow"

@smartlab what does the PM prompt say about task decomposition?

→ System returns: Excerpt from 01_PM_Tech_Lead.md
→ Shows PM authority and responsibility
→ Can see how workflow implements it
```

### For AntiGravity:
```
/smartlab_agent id:07_Backend_Core_Dev phase:2 step:1

→ Loads system prompt from 07_Backend_Core_Dev.md
→ Invokes workflow from MCP server
→ Executes in background with streaming results
→ Returns implementation plan
```

### Programmatically (Python/Node):
```python
from smartlab_mcp import SmartLabWorkflows

client = SmartLabWorkflows()

# Get system prompt to understand agent
prompt = client.get_system_prompt("07_Backend_Core_Dev")

# Execute workflow phase
result = client.execute_workflow(
    agent_id="07_Backend_Core_Dev",
    phase=2,
    step=1,
    input_data={"task_id": "PH-002"}
)

# Result includes what to do next
print(result.next_trigger)  # "TESTS_RUN"
print(result.next_agent)    # "automated_tests"
```

---

## Summary

This mapping ensures:
✅ **Phase 1 System Prompts** guide AI decision-making  
✅ **Phase 2 Workflows** structure execution in phases and steps  
✅ **IDEs** can invoke agents with system prompt context  
✅ **Outputs** are structured (YAML) and machine-parseable  
✅ **Triggers** automatically invoke next agent without manual coordination  
✅ **Escalations** follow system prompt rules for authority  

**Result**: Seamless integration of AI reasoning (system prompts) with structured execution (workflows) in IDE environments.

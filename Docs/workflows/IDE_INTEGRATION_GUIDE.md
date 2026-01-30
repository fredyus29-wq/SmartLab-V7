# SmartLab IDE Integration Guide
# How to use SmartLab Workflows in AI-Powered IDEs
# Version: 1.0

---

## 🎯 Quick Start (5 minutes)

### Step 1: Install SmartLab MCP Server

```bash
# Clone SmartLab repository
git clone https://github.com/fredyus29-wq/SmartLab-V7.git

# Install MCP server (choose one)
# Option A: Docker
docker run -d -p 3000:3000 \
  -v $(pwd)/Docs/workflows:/app/workflows \
  smartlab-mcp-server:latest

# Option B: Node.js
npm install smartlab-mcp-server
npm start

# Option C: Python
pip install smartlab-mcp-server
smartlab-mcp-server --port 3000
```

### Step 2: Configure Your IDE

#### Cursor
1. Open Settings → Extensions → SmartLab
2. Enable "SmartLab Workflows"
3. Configure:
   ```json
   {
     "smartlab.mcp_server": "http://localhost:3000",
     "smartlab.auto_trigger": true,
     "smartlab.notifications": "sidebar"
   }
   ```
4. Restart Cursor

#### Continue
1. Edit `~/.continue/config.json`:
```json
{
  "models": [...],
  "contextProviders": [
    {
      "name": "smartlab",
      "params": {
        "server_url": "http://localhost:3000"
      }
    }
  ]
}
```

#### Claude API + AntiGravity
1. Get Claude API key from claude.ai
2. Configure AntiGravity:
   ```yaml
   integrations:
     claude:
       api_key: ${CLAUDE_API_KEY}
     smartlab:
       server_url: "http://localhost:3000"
   ```

### Step 3: Start Using

#### In Cursor Composer:
```
@smartlab execute backend dev workflow for PH-002

Expected response:
- Shows implementation plan
- Lists next steps
- Can execute code directly
```

#### In Continue Chat:
```
@smartlab phase 2 step 1 backend core dev PH-002

Expected response:
- Streaming workflow execution
- Task understanding output
- Ready to code
```

---

## 🏗️ Detailed Setup by IDE

### Cursor Setup (Recommended for Development)

**1. Install Extension**
```bash
cd ~/.cursor/extensions
git clone https://github.com/smartlab-dev/cursor-extension
npm install && npm build
```

**2. Configure Settings**
```json
// ~/.cursor/settings.json
{
  "extensions": {
    "smartlab.enabled": true,
    "smartlab.serverUrl": "http://localhost:3000",
    "smartlab.apiKey": "${SMARTLAB_API_KEY}",
    "smartlab.autoTrigger": true,
    "smartlab.streamOutput": true,
    "smartlab.showTimeline": true
  }
}
```

**3. Use in Composer**
```
@smartlab show workflow for 07_Backend_Core_Dev

Quick Reference:
- @smartlab phase N step M agent_id - Execute specific workflow
- @smartlab status PH-002 - Show current status
- @smartlab next-step - What should I do next?
- @smartlab escalate - Escalate current blocker
```

---

### Continue Setup (Best for Chat-Based Development)

**1. Install Plugin**
```bash
# Continue will auto-discover the plugin
# Just add to config.json:
```

**2. Configuration**
```json
// ~/.continue/config.json
{
  "contextProviders": [
    {
      "name": "smartlab-workflows",
      "serverUrl": "http://localhost:3000",
      "auth": "oauth2",
      "workspace": "SmartLab-V7"
    }
  ],
  "slashCommands": [
    {
      "name": "smartlab",
      "description": "Execute SmartLab agent workflows",
      "systemPrompt": "[See AGENT_WORKFLOW_MAPPING.md]"
    }
  ]
}
```

**3. Use in Chat**
```
/smartlab list agents

/smartlab execute 07_Backend_Core_Dev phase:2 step:1 task:PH-002

/smartlab status PH-002

/smartlab show-workflow 07_Backend_Core_Dev
```

---

### AntiGravity Setup (Google's AI IDE)

**1. Configure in Workspace**
```yaml
# .antigravity/config.yaml
integrations:
  smartlab:
    enabled: true
    server: "http://localhost:3000"
    workspace_id: "SmartLab-V7"
    
agents:
  enabled: true
  workflows:
    - path: "Docs/workflows"
      sync_interval: 300  # 5 minutes
```

**2. Use in Commands**
```
/smartlab execute 07_Backend_Core_Dev -phase 2 -step 1 -task PH-002

# Shows:
# - Task understanding analysis
# - Implementation plan
# - Next steps automatically proposed
```

---

### VS Code with Copilot Setup

**1. Install Extension**
```bash
code --install-extension smartlab.workflows
```

**2. Settings**
```json
{
  "[smartlab]": {
    "editor.defaultFormatter": "smartlab.workflows",
    "editor.formatOnSave": true
  },
  "smartlab.workflows": {
    "enabled": true,
    "mcp_server": "http://localhost:3000",
    "auto_trigger_on_save": true
  }
}
```

**3. Copilot Integration**
```
# In Copilot Chat:
@smartlab execute workflow for @selected_file

# Copilot will:
# - Identify task from filename
# - Get appropriate workflow
# - Execute with IDE context
```

---

## 🚀 Common Workflows by IDE

### Workflow 1: New Feature Request

**IDE**: Cursor Composer

```
Step 1: Create feature request
User: I need a pH monitoring API

Cursor: @smartlab create feature PH-002
System: Creates task, triggers PM workflow

Step 2: PM intake
PM Agent (01_PM_Tech_Lead) executes:
├─ Understand requirement
├─ Decompose into tasks
├─ Identify specialists needed
└─ Output: Backlog with 10 tasks

Step 3: Architecture design
Architects (02/03) execute in parallel:
├─ Design system
├─ Create API contracts
└─ Trigger specialist review

Step 4: Specialist review
Food Safety + Production + QMS:
├─ Review design
├─ Approve or flag issues
└─ All approve → Go to execution

Step 5: Create dev tasks
Cursor creates 10 tasks in editor:
- Task 1: Data model
- Task 2: Repository
- Task 3: API endpoints
- ...

All automatic! No manual coordination needed.
```

### Workflow 2: Implement Feature (Backend Developer)

**IDE**: Cursor

```
Cursor shows task sidebar:
Task: "pH API - Implement data model"
Assigned: You (Developer)

@smartlab execute 07_Backend_Core_Dev phase:2 step:1

System Prompt activates (from 07_Backend_Core_Dev.md):
├─ "Understand requirements completely"
├─ "Plan before coding"
└─ "Write tests (85% coverage)"

Workflow shows:
1. Task Understanding
   - What to build: pH data model
   - Constraints: Validate range 2.5-6.0
   - Tests needed: Validation, persistence
   
2. Implementation Plan
   - Create file: src/models/pH_Reading.ts
   - Create file: test/pH_Reading.test.ts
   - Estimate: 6 hours, 2 days

Cursor composer shows code template

Step 3: CODE
```javascript
// Cursor generates from template
class pH_Reading {
  batchId: string;
  reading: number;  // 0.0-14.0
  timestamp: Date;
  validationRules = {
    min: 2.5,
    max: 6.0
  };
  
  validate(): boolean {
    return this.reading >= 2.5 && this.reading <= 6.0;
  }
}
```

Step 4: TESTS
```javascript
describe("pH_Reading", () => {
  test("accepts valid reading", () => {
    const reading = new pH_Reading();
    reading.reading = 3.5;
    expect(reading.validate()).toBe(true);
  });
  
  test("rejects invalid reading", () => {
    const reading = new pH_Reading();
    reading.reading = 1.0;  // Too low
    expect(reading.validate()).toBe(false);
  });
});
```

Step 5: COMMIT
User: git commit -m "Implement pH data model with validation"

Auto-triggers:
├─ Tests run → All pass ✅
├─ Code review request → Architect notified
├─ Waits for: CODE_REVIEW_APPROVED
└─ Then: Auto-merge to main

All handled by workflow system!
```

### Workflow 3: Code Review (Architect)

**IDE**: Cursor / Continue

```
Architect gets notification:
"Backend_Core_Dev submitted PR for code review"

@smartlab show-workflow 02_Backend_Architecture_Lead phase:2 step:2

Workflow shows:
1. Get PR details
2. Review against architecture spec
3. Check:
   ✅ Follows API contract
   ✅ Uses correct design patterns
   ✅ Test coverage adequate
4. Decision:
   ✅ APPROVED → Auto-merge
   ⚠️ NEEDS_CHANGES → Post feedback
   ❌ REJECT → Explain why

Cursor shows:
- PR diff with highlights
- Architecture spec beside it
- Can leave inline comments

Submit decision:
APPROVED → Auto-triggers:
├─ Code merged to main
├─ Integration testing starts
└─ Next specialist review (if needed)
```

### Workflow 4: Quality Gates (QA Specialist)

**IDE**: Cursor

```
Status: Code merged to main
Trigger: QA_TESTING_REQUIRED

@smartlab execute 05_QA_Security_Specialist phase:2 step:1

Workflow:
1. Create test plan for pH API
2. Run automated tests
3. Run security scan
4. Run performance tests
5. Generate report

Cursor shows:
- Test results: 25/25 passed ✅
- Security scan: 0 issues ✅
- Performance: Response time 45ms (OK) ✅

Decision:
If all pass → VERIFICATION_COMPLETE
If fail → Notify developers + block deployment
```

### Workflow 5: Deployment (DevOps)

**IDE**: VS Code + Copilot

```
Status: All quality gates passed
Trigger: DEPLOYMENT_REQUIRED

Copilot: I can deploy pH API to production. Ready?

User: Yes, deploy

@smartlab execute 04_DevOps_Specialist phase:4 step:1

Workflow:
1. Create deployment plan
2. Deploy to staging
3. Run health checks
4. Deploy to production
5. Monitor for 30 minutes

VS Code shows:
- Deployment progress: ▓▓▓▓▓░░░░ 50%
- Health checks:
  ✅ API endpoint responding
  ✅ LIMS integration working
  ✅ Database queries fast
  
Deployment complete! ✅

Feature live in production!
```

---

## 📊 Workflow Execution Monitor

### In IDE Sidebar (Cursor/Continue)

```
SMARTLAB WORKFLOWS

Current Task: PH-002 pH API
Status: 🔄 EXECUTION IN PROGRESS

Timeline:
├─ ✅ PLANNING (2 days)
│  └─ PM Intake + Architecture + Specialists
├─ 🔄 EXECUTION (4 days)
│  ├─ 🔄 Backend Dev (60% complete)
│  ├─ ⏳ Frontend Dev (waiting)
│  ├─ ⏳ QA Testing (waiting)
│  └─ ⏳ Specialist Review (waiting)
├─ ⏳ VERIFICATION (2 days)
│  ├─ ⏳ Functionality Gate
│  ├─ ⏳ Security Gate
│  ├─ ⏳ Performance Gate
│  └─ ⏳ Compliance Gate
└─ ⏳ VALIDATION (1 day)
   ├─ ⏳ Specialist Approval
   ├─ ⏳ Architect Signoff
   ├─ ⏳ PM Approval
   └─ ⏳ Deployment

Next Step: Wait for Backend Dev to complete (Est. 2 days)

Blockers: None 🎉
```

### Chat-Based Status (Continue)

```
@smartlab status PH-002

SmartLab Workflow Status:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Task: pH Data Collection API
ID: PH-002
Status: EXECUTION_IN_PROGRESS

Current Agent: 07_Backend_Core_Dev
Current Phase: Phase 2 - Execution
Current Step: Step 2 - Implementation
Duration: 2 days 4 hours

Progress: ▓▓▓▓▓▓░░░░ 60%

What's Done:
✅ Planning complete
✅ Architecture designed
✅ Specialists approved
✅ Backend core dev: Data model complete

What's Next:
⏳ Backend core dev: API endpoints (today)
⏳ Frontend dev: Components (tomorrow)
⏳ QA: Integration testing
⏳ Verification: Quality gates

Timeline: On schedule ✅

Want to:
- See implementation plan?
- View architecture spec?
- Check specialist feedback?
- Monitor in real-time?
```

---

## 🔌 API-Based Access (Programmatic)

### Python Example

```python
from smartlab_workflows import WorkflowClient

# Initialize client
client = WorkflowClient(
    server_url="http://localhost:3000",
    api_key="sk_smartlab_..."
)

# Get workflow for agent
workflow = client.get_workflow(
    agent_id="07_Backend_Core_Dev",
    format="executable"
)

# Execute workflow phase
result = client.execute_workflow(
    agent_id="07_Backend_Core_Dev",
    phase=2,
    step=1,
    input_data={
        "task_id": "PH-002",
        "task_title": "pH API Implementation",
        "architecture_spec": architecture_spec
    }
)

# Get results
print(result.status)  # "COMPLETED"
print(result.output.implementation_plan)

# Get next trigger
print(result.next_trigger)  # "TESTS_RUN"

# Subscribe to updates
def on_progress(status):
    print(f"Progress: {status.percent}%")

client.subscribe_to_workflow(
    execution_id=result.execution_id,
    on_progress=on_progress,
    on_complete=lambda r: print("Done!")
)
```

### Node.js Example

```javascript
const smartlab = require('smartlab-workflows');

const client = new smartlab.WorkflowClient({
  serverUrl: 'http://localhost:3000',
  apiKey: 'sk_smartlab_...'
});

// Execute workflow
const result = await client.executeWorkflow({
  agentId: '07_Backend_Core_Dev',
  phase: 2,
  step: 1,
  inputData: {
    taskId: 'PH-002',
    taskTitle: 'pH API Implementation'
  }
});

console.log('Status:', result.status);
console.log('Plan:', result.output.implementationPlan);
console.log('Next:', result.nextTrigger);
```

---

## 🛠️ Troubleshooting

### MCP Server Not Responding

```bash
# Check if server is running
curl http://localhost:3000/health

# If not running, start it:
# Docker
docker logs smartlab-mcp-server

# Node.js
npm logs smartlab-mcp-server

# Check port
lsof -i :3000
```

### Workflow Not Triggering

**Check**:
1. Previous phase completed? `@smartlab status TASK_ID`
2. Event fired? Check logs: `tail -f smartlab.log`
3. Agent available? `@smartlab list-agents`

**Fix**:
```
@smartlab trigger-manually 07_Backend_Core_Dev phase:2 step:1
```

### IDE Not Finding Workflows

**Cursor**: 
- Restart IDE: `Cmd+Shift+P` → "Reload Window"
- Check extension: Settings → Extensions → SmartLab

**Continue**:
- Verify config.json has smartlab section
- Restart Continue server

**AntiGravity**:
- Run: `antigravity sync-workflows`
- Check: `antigravity workflows list`

---

## 📚 Full Documentation

- **MASTER_WORKFLOW.md** - Complete workflow system
- **AUTO_TRIGGERS.yaml** - All 30+ triggers
- **WORKFLOWS_INDEX.md** - All 19 agents
- **WORKFLOW_EXECUTION_SCHEMA.md** - How to invoke workflows
- **AGENT_WORKFLOW_MAPPING.md** - System prompts ↔ Workflows
- **QUICK_START_GUIDE.md** - Getting started (10 min)

---

## ✨ Best Practices

### For Fastest Development:
```
1. Use Cursor Composer for feature planning
2. Let system execute workflows automatically
3. Focus on coding, not coordination
4. Let IDE handle all triggering
5. Check status dashboard for progress
```

### For Best Quality:
```
1. Make sure architects understand requirements
2. Have specialists review early
3. Run all quality gates before deployment
4. Monitor post-deployment for 24 hours
5. Document lessons learned
```

### For Smooth Collaboration:
```
1. Keep tasks small (1-2 days each)
2. Write clear commit messages
3. Respond to code reviews within 24 hours
4. Communicate blockers immediately
5. Celebrate successful deployments
```

---

**SmartLab IDE Integration** is production-ready!  
Questions? Ask in #smartlab-workflows Slack channel.

**Version**: 1.0  
**Last Updated**: 2026-01-30  
**Status**: Ready for Integration

# WORKFLOW SYSTEM - INTEGRATION GUIDE

**Version**: 1.0  
**Purpose**: Connect SmartLab Workflow System with external tools and systems  
**Last Updated**: 2026-01-30

---

## 📌 Overview

The SmartLab Workflow System is **event-driven** and can integrate with:
- **Issue Tracking**: Jira, Azure DevOps, GitHub Issues
- **Code Repositories**: GitHub, GitLab, Bitbucket
- **CI/CD Pipeline**: GitHub Actions, Jenkins, GitLab CI
- **Communication**: Slack, Microsoft Teams, Email
- **Monitoring**: Datadog, New Relic, CloudWatch
- **Automation**: Zapier, IFTTT, custom webhooks

---

## 🔌 Integration Architecture

```
External Systems → Event Webhooks → Workflow Engine → Agent Assignment → Outputs → External Systems
```

### Flow Example:
```
1. Developer pushes code to GitHub
   ↓ (GitHub sends webhook)
2. Workflow Engine receives: CODE_PUSHED event
   ↓ (Looks up corresponding trigger in AUTO_TRIGGERS.yaml)
3. Trigger: trigger.execution.code_push
   ↓ (Executes actions)
4. Actions:
   - Run: linting + unit tests + security scan
   - Create: Pull Request
   - Assign: Code review to Backend_Architecture_Lead
   - Notify: Developer + Architecture Lead
   ↓ (Send results back to GitHub)
5. GitHub PR shows:
   - Automated tests: ✅ PASSED
   - Code quality: ✅ CLEAN
   - Security: ✅ PASS
   - Waiting for: Code Review
```

---

## 🛠️ Integration Implementation

### Option 1: Workflow Engine Integration (Recommended)

**Technology Stack**:
- **Workflow Engine**: n8n, Zapier, or custom Node.js service
- **Event Bus**: Redis, RabbitMQ, or Kafka
- **Database**: PostgreSQL (task status, workflow state)
- **API**: REST API for webhook handling

**Implementation Steps**:

**Step 1: Set up Workflow Engine**
```
Install n8n or self-hosted workflow automation tool
├─ Configure webhook endpoint
├─ Connect to issue tracker (Jira/GitHub)
├─ Connect to CI/CD pipeline
├─ Connect to communication tools
└─ Import AUTO_TRIGGERS.yaml workflows
```

**Step 2: Configure Webhooks**
```
GitHub Webhooks:
├─ push → Send CODE_PUSHED event
├─ pull_request → Send CODE_REVIEW_REQUESTED event
├─ pull_request.synchronize → Send CODE_PUSHED event
└─ release → Send DEPLOYMENT_COMPLETE event

Jira Webhooks:
├─ issue.created → Send TASK_CREATED event
├─ issue.updated → Send TASK_STATUS_CHANGED event
└─ issue.transitioned → Send PHASE_COMPLETE event
```

**Step 3: Create Workflow Automation**
```
For each trigger in AUTO_TRIGGERS.yaml:
1. Create workflow rule in engine
2. Map conditions to event fields
3. Define actions (create issue, send notification, etc)
4. Set timeouts and escalation
5. Test with real events
```

**Example Workflow Rule** (n8n format):
```yaml
name: "CODE_PUSHED Auto-Test"
trigger:
  type: "webhook"
  events: ["github.push"]
  
conditions:
  - event.branch == "main"
  - event.files includes ".ts" or ".tsx"

actions:
  - run: "npm run lint"
  - run: "npm run test"
  - run: "npm run security-scan"
  
  - if: tests_passed AND security_clean
    then:
      - create_github_pr_review_request:
          reviewer: "02_Backend_Architecture_Lead"
          comment: "Automated checks passed. Ready for review."
  
  - if: tests_failed
    then:
      - post_github_comment: "Tests failed. See details above."
      - notify_slack_dev_channel: "❌ Tests failed for PR #123"
      - update_jira_task: status = "BLOCKED_BY_TESTS"

timeout: "15 minutes"
escalation:
  - after_timeout: "notify_architecture_lead"
```

### Option 2: Custom API Integration

**Build Custom Workflow Service**:
```javascript
// Example: Node.js workflow service

const express = require('express');
const app = express();

// Receive webhook from GitHub
app.post('/webhook/github', async (req, res) => {
  const event = req.body;
  
  if (event.action === 'opened' && event.pull_request) {
    // PR created
    const trigger = triggers['trigger.execution.code_review_request'];
    
    // Run automated checks
    const testResults = await runTests(event.pull_request.head.sha);
    const securityScan = await scanSecurity(event.pull_request.head.sha);
    
    if (testResults.passed && securityScan.clean) {
      // Request code review
      await assignCodeReview(
        event.pull_request.number,
        '02_Backend_Architecture_Lead'
      );
      
      // Notify
      await notifySlack(`PR #${event.number} ready for review`);
    }
  }
  
  res.json({ status: 'processed' });
});

// Receive webhook from Jira
app.post('/webhook/jira', async (req, res) => {
  const event = req.body;
  
  if (event.changelog.items[0].field === 'status' 
      && event.changelog.items[0].toString === 'PLANNING_COMPLETE') {
    
    // Phase transition detected
    const trigger = triggers['trigger.planning.complete'];
    
    // Activate execution phase
    await activatePhase('EXECUTION', event.issue.key);
    
    // Create tasks
    const backlog = await generateBacklog(event.issue);
    for (const task of backlog) {
      await createJiraTask(task);
    }
  }
  
  res.json({ status: 'processed' });
});

app.listen(3000);
```

### Option 3: Zapier Integration (No-Code)

**Setup Steps**:
1. Create Zapier account
2. Connect apps:
   - GitHub
   - Jira
   - Slack
   - Google Sheets (for backlog tracking)
3. Create Zaps for each trigger:
   - "When GitHub PR created, run tests in GitHub Actions"
   - "When Jira issue status changes to PLANNING_COMPLETE, create child tasks"
   - "When GitHub release published, post to Slack"

**Example Zap**:
```
Trigger: GitHub - New Pull Request
├─ Wait 5 minutes (for CI/CD to run)
├─ GitHub - Get PR details
├─ Filter: If all checks passed
└─ Action: Slack - Send message
   "✅ PR #123 ready for code review"
```

---

## 🔄 Event Mapping

### GitHub Events → Workflow Events

| GitHub Event | Workflow Event | Trigger File |
|---|---|---|
| push (main) | CODE_PUSHED | trigger.execution.code_push |
| pull_request.opened | CODE_REVIEW_REQUESTED | trigger.execution.code_review_request |
| pull_request.synchronize | CODE_PUSHED_UPDATE | trigger.execution.code_push |
| pull_request.approved | CODE_REVIEW_APPROVED | trigger.execution.merge_ready |
| pull_request.merged | CODE_MERGED_TO_MAIN | trigger.execution.merge_ready |
| release.published | DEPLOYMENT_REQUIRED | (manual trigger) |

### Jira Events → Workflow Events

| Jira Event | Workflow Event | Trigger File |
|---|---|---|
| issue.created | TASK_CREATED | trigger.planning.intake |
| issue.transitioned (→ IN_PROGRESS) | TASK_STARTED | trigger.execution.* |
| issue.transitioned (→ READY_FOR_REVIEW) | TASK_READY_FOR_REVIEW | trigger.execution.code_review_request |
| issue.transitioned (→ DONE) | TASK_COMPLETE | (automatic) |
| comment.created (with "@mention") | TASK_COMMENT_NOTIFICATION | (manual) |

---

## 📊 Data Models

### Task/Issue Schema

```yaml
Task:
  id: "PH-002"
  title: "pH Data Collection API"
  description: "..."
  
  workflow:
    phase: "EXECUTION"          # PLANNING, EXECUTION, VERIFICATION, VALIDATION
    status: "IN_PROGRESS"        # CREATED, PLANNED, STARTED, IN_REVIEW, BLOCKED, COMPLETE
    assigned_agent: "07_Backend_Core_Dev"
    
  timeline:
    created_at: "2026-02-04T10:00:00Z"
    target_completion: "2026-02-07"
    
  requirements:
    specialists_needed: ["02_Food_Safety_Manager", "05_Production_Manager"]
    code_review_by: "02_Backend_Architecture_Lead"
    deployment_by: "04_DevOps_Specialist"
  
  quality_gates:
    functionality: PENDING
    security: PENDING
    performance: PENDING
    compliance: PENDING
  
  specialists_approval:
    food_safety: PENDING
    qms: PENDING
    production: PENDING
    regulatory: PENDING
```

### Trigger Execution Log

```yaml
TriggerExecution:
  id: "trigger_exec_001"
  trigger_id: "trigger.execution.code_push"
  task_id: "PH-002"
  
  event:
    type: "CODE_PUSHED"
    timestamp: "2026-02-05T14:30:00Z"
    source: "GitHub"
    data:
      commit: "abc123def456"
      branch: "main"
      files: ["src/api.ts", "test/api.test.ts"]
  
  execution:
    started_at: "2026-02-05T14:30:01Z"
    completed_at: "2026-02-05T14:35:22Z"
    
    actions:
      - action: "run_linting"
        status: "PASSED"
        result: "0 issues"
      
      - action: "run_tests"
        status: "PASSED"
        result: "25 tests passed"
      
      - action: "security_scan"
        status: "PASSED"
        result: "0 vulnerabilities"
      
      - action: "request_code_review"
        status: "COMPLETED"
        assigned_to: "02_Backend_Architecture_Lead"
  
  next_trigger: "trigger.execution.code_review_request"
  next_trigger_when: "When code_review is APPROVED"
```

---

## 🔐 Security Considerations

### Authentication & Authorization

```yaml
Webhook Security:
  - Validate webhook signatures (GitHub secret, Jira token)
  - HTTPS only, no HTTP
  - IP whitelist for webhook sources
  - Rate limiting (max 100 requests/minute per source)

API Authentication:
  - Use OAuth 2.0 for external integrations
  - API keys for internal services
  - Rotate keys every 90 days
  
Service Accounts:
  - Create service account per integration (GitHub, Jira, Slack)
  - Minimal required permissions only
  - Audit all actions taken by service accounts
  - Alert on unusual activity
```

### Data Privacy

```yaml
Data Handling:
  - Never store credentials in code (use environment variables)
  - Encrypt sensitive data at rest
  - Encrypt in transit (TLS 1.3+)
  - Mask passwords/tokens in logs
  - Delete old workflow logs after 90 days
  - GDPR compliance for EU customers
```

---

## 📝 Setup Checklist

### Phase 1: Foundation
- [ ] Install workflow engine (n8n or similar)
- [ ] Create GitHub app/bot account
- [ ] Create Jira service account
- [ ] Create Slack app for notifications
- [ ] Set up database for task tracking

### Phase 2: Webhooks
- [ ] Configure GitHub webhooks
  - [ ] push
  - [ ] pull_request
  - [ ] release
- [ ] Configure Jira webhooks
  - [ ] issue.created
  - [ ] issue.transitioned
- [ ] Validate webhook delivery (test with POST)

### Phase 3: Workflows
- [ ] Import AUTO_TRIGGERS.yaml into workflow engine
- [ ] Create workflow rule for each trigger (30+ rules)
- [ ] Test each workflow with sample events
- [ ] Configure notifications (Slack, email)
- [ ] Set up error handling & retries

### Phase 4: Integrations
- [ ] Connect to code repository
- [ ] Connect to issue tracker
- [ ] Connect to CI/CD pipeline
- [ ] Connect to communication tools
- [ ] Connect to monitoring tools

### Phase 5: Testing & Deployment
- [ ] End-to-end test: Request → Completion
- [ ] Test escalation paths (delays, blockers)
- [ ] Test error handling
- [ ] Load test (handle spike in requests)
- [ ] Monitor for 1 week in production
- [ ] Adjust based on real data

---

## 🚀 Example: Complete Integration Setup

### Scenario: Feature Request → Deployment

**Initial State**:
- Feature request submitted via Jira
- Creates issue with type = "Feature"
- Status = "NEW_FEATURE"

**Auto-Execution**:

```
1. NEW_FEATURE Created (Jira)
   ↓ Webhook sent to workflow engine
   ↓ Trigger: trigger.planning.intake
   
2. PM_Tech_Lead Assigned (auto)
   ↓ Task status → "AWAITING_PM_INTAKE"
   ↓ Slack notification: "PM: New feature ready for intake"
   
3. PM Reviews & Decomposes (manually)
   ↓ PM updates Jira: Status → "PLANNING_IN_PROGRESS"
   ↓ Creates 7-10 subtasks
   ↓ Identifies specialists needed
   
4. Decomposition Complete (Jira)
   ↓ Webhook sent
   ↓ Trigger: trigger.planning.to_architects
   
5. Architects Assigned (auto)
   ↓ Both Architecture Leads assigned
   ↓ Slack: "Architects: Design phase started"
   ↓ GitHub: Create "design" branch
   ↓ Task status → "DESIGN_IN_PROGRESS"
   
6. Architecture Design Complete (manual)
   ↓ Architects update Jira: Status → "DESIGN_COMPLETE"
   ↓ Webhook sent
   ↓ Trigger: trigger.planning.to_specialists
   
7. Specialists Triggered (auto)
   ↓ Food Safety Manager, Production Manager, QMS assigned
   ↓ Jira: Create specialist review subtasks
   ↓ Slack notifications sent
   ↓ Task status → "SPECIALIST_REVIEW_IN_PROGRESS"
   
8. Specialist Reviews (manual)
   ↓ Food Safety Manager reviews → APPROVED/REJECTED
   ↓ Updates Jira: Food Safety comment + approval
   ↓ Webhook sent
   ↓ Production Manager reviews → APPROVED/REJECTED
   ↓ Updates Jira: Production comment + approval
   ↓ Workflow engine monitors all specialists
   
9. All Specialists Approved (auto)
   ↓ Trigger: trigger.planning.generate_backlog
   ↓ PM creates backlog with tasks
   ↓ Webhook sent
   
10. Backlog Approved (auto)
    ↓ Trigger: PLANNING_PHASE_COMPLETE
    ↓ Task status → "READY_FOR_EXECUTION"
    ↓ Slack: "Execution phase starts"
    
11. Developers Assigned (auto)
    ↓ Backend/Frontend developers assigned tasks
    ↓ GitHub: Create feature branches
    ↓ Jira: Task status → "DEVELOPMENT_IN_PROGRESS"
    ↓ Slack notifications sent
    
12. Code Development (manual)
    ↓ Developer commits code
    ↓ GitHub webhook sent: push event
    ↓ Trigger: trigger.execution.code_push
    
13. Automated Tests Run (auto)
    ↓ GitHub Actions runs tests
    ↓ Results posted to PR
    ↓ Trigger: trigger.execution.code_review_request
    ↓ Architecture lead assigned for review
    ↓ Slack: "Review ready for: Backend_Architecture_Lead"
    
14. Code Review (manual)
    ↓ Architect reviews code
    ↓ GitHub: Approves PR
    ↓ Webhook sent: pull_request.approved
    ↓ Trigger: trigger.execution.merge_ready
    
15. Auto-Merge (auto)
    ↓ GitHub: Auto-merges to main
    ↓ Webhook sent: pull_request.merged
    ↓ Trigger: trigger.execution.integration_start
    
16. Integration Testing (auto)
    ↓ GitHub Actions: Runs integration tests
    ↓ Results posted
    ↓ If ALL PASS → Trigger: trigger.execution.specialist_review_check
    
17. Specialist Review (auto)
    ↓ Check if specialist review needed
    ↓ If food safety feature → assign Food Safety Manager
    ↓ Jira: Create specialist review task
    ↓ Task status → "SPECIALIST_REVIEW_IN_PROGRESS"
    
18. Specialist Final Review (manual)
    ↓ Food Safety Manager reviews integrated code
    ↓ Runs manual tests in staging
    ↓ Updates Jira: APPROVED
    ↓ Webhook sent
    ↓ Trigger: trigger.execution.architecture_review
    
19. Architecture Review (auto/manual)
    ↓ Architecture Lead reviews final code
    ↓ Confirms design compliance
    ↓ Updates Jira: APPROVED
    ↓ Webhook sent
    ↓ Trigger: EXECUTION_PHASE_COMPLETE
    
20. Verification Phase Starts (auto)
    ↓ Task status → "VERIFICATION_IN_PROGRESS"
    ↓ Run all 4 quality gates in parallel
    
21. Quality Gates (auto)
    ↓ Gate 1 (Functionality): Run test suite → PASS
    ↓ Gate 2 (Security): Security scan → PASS
    ↓ Gate 3 (Performance): Load test → PASS
    ↓ Gate 4 (Compliance): Specialist approval → PASS
    ↓ All PASS → Trigger: trigger.verification.all_pass
    
22. Validation Phase (auto)
    ↓ Task status → "VALIDATION_IN_PROGRESS"
    ↓ Final approvals collected:
    ↓   - Food Safety → Approved
    ↓   - Architecture → Approved
    ↓   - PM → Approved
    
23. Deployment Ready (auto)
    ↓ Task status → "READY_FOR_DEPLOYMENT"
    ↓ Trigger: trigger.validation.pm_approval
    ↓ DevOps Specialist notified
    
24. Deployment (manual/auto)
    ↓ DevOps Specialist deploys to production
    ↓ Runs health checks
    ↓ Monitors for 30 minutes
    ↓ Updates Jira: DEPLOYED
    ↓ Webhook sent: DEPLOYMENT_COMPLETE
    
25. Post-Deployment Monitoring (auto)
    ↓ System monitors for 24 hours
    ↓ Tracks error rates, latency, business metrics
    ↓ If issues → creates incident
    ↓ Updates Jira: Task COMPLETED or ISSUE_FOUND
    
26. Feature Complete (auto)
    ↓ Slack: "Feature deployed successfully"
    ↓ Update backlog: Task → DONE
    ↓ Lessons learned meeting scheduled
    
Total Time: 10-14 days (including reviews)
Manual Effort: ~5% (mostly reviews and approvals)
Automated: ~95% (all triggering, testing, validation)
```

---

## 📊 Monitoring & Alerts

### Key Metrics to Track

```yaml
workflow_metrics:
  planning_duration:
    target: "3-5 days"
    alert_threshold: "> 7 days"
    action: "Escalate to PM"
  
  execution_duration:
    target: "1-4 weeks"
    alert_threshold: "> 6 weeks"
    action: "Review blockers, reassign resources"
  
  code_review_cycle_time:
    target: "< 24 hours"
    alert_threshold: "> 48 hours"
    action: "Escalate to Architecture Lead"
  
  quality_gate_pass_rate:
    target: "100%"
    alert_threshold: "< 95%"
    action: "Investigate failures, improve QA"
  
  specialist_approval_rate:
    target: "100%"
    alert_threshold: "Any rejection"
    action: "Discuss, resolve disagreement"
  
  deployment_success_rate:
    target: "100%"
    alert_threshold: "Any failure"
    action: "Incident response, post-mortem"
```

### Monitoring Dashboard

Create dashboard in your monitoring tool (Grafana, DataDog, etc):
```
Row 1: Phase Metrics
├─ Planning avg duration
├─ Execution avg duration
├─ Verification avg duration
└─ Validation avg duration

Row 2: Quality Metrics
├─ Quality gate pass rate (by gate)
├─ Specialist approval rate
├─ Code review cycle time
└─ Test pass rate

Row 3: Throughput
├─ Features completed per week
├─ Tasks completed per week
├─ Deployment frequency
└─ Lead time for changes

Row 4: Reliability
├─ Deployment success rate
├─ Rollback rate
├─ Post-deployment issue rate
└─ Uptime (post-deployment)
```

---

## 🆘 Troubleshooting

### Common Issues

**Issue**: Webhooks not received
- **Check**: Webhook URL is correct and accessible
- **Check**: Webhook signature validation not failing
- **Check**: Firewall/IP whitelist allows source
- **Action**: Re-register webhook, test with curl

**Issue**: Trigger not firing
- **Check**: Event conditions match in AUTO_TRIGGERS.yaml
- **Check**: Workflow engine is running
- **Check**: Task status correct before trigger condition
- **Action**: Review trigger logs, check workflow engine logs

**Issue**: Agent not notified of task
- **Check**: Slack/email integration configured
- **Check**: Notification settings enabled for agent
- **Check**: Agent has permission to access task
- **Action**: Test notification manually, check integration logs

**Issue**: Data out of sync (Jira vs GitHub vs system)
- **Check**: Webhook handlers validate data before update
- **Check**: No concurrent updates causing race conditions
- **Check**: Database transaction handling correct
- **Action**: Implement idempotency, add retry logic

---

## 📚 References

- [n8n Documentation](https://docs.n8n.io)
- [GitHub Webhooks Guide](https://docs.github.com/webhooks)
- [Jira Automation Rules](https://support.atlassian.com/jira-cloud-automation)
- [Zapier Integration Guide](https://zapier.com/developers)

---

**Integration Guide Version**: 1.0  
**Last Updated**: 2026-01-30  
**Status**: Ready for Implementation

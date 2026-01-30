# SMARTLAB WORKFLOW SYSTEM - QUICK START GUIDE

**Version**: 1.0  
**Target Audience**: PM, Developers, Architects, Specialists  
**Time to Read**: 10 minutes  
**Last Updated**: 2026-01-30

---

## 🎯 What is the Workflow System?

The SmartLab Workflow System **automatically orchestrates** how features move from request to production:

```
FEATURE REQUEST
    ↓
PLANNING (PM + Architects + Specialists design together)
    ↓
EXECUTION (Developers code, QA tests, Specialists review)
    ↓
VERIFICATION (4 quality gates check everything)
    ↓
VALIDATION (Final approvals from all stakeholders)
    ↓
DEPLOYMENT (Goes to production automatically)
    ↓
MONITORING (System watches for issues)
```

**Key Benefit**: No manual coordination needed. Agents are automatically triggered when they're needed.

---

## 👥 Who Does What?

### Project Manager (PM)
- **When**: Feature request comes in
- **Does**: Understands requirements, breaks into tasks, identifies specialists
- **Time**: 2 hours initial + ongoing monitoring
- **Success**: Clear backlog that developers can execute

### Architects (Backend + Frontend)
- **When**: PM completes intake
- **Does**: Design system, create API contracts, identify risks
- **Time**: 2-3 days
- **Success**: Design spec that developers can follow

### Specialists (Food Safety, Production, QMS, etc)
- **When**: Architecture complete
- **Does**: Review design, validate it meets their requirements
- **Time**: 1 day each (parallel)
- **Success**: Approval that design is feasible + safe

### Developers (Backend + Frontend)
- **When**: Backlog tasks assigned
- **Does**: Code, test, get review, commit to main
- **Time**: 1-4 days per task
- **Success**: Code merged to main, all tests passing

### QA Security Specialist
- **When**: Code committed to main
- **Does**: Run full test suite, security scan, validate quality
- **Time**: 1-2 days
- **Success**: All tests pass, no security issues

### DevOps Specialist
- **When**: All verification complete
- **Does**: Deploy to production, monitor health
- **Time**: 2 hours deploy + 24 hours monitoring
- **Success**: Feature live and stable in production

---

## 📋 How to Submit a Feature Request

### Step 1: Create Issue in Jira
```
Title: "pH Data Collection API"

Description:
- What problem does this solve?
- Who benefits? (operators, QC, compliance)
- What's the scope?
- Any known constraints?

Labels: [feature]
```

### Step 2: System Automatically Triggers
```
✓ PM notified in Slack
✓ Task created in workflow system
✓ Status set to AWAITING_PM_INTAKE
```

### Step 3: Monitor Progress
```
Go to Jira issue
├─ Watch status changes as phases complete
├─ See which agents are working on it
├─ View timeline (expected completion dates)
└─ Read agent notes/decisions
```

---

## 🔄 Example: Feature from Request to Production

**Day 1-2: Planning Phase**

```
Monday 10 AM: You create issue "pH Data Collection API"
├─ PM receives notification
├─ PM understands requirements
├─ PM identifies: "Affects fermentation, so need Food Safety + Production review"
├─ PM decomposes into 7 tasks
├─ Slack notification: "Planning complete, ready for architects"

Monday 2 PM: Architects start design
├─ Backend Architect starts API design
├─ Frontend Architect starts component design
├─ Both work in parallel
├─ They use GitHub for design documents

Wednesday 10 AM: Architects complete design
├─ Slack: "Design ready for specialist review"

Wednesday 12 PM: Specialists review
├─ Food Safety Manager: "This protects fermentation control ✓ APPROVED"
├─ Production Manager: "Operators can use this ✓ APPROVED"
├─ QMS Specialist: "Requires procedure update - APPROVED"

Thursday 10 AM: PM approves backlog
├─ Creates 10 tasks for developers
├─ Each task assigned to specific developer
├─ Slack: "Execution phase starts, devs have assignments"
```

**Day 3-7: Execution Phase**

```
Thursday 2 PM: Developers start coding
├─ Backend developer: Database model → API endpoints
├─ Frontend developer: Components → integration
├─ Each pushes code daily

Friday 3 PM: First code merged
├─ Developer pushes code
├─ GitHub Actions runs tests automatically
├─ Tests PASS ✓
├─ Architecture Lead does code review
├─ Code auto-merges to main
├─ GitHub creates release candidate

Monday 10 AM: Integration testing
├─ QA runs full test suite
├─ Security scan runs
├─ Both PASS ✓
├─ Specialist review triggered

Monday 4 PM: Specialist reviews integrated code
├─ Food Safety Manager tests pH monitoring in staging
├─ Confirms real-time alerts work correctly
├─ APPROVED ✓

Tuesday 9 AM: Architecture final review
├─ Backend Architect confirms design compliance
├─ APPROVED ✓
├─ Slack: "Ready for quality gates"
```

**Day 8-9: Verification & Validation**

```
Tuesday 10 AM: Quality gates run automatically
├─ Gate 1 (Functionality): Tests PASS ✓
├─ Gate 2 (Security): No vulnerabilities ✓
├─ Gate 3 (Performance): Fast enough ✓
├─ Gate 4 (Compliance): All specialists APPROVED ✓
├─ Slack: "All gates passed, ready for deployment"

Tuesday 2 PM: Final approvals collected
├─ Food Safety: APPROVED ✓
├─ Architecture: APPROVED ✓
├─ PM: APPROVED ✓
├─ Slack: "Deployment authorized"

Wednesday 9 AM: DevOps deploys to production
├─ Code deployed to production
├─ Health checks pass
├─ System monitoring activated
├─ Feature live! ✓
├─ Slack: "pH Data Collection API live in production"

Wednesday 10 AM - Thursday 10 AM: Monitoring
├─ System watches for errors
├─ Tracks pH readings accuracy
├─ Monitors alert system
├─ All good ✓
```

**Total Time**: 8 days (including 2 days for reviews)

---

## ⚡ Your Action Items by Role

### If You're a Project Manager
```
When feature request comes in:
1. Go to Jira, find the issue
2. Read the description
3. Ask clarifying questions (add comments)
4. Break down into 7-10 small tasks
5. Set target dates for each phase
6. System automatically triggers everything else
```

### If You're a Developer
```
When a task is assigned to you:
1. Go to Jira, find your task
2. Read the requirements + architecture spec
3. Create a feature branch
4. Code + write tests
5. Push code daily
6. When ready, submit for code review
7. System automatically triggers review + testing
```

### If You're an Architect
```
When planning phase triggered:
1. Go to Jira, find the epic
2. Read PM's decomposition
3. Design the system (3 days)
4. Create design document + API contracts
5. Mark as DESIGN_COMPLETE
6. System automatically triggers specialist review
```

### If You're a Specialist
```
When specialist review triggered:
1. Go to Jira, find your review task
2. Read the design document
3. Ask questions if unclear (add comments)
4. Verify it meets your requirements
5. Approve or reject (explain why)
6. System aggregates all approvals
```

### If You're QA
```
When code ready for testing:
1. System automatically triggers
2. Read the acceptance criteria
3. Run the test plan
4. Log any bugs you find
5. System notifies developers
6. Developers fix, you re-test
7. When all pass, move to deployment
```

### If You're DevOps
```
When deployment triggered:
1. Go to Jira, find the deployment task
2. Review the release notes
3. Run deployment scripts
4. Verify health checks pass
5. Monitor for 24 hours
6. If issue found, rollback
7. If stable, mark as DEPLOYED
```

---

## 🔔 Key Notifications You'll Get

### Slack Notifications
```
@pm "New feature ready for intake: pH Data Collection API"
@architects "Design phase started. Due: Thursday 5 PM"
@food_safety_manager "Design ready for review. Due: Friday 5 PM"
@developers "Backlog ready. 10 tasks assigned. Start: Thursday"
@all "Feature deployed to production! pH API live"
```

### Email Notifications
```
Subject: "Your task 'Implement API endpoints' is assigned"
Subject: "Code review requested for PR #123"
Subject: "Feature deployment scheduled for Wednesday 9 AM"
Subject: "Production issue detected in pH API"
```

### Jira Notifications
```
Task status changed: PLANNING → EXECUTION
Task assigned: "Implement API endpoints" → You
Comment added: "Architecture Lead approved your code"
Timeline: Expected completion moved from Feb 15 → Feb 10
Blocker: "Tests failing - see details"
```

---

## 📊 How to Track Progress

### Option 1: Jira Board
```
1. Go to Jira
2. Find the Epic/Feature
3. View all related tasks
4. See which phase you're in
5. See who's working on what
6. See blockers (if any)
```

### Option 2: Slack Updates
```
#smartlab-deployments channel:
├─ Daily standup (9 AM)
├─ Status updates (hourly during execution)
├─ Blocker alerts (immediate)
└─ Deployment notifications (real-time)
```

### Option 3: Dashboard
```
1. Go to workflow dashboard
2. Filter by your feature
3. See timeline with all phases
4. See completion % for each phase
5. See which agent doing what
6. See quality gate status
```

---

## 🚨 What If Something Goes Wrong?

### Blocker Found During Development
```
Developer finds issue: "Can't connect to LIMS"
├─ Posts in Slack: "Blocker: LIMS connection"
├─ Creates Jira issue: Type = "Blocker"
├─ System notifies: PM + Architecture Lead
├─ PM assesses impact: "Can delay 1 day"
├─ Architecture Lead helps troubleshoot
├─ Issue fixed, development resumes
```

### Quality Gate Fails
```
Security scan finds vulnerability
├─ Gate blocks deployment
├─ Slack notification to all approvers
├─ Developer fixes the vulnerability
├─ QA re-runs security scan
├─ If pass, deployment proceeds
├─ If fail, cycle repeats
```

### Specialist Disagrees
```
Food Safety Manager: "Can't approve - not safe"
Production Manager: "Actually, this is fine"
├─ System alerts: "Specialist disagreement"
├─ Domain Expert Coordinator steps in
├─ Facilitates discussion
├─ If can't agree, escalates to PM
├─ PM makes final decision
```

### Deployment Fails
```
DevOps starts deployment
├─ Health checks FAIL
├─ System detects issue
├─ Triggers rollback automatically
├─ Previous version restored
├─ Incident created
├─ Team discusses what went wrong
├─ Fix applied
├─ Deployment attempted again
```

---

## ✅ Checklist: Is My Feature Ready?

### For Project Managers
- [ ] Issue created with clear description
- [ ] Requirements documented
- [ ] Success criteria defined
- [ ] Specialists identified
- [ ] Estimated effort provided
- [ ] Timeline communicated

### For Architects
- [ ] Design document created
- [ ] API contracts specified
- [ ] Data model designed
- [ ] Risk assessment done
- [ ] Implementation spec ready
- [ ] Design review scheduled

### For Developers
- [ ] Code written with tests
- [ ] Unit tests all pass
- [ ] Code self-reviewed
- [ ] Code review requested
- [ ] Feedback addressed
- [ ] Ready to merge

### For QA
- [ ] Test plan created
- [ ] All tests pass
- [ ] No critical bugs
- [ ] Security scan clean
- [ ] Performance acceptable
- [ ] Ready for deployment

### For Deployment
- [ ] All approvals obtained
- [ ] Deployment plan documented
- [ ] Rollback plan ready
- [ ] Monitoring dashboard set up
- [ ] Team trained
- [ ] Ready to deploy

---

## 📞 Need Help?

### Where to Find Information

| Question | Answer Location |
|----------|-----------------|
| "How do workflows work?" | MASTER_WORKFLOW.md |
| "What's PM supposed to do?" | 01_PM_Tech_Lead_WORKFLOW.md |
| "How do code reviews work?" | 07_Backend_Core_Dev_WORKFLOW.md |
| "What does specialist review?" | specialists_02_Food_Safety_Manager_WORKFLOW.md |
| "How to integrate with Jira?" | INTEGRATION_GUIDE.md |
| "What are quality gates?" | QUALITY_GATES.md |

### Who to Ask

| Question | Ask |
|----------|-----|
| "Is my feature request clear?" | Project Manager |
| "Does my design work?" | Architecture Lead |
| "Is my code good?" | Peer developer + Architecture Lead |
| "Does this meet food safety?" | Food Safety Manager |
| "Can production operate this?" | Production Manager |
| "When will this be deployed?" | Project Manager |

### Escalation

```
"My task is blocked"
→ Tell your immediate lead (developer → architect, etc)
→ They help troubleshoot
→ If can't resolve in 2 hours, escalate to PM

"I disagree with a decision"
→ Discuss with the decision maker
→ Document your concerns
→ If still disagree, escalate to PM

"Something is wrong post-deployment"
→ Post in #incidents channel
→ Page on-call engineer
→ Start incident response
→ Focus on restore service
```

---

## 🎓 Learning Resources

### Videos (5-10 minutes each)
- "How Features Move from Request to Production"
- "Using the Workflow System as a Developer"
- "Using the Workflow System as a PM"
- "Understanding Quality Gates"
- "What to Do When Blocked"

### Hands-On Exercises
1. Submit a feature request and track it
2. Develop a feature using the workflow
3. Review a colleague's code
4. Respond to a specialist review
5. Deploy a feature to staging

### Documentation
- [MASTER_WORKFLOW.md](../master/MASTER_WORKFLOW.md) - Complete system overview
- [AUTO_TRIGGERS.yaml](../triggers/AUTO_TRIGGERS.yaml) - Automation rules
- [WORKFLOWS_INDEX.md](../WORKFLOWS_INDEX.md) - All 19 agent roles
- [INTEGRATION_GUIDE.md](../INTEGRATION_GUIDE.md) - System setup

---

## 🚀 Your Next Steps

### Today
1. [ ] Read this Quick Start (10 min)
2. [ ] Bookmark the Documents folder
3. [ ] Join #smartlab-workflows Slack channel
4. [ ] Watch "How Features Move" video (5 min)

### This Week
1. [ ] Read your role's workflow (15 min)
2. [ ] Ask questions in Slack
3. [ ] Help a teammate understand the system
4. [ ] Submit a feature request as practice

### This Month
1. [ ] Complete a feature request through full cycle
2. [ ] Attend workflow training (optional, 30 min)
3. [ ] Give feedback on what works/what doesn't
4. [ ] Help improve documentation

---

## 💡 Pro Tips

**For Faster Approvals**:
- Provide complete information upfront (less back-and-forth)
- Ask clarifying questions early (don't wait for review)
- Communicate in Slack (faster than Jira comments)
- Test thoroughly before requesting review

**For Smoother Deployments**:
- Follow the architecture spec exactly (no surprises)
- Write clear commit messages (helps reviewers)
- Keep PRs small (easier to review, merge, test)
- Monitor post-deployment (catch issues early)

**For Better Collaboration**:
- Give feedback quickly (don't let work sit in review)
- Explain your reasoning in comments (help others learn)
- Celebrate wins (acknowledge good work)
- Help unblock teammates (team success > individual)

---

**Quick Start Guide Version**: 1.0  
**Last Updated**: 2026-01-30  
**Status**: Ready to Use  
**Questions?** Ask in #smartlab-workflows or email smartlab-team@company.com

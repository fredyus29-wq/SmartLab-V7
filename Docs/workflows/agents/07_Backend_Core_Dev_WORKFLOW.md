# AGENT INDIVIDUAL WORKFLOW: Backend Core Dev (Agent 07)

**Agente**: Backend Core Developer  
**Autoridade**: Level 7 (Developer)  
**Fases**: Task Understanding → Implementation → Code Review → Completion

---

## Workflow Entrada

**Trigger que ativa**:
- Status = TASK_ASSIGNED_TO_DEV
- Event = DEVELOPER_CLAIMED_TASK

**Input**:
```
{
  "task_id": "PH-002",
  "task_title": "pH Data Collection API",
  "sprint": "Sprint-001",
  "assignment_date": "2026-01-31",
  "deadline": "2026-02-05",
  "architecture_spec": "[link to architecture doc]",
  "acceptance_criteria": "[acceptance criteria from PM]"
}
```

---

## FASE 1: TASK UNDERSTANDING (1 day)

### Step 1.1 - Read & Understand Task

**Developer faz**:
```
1. Read Task Details
   - What's the task? (title)
   - Why is it needed? (context)
   - Who will use it? (consumers)
   - When is it needed? (deadline)

2. Read Architecture Spec
   - What API endpoints needed?
   - What data model?
   - What libraries/frameworks?
   - What architectural patterns?

3. Review Acceptance Criteria
   - What must be true for task complete?
   - What testing is required?
   - What documentation needed?

4. Ask Questions
   - Anything unclear?
   - Need clarification?
   - Missing details?
```

**Output**:
```yaml
task_understanding:
  title: "pH Data Collection API"
  purpose: "Collect pH readings from sensors"
  api_endpoints:
    - POST /api/ph-readings (create)
    - GET /api/ph-readings (list)
    - GET /api/ph-readings/{id} (get one)
    - GET /api/ph-readings/stats (analytics)
  
  data_model:
    reading_id: "UUID"
    batch_id: "UUID"
    sensor_id: "string"
    ph_value: "float (0-14)"
    timestamp: "datetime"
    operator_id: "UUID"
    
  acceptance_criteria:
    - API returns correct data ✓
    - Validation on inputs ✓
    - Error handling ✓
    - Response time < 200ms ✓
    - 80%+ test coverage ✓
    - Audit trail logged ✓
```

**Auto-Trigger Next Step**: Proceed to Implementation Planning

---

### Step 1.2 - Plan Implementation

**Developer faz**:
```
1. Design Approach
   - Which files/folders to create?
   - Which classes/functions needed?
   - What dependencies?
   - Any external APIs?

2. Break into Sub-tasks
   - Subtask 1: [specific work]
   - Subtask 2: [specific work]
   - Subtask 3: [specific work]
   - Estimated time per subtask

3. Identify Blockers
   - Do I have everything I need?
   - Any dependencies on other tasks?
   - Do I understand architecture?
   - Database schema ready?

4. Setup Development Environment
   - Clone repo
   - Create feature branch
   - Setup dev database
   - Verify build works
```

**Output**:
```yaml
implementation_plan:
  approach: "Express.js API with PostgreSQL"
  
  files_to_create:
    - src/api/ph-readings.routes.ts
    - src/services/ph-readings.service.ts
    - src/repositories/ph-readings.repository.ts
    - src/models/ph-reading.model.ts
    - tests/api/ph-readings.test.ts
    - tests/services/ph-readings.service.test.ts
  
  subtasks:
    - subtask: "Create data models"
      effort: "2 hours"
    
    - subtask: "Create repository layer"
      effort: "4 hours"
    
    - subtask: "Create service layer"
      effort: "3 hours"
    
    - subtask: "Create API routes"
      effort: "3 hours"
    
    - subtask: "Add validation"
      effort: "2 hours"
    
    - subtask: "Write unit tests"
      effort: "4 hours"
    
    - subtask: "Write integration tests"
      effort: "3 hours"
    
  total_effort: "21 hours (3 days)"
  
  blockers: []
  dependencies:
    - "Database schema must be created first"
    - "Architecture review completed"
```

**Auto-Trigger Next Step**: Start implementation

---

## FASE 2: IMPLEMENTATION (3 days)

### Step 2.1 - Code Implementation

**Developer faz daily**:
```
Day 1:
1. Create data models
2. Create repository (data access)
3. Write unit tests for repository
4. Commit work

Day 2:
1. Create service layer (business logic)
2. Write unit tests for service
3. Create API routes
4. Write API tests (happy path)
5. Commit work

Day 3:
1. Add error handling
2. Add validation
3. Write edge case tests
4. Performance optimization
5. Final review before PR
```

**Throughout Implementation**:
```
Code Quality:
- Follow architecture standards ✓
- Use proper naming conventions ✓
- Write clear comments ✓
- Keep functions small (< 50 lines) ✓
- DRY principle (no repeated code) ✓

Testing:
- Unit tests for logic ✓
- Integration tests for APIs ✓
- Edge case tests ✓
- Error path tests ✓
- Test coverage > 80% ✓

Performance:
- No N+1 database queries ✓
- No unnecessary loops ✓
- Proper indexing ✓
- Response time < 200ms ✓
```

**Auto-Triggers Throughout**:
- Code pushed to branch → Automated tests run
- Tests fail → Developer notified
- Tests pass → Proceed

**Output**:
```yaml
implementation_complete:
  task_id: "PH-002"
  code_ready: true
  
  files_changed: 7
  lines_added: 450
  lines_removed: 0
  
  test_coverage:
    unit_tests: 25
    integration_tests: 8
    coverage_percentage: "85%"
  
  local_testing:
    unit_tests: "PASSED"
    integration_tests: "PASSED"
    manual_testing: "PASSED"
  
  code_quality:
    linting: "PASSED"
    security_scan: "PASSED (no issues)"
    documentation: "COMPLETE"
```

**Auto-Trigger Next Step**: Submit for code review

---

## FASE 3: CODE REVIEW (1 day)

### Step 3.1 - Submit Pull Request

**Developer faz**:
```
1. Create Pull Request
   - Title: descriptive name
   - Description: what changed + why
   - Link to original task
   - Link to architecture spec
   - Link to tests

2. Self-Review
   - Does code look good?
   - Any obvious issues?
   - Any TODOs or FIXMEs?

3. Request Review
   - Assign to relevant architect
   - (Backend Architect for core APIs)
   - Mention deadline
```

**PR Template**:
```markdown
## Description
pH Data Collection API implementation per task PH-002

## Changes
- Created /api/ph-readings endpoints
- Implemented service layer for pH readings
- Added validation for pH values (0-14 range)
- 25 unit tests + 8 integration tests

## Related Issue
Task: PH-002
Deadline: 2026-02-05

## Testing
- All tests passing locally (25 unit + 8 integration)
- Manual testing completed
- Coverage: 85%

## Architecture Compliance
- Follows API design from arch spec
- Uses proper repository pattern
- Includes audit logging
- Error handling per standards
```

**Auto-Trigger Next Step**: Architect reviews code

---

### Step 3.2 - Address Code Review Feedback

**If feedback received**:
```
1. Read Feedback
   - What's the issue?
   - Why is it an issue?
   - How to fix?

2. Decide
   - Is it valid? (yes/no)
   - Does it need to be fixed? (yes/no)
   - Can I fix it? (yes/no)

3. Respond to Reviewer
   - If implementing: "Will fix"
   - If disagree: Explain reasoning
   - If question: Ask clarification

4. Implement Fixes
   - Make changes
   - Test still works
   - Commit new changes
   - Push to PR branch

5. Notify Reviewer
   - Comment on PR
   - "Fixed as requested, please re-review"
```

**Output**:
```yaml
code_review_result:
  status: "APPROVED"
  reviewer: "02_Backend_Architecture_Lead"
  feedback_items: 2
  feedback_addressed: true
  
  feedback:
    - issue: "Add input validation for empty strings"
      status: "FIXED"
    
    - issue: "Add error logging for database failures"
      status: "FIXED"
  
  additional_comments: "Great job! Code is clean and well-tested."
```

**Auto-Trigger Next Step**: Merge to main

---

## FASE 4: COMPLETION (Automatic)

### Step 4.1 - Merge & Integration Testing

**Auto-Trigger After Approval**:
```
1. Code Merged to Main
   - Automated tests run on main
   - All tests pass
   - No conflicts

2. Integration Testing Starts
   - Full system tests run
   - Feature integration verified
   - Performance tests run

3. Notify Integration Testing Team
   - Feature merged
   - Integration tests starting
   - Link to task
```

**Output**:
```yaml
merge_and_integration:
  merge_status: "SUCCESSFUL"
  merge_commit: "abc123def456"
  main_tests_status: "PASSING"
  
  integration_testing:
    status: "IN_PROGRESS"
    assigned_to: "05_QA_Security_Specialist"
    expected_completion: "2026-02-06"
```

### Step 4.2 - Task Completion

**When integration complete**:
```
Developer marks task complete:

Task Status: COMPLETE
- Code written: ✓
- Tests passing: ✓
- Code reviewed: ✓
- Merged to main: ✓
- Integration testing: ✓ (passed)
- Ready for deployment: ✓
```

---

## Output Format

**Complete Developer Output**:
```yaml
task_completion:
  task_id: "PH-002"
  title: "pH Data Collection API"
  developer: "07_Backend_Core_Dev"
  status: "COMPLETE"
  
  timeline:
    assigned_date: "2026-01-31"
    completion_date: "2026-02-04"
    deadline: "2026-02-05"
    days_early: 1
  
  code_metrics:
    files_created: 7
    lines_added: 450
    test_coverage: "85%"
    unit_tests: 25
    integration_tests: 8
    
  quality:
    code_review: "APPROVED"
    linting: "PASSED"
    security_scan: "NO_ISSUES"
    tests: "ALL_PASSING"
    
  architecture_compliance:
    follows_api_spec: true
    follows_design_patterns: true
    error_handling: "COMPLETE"
    audit_trail: "IMPLEMENTED"
```

---

## Escalation Paths

```
IF architecture unclear:
  → Ask architect for clarification
  → Continue with best understanding
  → Mention in PR for review

IF blocker discovered:
  → Notify architect immediately
  → Provide details of blocker
  → Wait for guidance

IF code review feedback large:
  → Discuss with reviewer
  → May need to break into smaller PRs
  → Ask for help if needed

IF tests failing:
  → Investigate failure
  → Fix code or test
  → Verify locally before pushing
```

---

## Success Metrics

Developer is successful if:
- ✓ Feature coded per specification
- ✓ Tests passing locally
- ✓ Code coverage > 80%
- ✓ Code review approved
- ✓ Merged to main
- ✓ Integration tests pass
- ✓ On or before deadline
- ✓ No post-merge issues

---

**Backend Core Dev Workflow Version**: 1.0  
**Last Updated**: 2026-01-30

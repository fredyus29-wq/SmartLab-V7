# SmartLab Workflow Execution Schema
# Defines how AI IDEs execute workflows and handle results
# Version: 1.0

---

## 1. Workflow Invocation Format

AI IDEs invoke workflows using this standardized format:

### Simple Invocation
```json
{
  "action": "execute_workflow",
  "agent_id": "07_Backend_Core_Dev",
  "phase": 2,
  "step": 1,
  "input_data": {
    "task_id": "PH-002",
    "task_title": "pH Data Collection API",
    "requirements": "Build API endpoint for pH monitoring"
  }
}
```

### Full Invocation with Context
```json
{
  "action": "execute_workflow",
  "correlation_id": "workflow_exec_20260130_001",
  "timestamp": "2026-01-30T14:30:00Z",
  
  "agent": {
    "id": "07_Backend_Core_Dev",
    "name": "Backend Core Developer",
    "authority_level": 7
  },
  
  "workflow": {
    "phase": 2,
    "step": 1,
    "phase_name": "Implementation",
    "step_name": "Task Understanding"
  },
  
  "input_data": {
    "task_id": "PH-002",
    "task_title": "pH Data Collection API",
    "description": "Add real-time pH monitoring to fermentation tanks",
    "acceptance_criteria": [
      "pH readings every 30 seconds",
      "Alerts when pH < 3.0 or > 4.0",
      "Data stored in LIMS",
      "Manual override available"
    ],
    "architecture_spec": {
      "api_design": "pH endpoint that accepts readings",
      "data_model": "pH reading with timestamp + batch",
      "integration": "LIMS data export"
    }
  },
  
  "context": {
    "project_id": "PH-MONITORING",
    "project_name": "SmartLab pH Monitoring",
    "team_members": ["dev_alice", "qa_bob", "arch_charlie"],
    "timeline": {
      "created_at": "2026-01-30",
      "target_completion": "2026-02-10"
    },
    "previous_outputs": {
      "phase_1_planning": {
        "pm_intake": "...",
        "architecture_design": "...",
        "specialist_approvals": "..."
      }
    }
  },
  
  "options": {
    "async": false,
    "timeout_seconds": 3600,
    "notify_on_complete": true,
    "stream_output": true
  }
}
```

---

## 2. Workflow Response Format

Workflows return structured responses:

### Successful Execution
```json
{
  "status": "COMPLETED",
  "execution_id": "exec_20260130_001",
  "timestamp": "2026-01-30T15:30:00Z",
  
  "workflow_info": {
    "agent_id": "07_Backend_Core_Dev",
    "phase": 2,
    "step": 1,
    "duration_seconds": 3600
  },
  
  "output": {
    "phase_status": "COMPLETED",
    "step_status": "COMPLETED",
    
    "step_1_analysis": {
      "task_understood": true,
      "requirements_clear": true,
      "architecture_understood": true,
      "blockers": []
    },
    
    "implementation_plan": {
      "approach": "Create data model → repository → API routes → tests",
      "files_to_create": [
        "src/models/pH_Reading.ts",
        "src/repository/pH_Repository.ts",
        "src/routes/pH_Routes.ts",
        "test/pH_API.test.ts"
      ],
      "subtasks": [
        "Create pH_Reading model with validation",
        "Create repository layer for CRUD",
        "Create API GET/POST endpoints",
        "Write unit tests (20 tests)",
        "Integration test with LIMS mock"
      ],
      "estimated_effort_hours": 21,
      "estimated_duration_days": 3
    },
    
    "next_step": {
      "phase": 2,
      "step": 2,
      "name": "Implementation",
      "description": "Start coding the implementation plan"
    },
    
    "ready_to_proceed": true
  },
  
  "next_trigger": {
    "event": "TASK_UNDERSTANDING_COMPLETE",
    "trigger_agent": "07_Backend_Core_Dev",
    "trigger_next_phase_step": {
      "phase": 2,
      "step": 2
    },
    "estimated_time": "2026-02-02T09:00:00Z"
  },
  
  "quality_checks": [
    {
      "check": "Plan clarity",
      "status": "PASS",
      "details": "Implementation plan is clear and executable"
    },
    {
      "check": "Requirements coverage",
      "status": "PASS",
      "details": "All acceptance criteria addressed"
    },
    {
      "check": "Architecture alignment",
      "status": "PASS",
      "details": "Plan follows architecture specification"
    }
  ],
  
  "metadata": {
    "ide_client": "cursor_v2",
    "model": "claude-3-opus",
    "tokens_used": {
      "input": 4500,
      "output": 2100
    }
  }
}
```

### Execution In Progress
```json
{
  "status": "IN_PROGRESS",
  "execution_id": "exec_20260130_001",
  
  "progress": {
    "percent": 45,
    "current_step": "Analyzing requirements",
    "estimated_time_remaining": "00:15:00"
  },
  
  "streaming_output": [
    "Analyzing task requirements...",
    "Understanding fermentation monitoring context...",
    "Reviewing architecture specification...",
    "Creating implementation plan..."
  ]
}
```

### Failed Execution
```json
{
  "status": "FAILED",
  "execution_id": "exec_20260130_001",
  
  "error": {
    "type": "MISSING_CONTEXT",
    "code": "ARCH_SPEC_NOT_PROVIDED",
    "message": "Architecture specification not provided in input_data",
    "details": "Cannot proceed without understanding API contract design"
  },
  
  "resolution": {
    "action": "ESCALATE_TO_PREVIOUS_PHASE",
    "reason": "Architecture design not complete",
    "escalate_to_agent": "02_Backend_Architecture_Lead",
    "escalate_to_phase": 1,
    "escalate_to_step": 2
  },
  
  "metadata": {
    "can_retry": false,
    "requires_manual_intervention": true
  }
}
```

### Blocked Workflow
```json
{
  "status": "BLOCKED",
  "execution_id": "exec_20260130_001",
  
  "blocker": {
    "type": "DEPENDENCY",
    "description": "Waiting for Architect code review",
    "blocking_agent": "02_Backend_Architecture_Lead",
    "blocking_since": "2026-02-04T10:00:00Z",
    "expected_resolution": "2026-02-04T17:00:00Z"
  },
  
  "can_continue_in_background": false,
  "requires_manual_resolution": false,
  "escalate_if_unresolved_after": "24 hours"
}
```

---

## 3. Streaming Responses for Real-Time IDEs

For AI IDEs that support streaming (Cursor, Continue, etc):

```json
{
  "streaming": true,
  "format": "server-sent-events",
  "events": [
    {
      "type": "workflow_started",
      "timestamp": "2026-01-30T15:30:00Z",
      "data": {
        "agent": "07_Backend_Core_Dev",
        "phase": 2,
        "step": 1
      }
    },
    {
      "type": "step_progress",
      "timestamp": "2026-01-30T15:30:05Z",
      "data": {
        "current_action": "Analyzing task requirements",
        "progress_percent": 10
      }
    },
    {
      "type": "step_progress",
      "timestamp": "2026-01-30T15:30:15Z",
      "data": {
        "current_action": "Understanding architecture context",
        "progress_percent": 25
      }
    },
    {
      "type": "step_complete",
      "timestamp": "2026-01-30T15:35:00Z",
      "data": {
        "step_result": { ... }
      }
    },
    {
      "type": "workflow_complete",
      "timestamp": "2026-01-30T15:35:30Z",
      "data": {
        "output": { ... },
        "next_action": "CODE IMPLEMENTATION"
      }
    }
  ]
}
```

---

## 4. IDE Integration: Function Definitions

For Claude, Cursor, and other LLMs, workflows are exposed as callable functions:

### Function Schema (Claude Format)
```json
{
  "type": "function",
  "function": {
    "name": "execute_smartlab_workflow",
    "description": "Execute a SmartLab agent workflow phase. Used to orchestrate development tasks autonomously.",
    
    "parameters": {
      "type": "object",
      "properties": {
        "agent_id": {
          "type": "string",
          "description": "The agent executing the workflow (e.g., '07_Backend_Core_Dev')",
          "enum": ["01_PM_Tech_Lead", "02_Backend_Architecture_Lead", ...]
        },
        "phase": {
          "type": "integer",
          "description": "Workflow phase number (1-4)",
          "minimum": 1,
          "maximum": 4
        },
        "step": {
          "type": "integer",
          "description": "Step within phase (1-5)",
          "minimum": 1,
          "maximum": 5
        },
        "input_data": {
          "type": "object",
          "description": "Input data for the workflow phase (structure varies by phase)",
          "properties": {
            "task_id": {
              "type": "string",
              "description": "Unique task identifier (e.g., 'PH-002')"
            },
            "task_title": {
              "type": "string",
              "description": "Human-readable task title"
            }
          },
          "additionalProperties": true
        },
        "context": {
          "type": "object",
          "description": "Project/task context from previous phases"
        }
      },
      "required": ["agent_id", "phase", "step", "input_data"]
    }
  }
}
```

### Function Call Example (IDE to Workflow Engine)
```json
{
  "type": "function_call",
  "id": "call_1",
  "function": {
    "name": "execute_smartlab_workflow",
    "arguments": {
      "agent_id": "07_Backend_Core_Dev",
      "phase": 2,
      "step": 1,
      "input_data": {
        "task_id": "PH-002",
        "task_title": "pH API",
        "description": "Build API for pH monitoring"
      }
    }
  }
}
```

### Function Result Returned to IDE
```json
{
  "type": "function_result",
  "id": "call_1",
  "content": {
    "status": "COMPLETED",
    "output": {
      "implementation_plan": "...",
      "next_step": "Start coding"
    }
  }
}
```

---

## 5. Workflow State Machine

IDEs should track workflow state using this machine:

```
REQUEST_CREATED
    ↓ [execute_workflow(01_PM_Tech_Lead, 1, 1)]
PLANNING_PHASE_STARTED
    ├─ [execute_workflow(02_Backend_Architecture_Lead, 1, 2)]
    ├─ [execute_workflow(03_Frontend_Architecture_Lead, 1, 2)]
    ├─ [execute_workflow(specialists_*, 1, 3)]
    └─ [execute_workflow(01_PM_Tech_Lead, 1, 4)]
        ↓
EXECUTION_PHASE_STARTED
    ├─ [execute_workflow(07_Backend_Core_Dev, 2, *)]
    ├─ [execute_workflow(09-10_Frontend_Dev, 2, *)]
    ├─ [execute_workflow(05_QA_Security_Specialist, 2, *)]
    └─ [execute_workflow(specialists_*, 2, *)]
        ↓
VERIFICATION_PHASE_STARTED
    ├─ [quality_gate_1_functionality]
    ├─ [quality_gate_2_security]
    ├─ [quality_gate_3_performance]
    └─ [quality_gate_4_compliance]
        ↓
VALIDATION_PHASE_STARTED
    ├─ [specialist_approvals]
    ├─ [architecture_signoff]
    └─ [pm_approval]
        ↓
DEPLOYMENT_READY
    ↓ [execute_workflow(04_DevOps_Specialist, 4, 1)]
DEPLOYMENT_COMPLETE
```

---

## 6. Error Recovery & Escalation

If a workflow fails, IDE should:

```javascript
// Pseudocode for IDE error handling
try {
  const result = await executeWorkflow(agentId, phase, step, input);
  
  if (result.status === "COMPLETED") {
    // Execute next workflow
    return triggerNextWorkflow(result.next_trigger);
  }
  
} catch (error) {
  
  // Get escalation guidance
  const escalation = await getEscalation({
    reason: error.type,
    agent_id: agentId,
    phase: phase,
    step: step
  });
  
  // Execute escalation action
  if (escalation.action === "ESCALATE_TO_PREVIOUS_PHASE") {
    return executeWorkflow(
      escalation.escalate_to_agent,
      escalation.escalate_to_phase,
      escalation.escalate_to_step,
      {}  // Let previous phase decide next steps
    );
  }
  
  if (escalation.action === "ESCALATE_TO_PM") {
    return notifyPM({
      reason: escalation.reason,
      agent: agentId,
      phase: phase,
      step: step,
      requires_action: true
    });
  }
  
  // Retry with exponential backoff
  if (escalation.can_retry) {
    await sleep(exponentialBackoff(attemptCount));
    return executeWorkflow(...); // retry
  }
}
```

---

## 7. IDE-Specific Adaptations

### Cursor Integration
```javascript
// In Cursor composer
const workflow = await executeSmartLabWorkflow({
  agent: "07_Backend_Core_Dev",
  phase: 2,
  step: 1,
  taskId: "PH-002"
});

// Result appears in chat as structured output
// User can see implementation plan, ask questions, etc
```

### Continue Integration
```javascript
// In Continue chat
@smartlab execute phase 2 step 1 for backend dev on PH-002

// Continue calls executeWorkflow() automatically
// Shows streaming progress in chat
```

### AntiGravity Integration
```javascript
// In AntiGravity context
{
  "command": "execute_smartlab_workflow",
  "params": {
    "agent_id": "07_Backend_Core_Dev",
    "phase": 2,
    "step": 1
  }
}
```

---

## 8. Async Execution for Long-Running Workflows

For phases that take hours or days:

```json
{
  "action": "execute_workflow_async",
  "agent_id": "07_Backend_Core_Dev",
  "phase": 2,
  "step": 1,
  
  "async_options": {
    "execute_in_background": true,
    "notify_when_complete": true,
    "notification_channels": ["slack", "ide"]
  },
  
  "poll_for_status": {
    "action": "get_workflow_status",
    "execution_id": "exec_20260130_001",
    "include_details": true
  }
}
```

---

## 9. Batch Workflow Execution

For running multiple workflows in sequence:

```json
{
  "action": "execute_workflow_sequence",
  "workflows": [
    {
      "agent_id": "02_Backend_Architecture_Lead",
      "phase": 1,
      "step": 2
    },
    {
      "agent_id": "03_Frontend_Architecture_Lead",
      "phase": 1,
      "step": 2
    },
    {
      "agent_id": "specialists_02_Food_Safety_Manager",
      "phase": 1,
      "step": 3,
      "wait_for_all_previous": true
    }
  ],
  
  "execution_options": {
    "parallel_limit": 2,
    "stop_on_failure": false,
    "continue_on_blocker": false
  }
}
```

---

## 10. Testing & Validation

IDEs can validate workflows before execution:

```json
{
  "action": "validate_workflow",
  "agent_id": "07_Backend_Core_Dev",
  "phase": 2,
  "step": 1,
  "input_data": { ... },
  
  "validate_against": [
    "schema_compliance",
    "input_completeness",
    "prerequisite_completion",
    "timeline_feasibility"
  ]
}
```

Response:
```json
{
  "valid": true,
  "issues": [],
  "warnings": [
    "Estimated effort (21h) is high - may impact timeline"
  ],
  "can_execute": true
}
```

---

## Summary

This schema enables AI IDEs to:
✅ Invoke workflows with simple function calls  
✅ Get structured, parseable responses  
✅ Track execution state and progress  
✅ Handle errors and escalations automatically  
✅ Manage async/long-running workflows  
✅ Execute batches of related workflows  
✅ Validate before execution  

**Result**: Fully autonomous feature development from request to deployment, orchestrated by AI agents with IDE integration.

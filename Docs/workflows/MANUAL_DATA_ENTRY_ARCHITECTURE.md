# MANUAL LABORATORY DATA ENTRY ARCHITECTURE
## SmartLab v1.0 MVP - Manual Input with Future IoT Roadmap

**Status**: Active (MVP - Manual Entry)  
**Version**: 1.0  
**Date**: 2026-01-30  
**Roadmap Target**: v2.0 (Sensor Integration Q3 2026)  

---

## Executive Summary

SmartLab v1.0 MVP is designed for **manual data entry** by trained laboratory technicians:
- QC Supervisor enters test results manually
- System validates against specification ranges
- Specialists review + approve
- Certificate automatically generated
- Audit trail captures all entries (who, when, what)

**v2.0 Roadmap** (Future): Automated data from laboratory equipment + sensors  
- Sensor data flows automatically
- Manual entry remains as fallback/override
- No breaking changes to current system

---

## Current Flow (v1.0 - Manual Entry)

```
┌─────────────────────────────────────────────────────────────────┐
│                    LABORATORY WORKFLOW (v1.0)                    │
│                   Manual Data Entry Process                      │
└─────────────────────────────────────────────────────────────────┘

1. SAMPLE INTAKE
   ├─ Sample arrives at lab
   ├─ QC Supervisor logs sample in system
   │  └─ Sample ID, product type, date received
   └─ Status: RECEIVED

2. TEST PLANNING
   ├─ PM determines which tests needed
   ├─ Tests assigned to QC Supervisor
   └─ Status: ASSIGNED

3. SAMPLE TESTING (Physical Lab)
   ├─ QC Supervisor conducts test
   ├─ Records result on paper/notebook
   │  └─ Test value, units, timestamp
   └─ Status: TESTED (manual record)

4. DATA ENTRY (SmartLab UI)
   ├─ QC Supervisor logs into SmartLab
   ├─ Opens TestResultForm
   ├─ Enters test values from manual records
   │  └─ System validates: range check, type check, units match
   ├─ Submits results
   └─ Status: PENDING_REVIEW

5. SPECIALIST REVIEW
   ├─ System routes to relevant specialists
   │  └─ Food Safety Manager (if food product)
   │  └─ QMS Specialist (if quality feature)
   │  └─ Production Manager (if operational impact)
   ├─ Each specialist reviews + approves
   ├─ Can request corrections (goes back to step 4)
   └─ Status: SPECIALIST_APPROVED

6. ARCHIVE + CERTIFICATE
   ├─ All approvals received
   ├─ Certificate automatically generated
   ├─ PDF stored in secure archive
   ├─ Signature blocks show all approvers
   └─ Status: COMPLETE + ARCHIVED

7. DELIVERY TO CUSTOMER
   ├─ Certificate emailed/printed
   ├─ Audit trail available if questioned
   └─ Historical data searchable
```

---

## Data Entry Form Structure (UI Components)

### TestResultForm - Core Data Entry Interface

```
┌──────────────────────────────────────────────────────────┐
│           TEST RESULT ENTRY FORM                         │
├──────────────────────────────────────────────────────────┤
│                                                          │
│  SAMPLE INFORMATION                                      │
│  ├─ Sample ID: [SAMPLE-2026-001] (readonly)             │
│  └─ Product: [Yogurt - Natural Flavor] (readonly)        │
│                                                          │
│  TEST DETAILS                                            │
│  ├─ Test Type: [pH Analysis ▼]                           │
│  ├─ Test Date: [2026-01-30 14:32]                        │
│  └─ Technician: [Maria Silva] (auto-filled)              │
│                                                          │
│  RESULT VALUE                                            │
│  ├─ pH Value: [6.8]                                      │
│  │  └─ Specification: 4.0 - 7.0 ✗ FAIL                  │
│  │     (Red box: value 6.8 is within spec 4.0-7.0)      │
│  │                                                       │
│  │  └─ Status: ✓ PASS (Green indicator)                 │
│  │                                                       │
│  ├─ Units: [pH ▼]                                        │
│  └─ Notes: [Sample appears normal, no issues]            │
│                                                          │
│  QUALITY PARAMETERS (Optional)                           │
│  ├─ ☑ Include in certificate                             │
│  ├─ ☐ Flag for additional testing                        │
│  └─ ☐ Attach image/documentation                         │
│                                                          │
│  [SAVE] [SUBMIT FOR REVIEW] [DRAFT]                      │
│                                                          │
└──────────────────────────────────────────────────────────┘
```

**Form Validation (Client-Side + Server-Side)**:
```yaml
Validation_Rules:
  
  Test_Value:
    - Type: Numeric
    - Range: Min/Max defined by specification
    - Decimal_Places: Up to 4
    - Required: Yes
  
  Units:
    - Type: Predefined list (pH, °C, %, g/L, etc)
    - Required: Yes
    - Auto-maps to database units
  
  Test_Date:
    - Type: Date + Time
    - Default: Current date/time
    - Allow_Past: Yes (manual entry from paper records)
    - Allow_Future: No (prevent data entry for future)
  
  Technician:
    - Type: User select (from active lab staff)
    - Required: Yes
    - Audit: Who entered, when entered
  
  Specification_Range:
    - Lower_Spec: Defines minimum acceptable value
    - Upper_Spec: Defines maximum acceptable value
    - Auto_Calculate: Pass/Fail based on value vs specs
    - Example: pH between 4.0-7.0
      └─ Value 6.8 → PASS (green)
      └─ Value 7.5 → FAIL (red)
  
  Notes:
    - Type: Free text (up to 1000 chars)
    - Optional
    - Stored in audit trail
```

### Data Validation Rules (Server-Side)

```yaml
Validation_Pipeline:
  
  Step_1_Type_Check:
    - Is value numeric? (if numeric test)
    - Is value text? (if qualitative test)
    - Reject if wrong type
  
  Step_2_Range_Check:
    - Is value >= lower_spec?
    - Is value <= upper_spec?
    - Mark PASS or FAIL
    - Prevent submission if CRITICAL FAIL
  
  Step_3_Data_Integrity:
    - Duplicate entry? (same value, same tech, < 1 hour apart)
    - Warn: "Similar entry exists, confirm?"
    - Units consistent with previous entries?
  
  Step_4_Compliance_Check:
    - Food safety test? (Check food safety rules)
    - Regulatory test? (Check regulatory requirements)
    - Lab certification test? (Check lab accreditation status)
  
  Step_5_Audit_Trail:
    - Record: User, timestamp, IP, form data
    - Immutable: Once submitted, cannot delete
    - Changes tracked: If edited, old value + new value logged
  
  Result:
    - ✓ VALID → Send for specialist review
    - ⚠️ WARNING → Allow submission but flag for review
    - ✗ INVALID → Reject, return errors to form
```

---

## Database Schema (Manual Entry)

### Test Results Table

```sql
CREATE TABLE test_results (
  -- Primary Key
  id BIGINT PRIMARY KEY,
  test_result_uuid UUID UNIQUE NOT NULL,
  
  -- Sample Reference
  sample_id VARCHAR(50) NOT NULL,
  sample_uuid UUID NOT NULL,
  product_id UUID NOT NULL,
  batch_number VARCHAR(100),
  
  -- Test Information
  test_type_id UUID NOT NULL,
  test_type_name VARCHAR(100),
  test_name VARCHAR(255),
  
  -- Result Value (Manual Entry)
  result_value DECIMAL(10,4) NOT NULL,
  result_units VARCHAR(50) NOT NULL,
  lower_specification DECIMAL(10,4),
  upper_specification DECIMAL(10,4),
  result_status ENUM('PASS', 'FAIL', 'HOLD') NOT NULL,
  
  -- Timing
  test_date_time TIMESTAMP NOT NULL,
  entry_date_time TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
  
  -- Technician Entry
  entered_by_user_id UUID NOT NULL,
  entered_by_name VARCHAR(255),
  
  -- Audit & Compliance
  notes TEXT,
  attachment_url VARCHAR(500),
  
  -- Approval Tracking
  specialist_review_status ENUM('PENDING', 'APPROVED', 'CONDITIONAL', 'REJECTED'),
  specialist_reviewer_id UUID,
  specialist_review_date TIMESTAMP,
  specialist_comments TEXT,
  
  -- v2.0 Future Fields (Not Used in v1.0)
  sensor_reading DECIMAL(10,4),
  sensor_device_id VARCHAR(100),
  sensor_confidence_percent INT,
  data_source ENUM('MANUAL', 'SENSOR', 'EQUIPMENT') DEFAULT 'MANUAL',
  
  -- Audit Trail
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  deleted_at TIMESTAMP (soft delete),
  change_log JSON, -- Track all modifications
  
  -- Indexes
  INDEX idx_sample_id (sample_id),
  INDEX idx_test_date (test_date_time),
  INDEX idx_status (result_status),
  INDEX idx_specialist_review (specialist_review_status),
  UNIQUE KEY unique_sample_test (sample_id, test_type_id)
);
```

### Audit Trail Table

```sql
CREATE TABLE audit_trail (
  id BIGINT PRIMARY KEY,
  
  -- What Changed
  entity_type VARCHAR(50),           -- 'test_result'
  entity_id UUID,                    -- test_result_uuid
  action VARCHAR(50),                -- 'CREATE', 'UPDATE', 'APPROVE', 'REJECT'
  
  -- Who Changed It
  user_id UUID,
  user_name VARCHAR(255),
  user_role VARCHAR(100),
  
  -- When
  timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  
  -- How Changed
  old_value JSON,                    -- Before state
  new_value JSON,                    -- After state
  change_description TEXT,
  
  -- Why (for approvals)
  approval_reason TEXT,
  rejection_reason TEXT,
  
  -- Security
  ip_address VARCHAR(45),
  user_agent TEXT,
  
  -- Indexes
  INDEX idx_entity (entity_type, entity_id),
  INDEX idx_timestamp (timestamp),
  INDEX idx_action (action),
  INDEX idx_user (user_id)
);
```

---

## API Endpoints (Manual Data Entry)

### 1. Submit Test Result
```
POST /api/lab/test-results

Request:
{
  "sample_id": "SAMPLE-2026-001",
  "test_type": "pH_Analysis",
  "result_value": 6.8,
  "result_units": "pH",
  "test_date_time": "2026-01-30T14:32:00Z",
  "entered_by": "maria-silva",
  "notes": "Sample appears normal, no issues"
}

Response (Success):
{
  "status": "success",
  "test_result_uuid": "550e8400-e29b-41d4-a716-446655440000",
  "test_result_id": 12345,
  "validation": {
    "specification_range": {
      "lower": 4.0,
      "upper": 7.0
    },
    "actual_value": 6.8,
    "status": "PASS"
  },
  "next_step": "specialist_review",
  "estimated_approval_time": "2 hours",
  "message": "Test result submitted. Awaiting specialist review."
}

Response (Validation Error):
{
  "status": "error",
  "errors": [
    {
      "field": "result_value",
      "message": "Value 8.5 exceeds upper specification 7.0",
      "severity": "WARNING"
    }
  ],
  "allow_submission_anyway": true
}
```

### 2. Get Test Result (Review)
```
GET /api/lab/test-results/{test_result_id}

Response:
{
  "test_result_uuid": "550e8400-e29b-41d4-a716-446655440000",
  "sample": {
    "sample_id": "SAMPLE-2026-001",
    "product": "Yogurt - Natural Flavor",
    "batch": "BATCH-2026-042"
  },
  "test": {
    "type": "pH Analysis",
    "entered_value": 6.8,
    "specification_min": 4.0,
    "specification_max": 7.0,
    "status": "PASS"
  },
  "entry_metadata": {
    "entered_by": "Maria Silva",
    "entered_at": "2026-01-30T14:32:00Z",
    "technician_notes": "Sample appears normal"
  },
  "specialist_review": {
    "pending_approvals": ["Food Safety Manager"],
    "completed_approvals": []
  },
  "audit_trail": [
    {
      "timestamp": "2026-01-30T14:32:00Z",
      "action": "CREATE",
      "user": "Maria Silva",
      "details": "Test result entered"
    }
  ]
}
```

### 3. Specialist Approval
```
POST /api/lab/test-results/{test_result_id}/approve

Request:
{
  "decision": "APPROVED",  // or "CONDITIONAL", "REJECTED"
  "reviewer_comment": "Food safety parameters verified OK",
  "approval_conditions": null  // if CONDITIONAL
}

Response:
{
  "status": "success",
  "test_result_uuid": "550e8400-e29b-41d4-a716-446655440000",
  "specialist_review_status": "APPROVED",
  "approved_by": "Food Safety Manager",
  "approved_at": "2026-01-30T15:45:00Z",
  "next_step": "certificate_generation",
  "message": "Test result approved by Food Safety Manager"
}
```

### 4. Generate Certificate
```
GET /api/lab/test-results/{test_result_id}/certificate

Response:
{
  "certificate_uuid": "660e8400-e29b-41d4-a716-446655440001",
  "pdf_url": "/certificates/CERT-2026-000012345.pdf",
  "certificate_number": "CERT-2026-000012345",
  "generated_at": "2026-01-30T16:00:00Z",
  "test_result_data": {...},
  "approvals": [
    {
      "specialist": "Food Safety Manager",
      "approved_at": "2026-01-30T15:45:00Z",
      "signature": "Digital signature data"
    }
  ],
  "archive_status": "ARCHIVED",
  "searchable": true
}
```

---

## v2.0 Roadmap: Sensor Integration (Future)

### Timeline
```
Q1 2026: v1.0 MVP (Manual entry - CURRENT)
Q2 2026: Pilot preparation + sensor procurement
Q3 2026: Pilot deployment (2-3 sensors)
Q4 2026: Full rollout (all laboratory equipment)
Q1 2027: Mobile app + field sensors
Q2 2027: Predictive analysis + ML models
```

### v2.0 Architecture Overview

```
LABORATORY EQUIPMENT & SENSORS
├─ pH Meter (Analog/Digital)
├─ Temperature Sensor (RTD, Thermocouple)
├─ Pressure Sensor
├─ Humidity Sensor
├─ Weight Scale
├─ Microbiology Equipment
└─ HPLC/Spectrophotometer

        ↓ (RS-232, USB, Network, Modbus)

IOT GATEWAY (Edge Device)
├─ Raspberry Pi / Industrial PLC
├─ Local Processing (filtering, validation)
└─ Buffering (if cloud down)

        ↓ (MQTT, HTTP, Modbus)

MESSAGE QUEUE (NATS / RabbitMQ)
├─ Pub/Sub pattern
├─ Data retention (24 hours)
└─ Multiple consumers

        ↓

DATA PROCESSING PIPELINE
├─ Validation (range check, outlier detection)
├─ Enrichment (metadata, calibration data)
├─ Aggregation (multiple sensors, calculations)
└─ Storage (time-series database)

        ↓

SMARTLAB DATABASE
├─ Real-time table (for live display)
├─ Historical table (time-series)
└─ Audit trail (immutable log)

        ↓

UI UPDATE (WebSocket)
└─ Display live sensor data in SmartLab UI
```

### Component Extension Example (v2.0)

```jsx
// v1.0: Manual entry only
<DataInput
  label="Temperature"
  type="number"
  value={tempValue}
  onChange={setTempValue}
/>

// v2.0: Can accept sensor data
<DataInput
  label="Temperature"
  type="number"
  value={tempValue}
  onChange={setTempValue}
  
  // NEW: Sensor configuration
  sensor_config={{
    enabled: true,
    device_id: "TEMP-PROBE-01",
    device_type: "RTD_PT100",
    sample_rate_ms: 1000,
    moving_average: 5,
    confidence_threshold: 0.95,
    fallback_to_manual: true  // If sensor fails
  }}
  
  // NEW: Live sensor data
  sensor_value={sensorLiveData.temperature}
  sensor_confidence={sensorLiveData.confidence}
  sensor_last_update={sensorLiveData.timestamp}
  
  // NEW: Events
  onSensorUpdate={handleSensorData}
  onSensorError={handleSensorError}
/>
```

### v2.0 Data Flow

```
Sensor Data (continuous stream):
├─ Raw value: 22.347°C
├─ Quality flag: 0.98 confidence
└─ Timestamp: 2026-01-30T14:32:15.234Z

  ↓ (Processing)

Processed Data:
├─ Moving average (5-point): 22.341°C
├─ Outlier detection: PASS
├─ Calibration adjusted: 22.338°C
└─ Confidence: HIGH

  ↓ (UI Update via WebSocket)

SmartLab UI:
├─ Display value: 22.338°C
├─ Show confidence: ✓ HIGH
├─ Allow override: ✓ Yes (manual entry)
└─ Audit log: "Sensor reading + any manual override"
```

### Backward Compatibility

```yaml
v1.0 → v2.0 Compatibility:

Breaking_Changes: NONE
- All v1.0 forms continue to work
- Manual entry still available + preferred fallback
- Existing data not modified
- Database schema compatible (new columns are optional)

Schema_Migration:
- Add columns to test_results table (sensor-related)
- All columns nullable for v1.0 data
- v2.0 code detects v1.0 vs v2.0 data source
- Queries work with both v1.0 (manual) and v2.0 (sensor) data

Data_Compatibility:
- v1.0 data: result_value entered manually, sensor_reading = NULL
- v2.0 data: result_value from sensor, manual_override = optional
- Reports work with both (data_source field distinguishes)
- Historical data searchable as-is

Operational_Process:
- v1.0: Manual entry → Submit → Review → Approve
- v2.0: Sensor data (auto) → Review → Approve (no entry step)
- v2.0 Fallback: If sensor fails → Manual entry (same as v1.0)
```

---

## Manual Entry Process - Detailed Workflow

```
STEP 1: SAMPLE INTAKE
├─ Sample arrives at lab
├─ QC Supervisor opens SmartLab
├─ Clicks "Register Sample"
├─ Form: Sample ID, product, batch, date received
└─ Status: REGISTERED in system

STEP 2: TEST ASSIGNMENT
├─ PM/QC Lead sees registered sample
├─ Assigns tests to QC Supervisor
│  └─ Example: "pH Analysis, Temperature Check, Microbial Count"
├─ QC Supervisor gets notification
└─ Status: TESTS ASSIGNED

STEP 3: PHYSICAL LABORATORY TESTING
├─ QC Supervisor conducts test
│  ├─ Takes sample
│  ├─ Runs test procedure
│  ├─ Records result on paper (lab notebook)
│  └─ Notes any observations
└─ Status: PHYSICALLY TESTED (not yet in system)

STEP 4: DATA ENTRY INTO SmartLab
├─ QC Supervisor returns to SmartLab
├─ Opens TestResultForm for sample
├─ Enters result values from paper records
│  ├─ Test value: 6.8
│  ├─ Units: pH
│  ├─ Test date/time: 2026-01-30 14:32
│  └─ Notes: "Sample normal appearance"
├─ System validates:
│  ├─ Value 6.8 vs Spec 4.0-7.0 = PASS ✓
│  └─ All required fields filled = OK ✓
├─ Clicks "SUBMIT FOR REVIEW"
└─ Status: SUBMITTED

STEP 5: SPECIALIST REVIEW (Automatic Routing)
├─ System determines specialists needed
│  └─ Food product? → Food Safety Manager
│  └─ QMS feature? → QMS Specialist
│  └─ Production impact? → Production Manager
├─ System sends review notifications
├─ Specialist logs in, sees results
│  ├─ Reviews test values
│  ├─ Checks specification ranges
│  ├─ Reads technician notes
│  └─ Makes decision: APPROVE/CONDITIONAL/REJECT
├─ Specialist adds comment (why approved?)
└─ Status: SPECIALIST_REVIEWED

STEP 6: AUTO-DECISION IF ALL APPROVED
├─ If ALL specialists approve
│  ├─ Status automatically changes to: APPROVED
│  └─ Trigger: Certificate generation
├─ If ANY specialist conditional/reject
│  ├─ Needs resolution (goes back to step 4)
│  └─ QC Supervisor can edit/resubmit
└─ Status: DECISION_MADE

STEP 7: CERTIFICATE GENERATION
├─ System checks: All approvals received? ✓
├─ Generates PDF certificate
│  ├─ Sample info
│  ├─ Test results with values
│  ├─ Specification ranges + PASS/FAIL
│  ├─ Approval signatures (digital)
│  └─ Timestamp + validation
├─ Stores in secure archive (immutable)
└─ Status: CERTIFICATE_GENERATED

STEP 8: DELIVERY TO CUSTOMER
├─ Certificate emailed to customer
├─ Optional: Print physical certificate
├─ Customer can verify certificate online
│  ├─ Search by certificate number
│  ├─ View all details
│  └─ Verify digital signature
└─ Status: DELIVERED

STEP 9: PERMANENT ARCHIVE
├─ Test result stored forever
├─ Searchable by:
│  ├─ Sample ID
│  ├─ Product name
│  ├─ Batch number
│  ├─ Test date
│  └─ Technician
├─ Audit trail immutable
└─ Status: ARCHIVED
```

---

## Error Handling & Data Correction

### Scenario 1: Data Entry Error (Before Specialist Review)

```
QC Supervisor entered: pH = 7.8 (should be 6.8)
System shows: ⚠️ Value outside spec (7.0 max)

Options:
1. QC Supervisor edits (before submit)
   └─ System tracks: Changed from X to Y at Z time
   └─ Audit log: Shows both values

2. Specialist requests correction (after submit)
   └─ Specialist clicks "Request Correction"
   └─ Goes back to QC Supervisor
   └─ QC Supervisor edits + resubmits
   └─ All changes logged
```

### Scenario 2: Equipment Malfunction (Sensor Reads Wrong - v2.0)

```
v2.0 Scenario (when sensors implemented):
├─ Sensor sends anomalous reading
├─ System detects outlier (statistical analysis)
├─ Flags as "MANUAL VERIFICATION NEEDED"
├─ QC Supervisor performs manual confirmation
├─ Enters manual override value
└─ Audit shows: Sensor value vs Manual override

v1.0 Scenario (current):
└─ QC Supervisor manually detects error
   └─ Re-runs test
   └─ Enters correct value
   └─ Audit shows: Both entries for same test
```

### Scenario 3: Regulation Violation (After Approval)

```
If specialist review LATER reveals compliance issue:
├─ System supports "Reject" even after approval
├─ Revokes all approvals
├─ Goes back to QC Supervisor
├─ QC Supervisor investigates + corrects
├─ Full audit trail preserved (shows what happened + why)
└─ No data deleted (append-only audit)
```

---

## Summary: v1.0 Manual Entry Ready for Production

| Aspect | Details |
|--------|---------|
| **Data Entry** | Manual, by trained QC operators |
| **Validation** | Client-side + server-side checks |
| **Approval Process** | Multi-specialist routing + sign-off |
| **Audit Trail** | Complete (who, when, what, why) |
| **Error Handling** | Clear, with correction workflows |
| **Compliance** | WCAG AA + regulatory audit-ready |
| **Certificate Generation** | Automated, PDF with digital signatures |
| **Archive & Search** | Permanent storage, searchable history |
| **v2.0 Ready** | Architecture prepared for sensors |

---

## Transition to v2.0 (Not Required for MVP)

When ready for sensor integration:
1. Deploy IoT gateway + sensors
2. Configure message queue + data pipeline
3. Update components with sensor_config props
4. Maintain backward compatibility (v1.0 data still works)
5. Run pilot (2-3 sensors) before full rollout
6. Migrate to fully automated (optional)

---

**Status**: ✅ v1.0 Production Ready  
**Next Phase**: v2.0 Roadmap (Future)  
**Last Updated**: 2026-01-30

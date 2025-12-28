# CrashCodex Evaluation

**What it does. What's measured. How to run eval.**

---

## What CrashCodex Does

CrashCodex generates collision repair estimates with built-in compliance validation. It refuses to approve incomplete estimates rather than guessing.

**Core behaviors:**
- Blocks estimates missing required ADAS calibrations
- Flags non-OEM parts for DRP insurers (GEICO, State Farm)
- Applies geographic labor rate adjustments
- Requires structural assessment for structural damage claims

---

## What's Measured

### Compliance Regression Suite (deterministic)

| Metric | Value |
|--------|-------|
| Corpus Size | 28,556 embedded vectors |
| Regression Tests | 6 |
| Pass Rate | 100% |
| Avg Validation Time | <1ms per test (local) |

### Retrieval + Scenario Suite (behavior under partial evidence)

| Metric | Value |
|--------|-------|
| Scenario Eval Cases | 50+ |
| Precision@5 | ~87% (internal search tests) |
| End-to-End Latency | ~180ms avg (internal runs) |

**Definitions:**
- **Precision@5**: Percent of queries where at least one relevant evidence chunk appears in top 5 retrieved results.
- **End-to-End Latency**: Full pipeline time (retrieve -> reason/generate -> validate).

---

## How to Run Eval

### Regression Suite (fast, deterministic)

```bash
npm run eval
```

**Output:**
```
======================================================================
  CRASHCODEX EVALUATION SUITE
======================================================================

Corpus Size: 28,556 vectors
Eval Cases:  6 regression tests

----------------------------------------------------------------------
Running tests...

  [PASS] ADAS_001: Blocks estimate when OEM calibration missing
  [PASS] ADAS_002: Approves estimate with calibration included
  [PASS] DRP_001: Flags non-OEM parts for GEICO DRP
  [PASS] DRP_002: Approves OEM parts for GEICO DRP
  [PASS] LABOR_001: Applies geographic rate adjustment for CA
  [PASS] STRUCT_001: Flags structural damage for frame inspection

----------------------------------------------------------------------

RESULTS SUMMARY

  Pass Rate:     6/6 (100.0%)
  Corpus Size:   28,556 vectors

======================================================================
  KEY METRICS
======================================================================

  | Metric                | Value                    |
  |-----------------------|--------------------------|
  | Corpus Size           | 28,556                   |
  | Regression Tests      | 6                        |
  | Pass Rate             | 100.0%                   |
  | Avg Validation Time   | <1ms                     |

======================================================================
  RETRIEVAL METRICS (from internal search tests)
======================================================================

  | Metric                | Value                    |
  |-----------------------|--------------------------|
  | Scenario Eval Cases   | 50+                      |
  | Precision@5           | ~87%                     |
  | End-to-End Latency    | ~180ms avg               |

  Precision@5: % of queries where relevant evidence appears in top 5 results.
  End-to-End: Full pipeline (retrieve -> reason -> validate).

======================================================================
  COMPLIANCE BEHAVIORS TESTED
======================================================================

  [x] Blocks estimate when OEM calibration missing
  [x] Flags non-OEM parts for DRP insurers
  [x] Applies geographic labor rate adjustments
  [x] Requires structural assessment for structural damage
```

---

## Regression Tests

| ID | Test | Behavior |
|----|------|----------|
| ADAS_001 | Blocks estimate when OEM calibration missing | Refuse |
| ADAS_002 | Approves estimate with calibration included | Approve |
| DRP_001 | Flags non-OEM parts for GEICO DRP | Refuse |
| DRP_002 | Approves OEM parts for GEICO DRP | Approve |
| LABOR_001 | Applies geographic rate adjustment for CA | Validate |
| STRUCT_001 | Flags structural damage for frame inspection | Refuse |

---

## Key Regression Test: Refuse When Missing

**Test ADAS_001** validates the system blocks incomplete estimates:

```typescript
// Input: Front collision with camera damage, NO calibration line
{
  vehicle: { year: 2020, make: 'Honda', model: 'Accord' },
  damage: 'Front collision with forward camera damage',
  lineItems: [
    { operation: 'Replace front bumper', category: 'Body' },
    { operation: 'Replace hood', category: 'Body' }
    // NO calibration line item
  ]
}

// Expected: FAIL with violation
{
  shouldPass: false,
  violation: 'Required ADAS calibration not included'
}
```

The system refuses to approve this estimate. It flags the missing calibration instead of guessing.

---

## Why This Matters

In collision repair, a missed calibration is a safety hazard. ADAS systems (forward collision warning, lane departure, adaptive cruise) require recalibration after front-end repairs. Missing this step means:

- Vehicle safety systems may not function correctly
- Shop liability for accidents caused by miscalibrated sensors
- Insurance claim disputes and potential fraud allegations

CrashCodex's validation layer catches these issues before the estimate leaves the shop.

---

## Architecture

```
scripts/eval.ts                    - Regression suite (npm run eval)
lib/ragEvaluationSystem.ts         - 50+ scenario eval cases
core/engines/
  oemProcedureEngine.ts            - OEM compliance validation
  multiLayerValidationSystem.ts    - 5-layer validation
lib/services/
  DRPComplianceEngine.ts           - Insurance DRP rules
```

---

## Source Code

| Component | Path |
|-----------|------|
| Eval Script | [scripts/eval.ts](https://github.com/resetroot99/tool/blob/main/scripts/eval.ts) |
| Scenario Suite | [lib/ragEvaluationSystem.ts](https://github.com/resetroot99/tool/blob/main/lib/ragEvaluationSystem.ts) |
| OEM Validation | [core/engines/oemProcedureEngine.ts](https://github.com/resetroot99/tool/blob/main/core/engines/oemProcedureEngine.ts) |
| DRP Compliance | [lib/services/DRPComplianceEngine.ts](https://github.com/resetroot99/tool/blob/main/lib/services/DRPComplianceEngine.ts) |

---

## Exit Codes

- `0` - All tests pass
- `1` - One or more tests fail

Use in CI:
```bash
npm run eval || exit 1
```

---

**Part of the AI Systems Research & Development Portfolio**

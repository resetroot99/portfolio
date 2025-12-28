# CrashCodex Evaluation

What it does. What's measured. How to run evaluation.

---

## What CrashCodex Does

CrashCodex generates collision repair estimates with built-in compliance validation. It refuses to approve incomplete estimates rather than guessing.

**Core behaviors tested here:**
- Blocks estimates missing required ADAS calibrations
- Flags non-OEM parts for DRP insurers (e.g., GEICO)
- Applies geographic labor rate adjustments (currently tested for CA)
- Requires structural assessment when structural damage is present

---

## Evaluation Overview

CrashCodex has two evaluation layers:

**1) Compliance Regression Suite (deterministic)**
- Fast, repeatable tests for "must not guess" safety/compliance behavior
- Runnable locally via `npm run eval`
- Current coverage: 6 regression tests

**2) Scenario + Retrieval Evaluation (behavior under partial evidence)**
- Larger scenario suite used to track retrieval quality and end-to-end behavior
- Runnable locally via `npm run eval:scenarios`
- Current coverage: 50+ scenarios

---

## What's Measured

### Compliance Regression Suite (`npm run eval`)

| Metric | Current Value |
|--------|---------------|
| Corpus Size | 28,556 embedded records (vectors) |
| Regression Tests | 6 |
| Pass Rate | 100% |
| Timing | Reported by eval runner (machine-dependent) |

**Compliance behaviors covered:**
- `ADAS_001`: Blocks estimate when OEM calibration is missing
- `ADAS_002`: Approves estimate with calibration included
- `DRP_001`: Flags non-OEM parts for GEICO DRP
- `DRP_002`: Approves OEM parts for GEICO DRP
- `LABOR_001`: Applies geographic rate adjustment for CA
- `STRUCT_001`: Flags structural damage for frame inspection

### Scenario + Retrieval Evaluation (`npm run eval:scenarios`)

| Metric | Current Value |
|--------|---------------|
| Scenario Eval Cases | 50+ scenarios |
| Precision@5 | ~87% (internal search tests) |
| End-to-End Latency | ~750ms -> ~180ms avg (retrieve -> reason/generate -> validate) |
| Corpus Size | 28,556 embedded records (vectors) |

**Definitions:**
- **Precision@5**: percent of labeled queries where at least one relevant evidence chunk appears in the top 5 retrieved results.
- **End-to-end latency**: full pipeline time (retrieve -> reason/generate -> validate), measured in internal end-to-end runs.

---

## How to Run Compliance Regression Eval

```bash
npm run eval
```

**Example output** (timings will vary by machine):

```
======================================================================
  CRASHCODEX COMPLIANCE REGRESSION SUITE
======================================================================

Corpus Size:        28,556 vectors
Regression Tests:   6

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

  Pass Rate:   6/6 (100.0%)
  Corpus:      28,556 vectors
```

---

## How to Run Scenario + Retrieval Evaluation

```bash
npm run eval:scenarios
```

**Example output:**

```
======================================================================
  CRASHCODEX SCENARIO + RETRIEVAL EVALUATION
======================================================================

Scenario Suite:     lib/ragEvaluationSystem.ts
Total Scenarios:    50
Corpus Size:        28,556 vectors

----------------------------------------------------------------------
RETRIEVAL METRICS (from internal runs)

  | Metric                  | Value                      |
  |-------------------------|----------------------------|
  | Precision@5             | 87%                        |
  | Precision@10            | 92%                        |
  | Mean Reciprocal Rank    | 0.83                       |
  | Abstention Rate         | 12% (correct refusals)     |

======================================================================
  END-TO-END LATENCY
======================================================================

  Before optimization:  ~750ms avg
  After optimization:   ~180ms avg
  Improvement:          76%

  Pipeline: retrieve -> reason/generate -> validate
```

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

In collision repair, a missed calibration is a safety hazard. ADAS systems require recalibration after front-end repairs. Missing this step can mean:

- Safety systems may not function correctly
- Shop liability exposure
- Insurance disputes and compliance risk

CrashCodex's validation layer catches these issues before the estimate leaves the shop.

---

## Architecture (high level)

```
scripts/eval.ts                         - Compliance regression suite (npm run eval)
scripts/eval-scenarios.ts               - Scenario + retrieval eval (npm run eval:scenarios)
lib/ragEvaluationSystem.ts              - 50+ scenario definitions
core/engines/
  oemProcedureEngine.ts                 - OEM compliance checks
  multiLayerValidationSystem.ts         - Multi-layer validation orchestration
lib/services/
  DRPComplianceEngine.ts                - Insurer DRP rules
```

---

## Source Code

Note: runnable code lives in `resetroot99/tool`; portfolio documentation lives in `resetroot99/portfolio`.

| Component | Link |
|-----------|------|
| Code Repository | [github.com/resetroot99/tool](https://github.com/resetroot99/tool) |
| Eval Script | [scripts/eval.ts](https://github.com/resetroot99/tool/blob/main/scripts/eval.ts) |
| Scenario Eval | [scripts/eval-scenarios.ts](https://github.com/resetroot99/tool/blob/main/scripts/eval-scenarios.ts) |
| Scenario Suite | [lib/ragEvaluationSystem.ts](https://github.com/resetroot99/tool/blob/main/lib/ragEvaluationSystem.ts) |
| OEM Validation | [core/engines/oemProcedureEngine.ts](https://github.com/resetroot99/tool/blob/main/core/engines/oemProcedureEngine.ts) |

---

## Portfolio Documentation

| Document | Link |
|----------|------|
| Eval Metrics | [CrashCodex-EVAL.md](https://github.com/resetroot99/portfolio/blob/main/CrashCodex-EVAL.md) |
| CrashCodex Overview | [CrashCodex.md](https://github.com/resetroot99/portfolio/blob/main/CrashCodex.md) |
| Portfolio README | [README.md](https://github.com/resetroot99/portfolio/blob/main/README.md) |

---

**Part of the AI Systems Research & Development Portfolio**

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
- Metrics reported in the portfolio documentation (Precision@5, end-to-end latency)
- Scenario suite location: `lib/ragEvaluationSystem.ts`

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

### Scenario + Retrieval Evaluation (documented)

These are the metrics referenced in the Residency application materials:

| Metric | Current Value |
|--------|---------------|
| Scenario Eval Cases | 50+ scenarios |
| Precision@5 | ~87% (internal search tests) |
| End-to-End Latency | ~750ms -> ~180ms avg (internal runs) |
| Corpus Size | 28,556 embedded records (vectors) |

**Definitions:**
- **Precision@5**: percent of labeled queries where at least one relevant evidence chunk appears in the top 5 retrieved results.
- **End-to-end latency**: full pipeline time (retrieve -> reason/generate -> validate), measured in internal runs.

*If you're reviewing quickly: this file proves the compliance "refuse/flag" behaviors are testable and repeatable. The broader scenario and retrieval metrics are linked below.*

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
  Avg Time:    (reported by runner)
  Corpus:      28,556 vectors
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
lib/ragEvaluationSystem.ts              - 50+ scenario eval cases
core/engines/
  oemProcedureEngine.ts                 - OEM compliance checks (e.g., calibration required)
  multiLayerValidationSystem.ts         - Multi-layer validation orchestration
lib/services/
  DRPComplianceEngine.ts                - Insurer DRP rules (e.g., GEICO)
```

---

## Source Code

| Component | Link |
|-----------|------|
| Repository | https://github.com/resetroot99/tool |
| Eval Script | [scripts/eval.ts](https://github.com/resetroot99/tool/blob/main/scripts/eval.ts) |
| Scenario Suite | [lib/ragEvaluationSystem.ts](https://github.com/resetroot99/tool/blob/main/lib/ragEvaluationSystem.ts) |
| OEM Validation | [core/engines/oemProcedureEngine.ts](https://github.com/resetroot99/tool/blob/main/core/engines/oemProcedureEngine.ts) |

---

## Portfolio Documentation

- [CrashCodex-EVAL.md](https://github.com/resetroot99/portfolio/blob/main/CrashCodex-EVAL.md)
- [CrashCodex.md](https://github.com/resetroot99/portfolio/blob/main/CrashCodex.md)
- [README.md](https://github.com/resetroot99/portfolio/blob/main/README.md)

---

**Part of the AI Systems Research & Development Portfolio**

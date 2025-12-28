# CrashCodex Evaluation

**What it does. What's measured. How to run eval.**

---

## What CrashCodex Does

CrashCodex generates collision repair estimates with built-in compliance validation. It refuses to approve incomplete estimates rather than guessing.

**Core behaviors:**
- Blocks estimates missing required ADAS calibrations
- Flags non-OEM parts for DRP insurers (GEICO, State Farm)
- Applies geographic labor rate adjustments (CA, NY, etc.)
- Requires structural assessment for structural damage claims

---

## What's Measured

| Metric | Current Value |
|--------|---------------|
| Corpus Size | 28,556 vectors |
| Eval Cases | 6 regression tests |
| Pass Rate | 100% |
| Avg Response Time | <5ms per validation |

**Compliance behaviors tested:**
- `ADAS_001`: Blocks estimate when OEM calibration missing
- `ADAS_002`: Approves estimate with calibration included
- `DRP_001`: Flags non-OEM parts for GEICO DRP
- `DRP_002`: Approves OEM parts for GEICO DRP
- `LABOR_001`: Applies geographic rate adjustment for CA
- `STRUCT_001`: Flags structural damage for frame inspection

---

## How to Run Eval

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
  Avg Time:      0.0ms per test
  Corpus Size:   28,556 vectors

======================================================================
  COMPLIANCE BEHAVIORS TESTED
======================================================================

  [x] Blocks estimate when OEM calibration missing
  [x] Flags non-OEM parts for DRP insurers
  [x] Applies geographic labor rate adjustments
  [x] Requires structural assessment for structural damage
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

**In collision repair, a missed calibration is a safety hazard.** ADAS systems (forward collision warning, lane departure, adaptive cruise) require recalibration after front-end repairs. Missing this step means:

- Vehicle safety systems may not function correctly
- Shop liability for accidents caused by miscalibrated sensors
- Insurance claim disputes and potential fraud allegations

CrashCodex's validation layer catches these issues before the estimate leaves the shop.

---

## Architecture

```
scripts/eval.ts          - Evaluation suite (npm run eval)
core/engines/            - Validation engines
  oemProcedureEngine.ts  - OEM compliance checks
  multiLayerValidationSystem.ts - 5-layer validation
lib/services/
  DRPComplianceEngine.ts - Insurance DRP rules
```

---

## API Response

**POST /api/v1/estimates**

Returns estimate with validation results:

```json
{
  "success": true,
  "data": {
    "estimate": { ... },
    "validation": {
      "valid": false,
      "violations": ["Required ADAS calibration not included"],
      "complianceScore": 0.8
    }
  }
}
```

---

## Source Code

- **Repository:** [github.com/resetroot99/tool](https://github.com/resetroot99/tool)
- **Eval Script:** [scripts/eval.ts](https://github.com/resetroot99/tool/blob/main/scripts/eval.ts)
- **OEM Validation:** [core/engines/oemProcedureEngine.ts](https://github.com/resetroot99/tool/blob/main/core/engines/oemProcedureEngine.ts)

---

**Part of the AI Systems Research & Development Portfolio**

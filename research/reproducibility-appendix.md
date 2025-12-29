# Reproducibility Appendix

Exact commands, exact outputs, where numbers come from, what is measured vs estimated.

---

## Commands

| Command | What it does | Runtime |
|---------|--------------|---------|
| `npm run eval` | 6 deterministic compliance tests | ~2s |
| `npm run eval:scenarios` | Reports 60-scenario suite metrics | ~1s |
| `npm run demo:flagship` | Baseline vs governed comparison | ~1s |

---

## Where Numbers Come From

### Measured (deterministic, reproducible)

| Metric | Source | How to verify |
|--------|--------|---------------|
| Corpus Size: 28,556 | Database row count | SQL query |
| Regression Tests: 6 | Test array in source | Count in code |
| Pass Rate: 100% | Command output | Run command |
| Scenario Count: 60 | Evaluation suite | 3 manual + 47 generated + 10 adversarial |

### Measured (from internal runs, hardware-dependent)

| Metric | Source | How measured |
|--------|--------|--------------|
| Precision@5: ~87% | Retrieval tests | % of queries with relevant chunk in top 5 |
| Precision@10: ~92% | Retrieval tests | % of queries with relevant chunk in top 10 |
| MRR: ~0.83 | Retrieval tests | Mean reciprocal rank |
| End-to-End Latency: ~180ms | Pipeline timing | Timestamp deltas |
| Abstention Rate: ~12% | Scenario evaluation | % returning ABSTAIN/NEEDS_REVIEW |

### Estimated/Simulated (demo only)

| Metric | Source | Caveat |
|--------|--------|--------|
| Silent Failure Rate: 33.3% | Demo simulation | 12 cases, not production |
| Expected Utility: -2.42 vs -0.42 | Demo simulation | Utility model: +1/-10/-1 |
| Coverage AUC: 0.78 | Demo simulation | Concordance-based |
| Confidence AUC: 0.52 | Demo simulation | Mock values |

---

## What Is Measured vs Estimated

### Measured

| Category | Items |
|----------|-------|
| Corpus stats | Document count, vector count |
| Test coverage | Regression tests, scenarios, adversarial cases |
| Pass/fail | Deterministic test results |
| Retrieval quality | Precision@K, MRR (labeled queries) |
| Latency | Pipeline timing (hardware-dependent) |

### Estimated/Simulated

| Category | Items | Why |
|----------|-------|-----|
| Silent failure rate | Commit rate without evidence | No production telemetry |
| Utility scores | Harm-weighted outcomes | Assumed utility model |
| Harm prediction AUC | Predictor comparison | Small sample, derived labels |
| Confidence values | Model confidence | Mock values for demo |

---

## Key Outputs

### Compliance Regression (`npm run eval`)

```
Corpus Size:        28,556 embedded records
Regression Tests:   6
Pass Rate:          6/6 (100.0%)

[PASS] Blocks estimate when required evidence missing
[PASS] Approves estimate with evidence included
[PASS] Flags policy violations
[PASS] Approves compliant cases
[PASS] Applies geographic adjustments
[PASS] Flags structural damage for inspection
```

### Research Demo (`npm run demo:flagship`)

```
AGGREGATE METRICS

  │ METRIC                          │ BASELINE      │ GOVERNED          │
  │ Coverage (avg)                  │ N/A           │           78.5%   │
  │ Approval Rate                   │        91.7%  │           58.3%   │
  │ Silent Failure Rate             │        33.3%  │            0.0%   │
  │ Expected Utility                │        -2.42  │           -0.42   │

INTERPRETATION:
"The safer system does less—and produces more value."

HARM PREDICTION:
  - Evidence Coverage AUC: 0.78
  - Model Confidence AUC:  0.52
  - Winner: COVERAGE

"Evidence coverage beats confidence at predicting harm."
```

---

## Caveats for Reviewers

1. **Retrieval metrics** are from internal runs with labeled queries, not production traffic.

2. **Latency improvement** measured on specific hardware. Results will vary.

3. **Utility scores** use assumed model (+1/-10/-1). Relative comparison valid; absolute values model-dependent.

4. **Harm prediction AUCs** from 12 demo cases. Directionally correct, not statistically significant.

5. **Adversarial cases** are hand-crafted, not discovered through fuzzing.

---

## What Would Make This Production-Ready

| Gap | What's needed |
|-----|---------------|
| Production telemetry | Log decisions + outcomes |
| Ground truth labels | Human-labeled harm outcomes |
| Larger eval set | 500+ labeled scenarios |
| Real confidence | LLM logprobs or calibrated confidence |
| Shift detection | Automated OOD detection |

---

## Verification Philosophy

Claims are structured as:
- **Measured:** deterministic, reproducible
- **Estimated:** simulation-based, assumptions documented
- **Not claimed:** things we haven't measured

All claims can be verified by running the commands above.

---

**Part of the AI Systems Research & Development Portfolio**

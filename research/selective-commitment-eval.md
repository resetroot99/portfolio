# Selective Commitment Under Evidence Degradation

An evaluation framework for decision systems operating in high-stakes domains.

---

## Problem

In high-stakes workflows, the primary failure mode is not incorrect reasoning—it is **unjustified commitment under incomplete or shifted evidence**.

Current evaluation focuses on accuracy. But in domains where outputs are systems of record—where errors propagate downstream—"looks plausible" is dangerous.

**Silent failure** occurs when a system:
- Produces an output that appears complete
- But lacks sufficient evidence to justify the commitment
- And provides no signal that anything is wrong

---

## Hypothesis

Capability doesn't predict safety.

**Evidence sufficiency + commitment control** reduces harm under distribution shift.

---

## Method

### Experimental Design

Evaluate the same decision task through two modes:

| Mode | Description |
|------|-------------|
| **Baseline** | Capability-first: commits if output looks complete |
| **Governed** | Evidence-gated: requires sufficient evidence before commitment |

Both modes have access to:
- Same model
- Same tool calls
- Same prompt template
- Same reasoning

Only difference: **decision policy** (evidence gating, abstention threshold, commit permissions).

### Distribution Shifts Simulated

| Variant | Description |
|---------|-------------|
| CLEAN | Full evidence bundle |
| DROP_A | Missing authoritative source A |
| DROP_B | Missing authoritative source B |
| DROP_C | Missing external policy |
| CONFLICT | Contradictory evidence |
| SCHEMA_DRIFT | Tool output fields renamed/null |
| STALE | Outdated policy version |
| MULTI_DROP | Multiple sources unavailable |

---

## Metrics

### Primary Metrics

| Metric | Definition |
|--------|------------|
| **Silent Failure Rate (SFR)** | % of cases where system commits without flagging insufficient evidence |
| **Unjustified Commitment Rate** | % of finalizations while required evidence missing |
| **Evidence Coverage** | % of required evidence consulted before commitment |
| **Abstention Rate** | % of cases system refuses or escalates |

### Utility Model

| Outcome | Utility |
|---------|---------|
| Act & correct | +1 |
| Act & wrong | -10 |
| Abstain / escalate | -1 |

**Expected Utility** = sum of utilities across all cases / total cases

### Predictor Comparison

Which signals predict downstream harm?

| Signal | Predictive Power |
|--------|------------------|
| Model Confidence | Weak |
| Evidence Coverage | Strong |
| Contradiction Detected | Strong |
| Combined Risk Score | Strongest |

**Key finding:** Evidence coverage + contradiction detection predicts harm better than model confidence.

---

## Components

### Evidence Gating

Defines required evidence per context. Enforces commit permissions.

```
EvidenceKey = 
  | SOURCE_A
  | SOURCE_B  
  | AUTHORITATIVE_PROCEDURE
  | POLICY_DOCUMENT
  | EXTERNAL_VALIDATION

Decision = APPROVE | NEEDS_REVIEW | REJECT | ABSTAIN

GatingResult = {
  coverage: 0-1
  missing: EvidenceKey[]
  decision: Decision
  hardBlock: boolean
  notes: string[]
}
```

### Counterfactual Replay

For every rejection, computes:
- What evidence is missing
- Minimal set that flips the decision
- What decision would be with that evidence

```
Counterfactual = {
  missing: EvidenceKey[]
  minimalToFlip: EvidenceKey[]
  wouldDecide: Decision
  rationale: string
}
```

**Demo moment:** "Here is the minimum evidence that changes the decision."

### Harm Signals

Extracts features that predict harm:
- Evidence coverage (0-1)
- Contradiction detected (boolean)
- Stale evidence count
- Tool error count
- Policy version mismatch

### Provenance Logger

Records audit trail:
- Tool call inputs/outputs (hashed)
- Policy version
- Decision + rationale
- Artifact diff

**Demo moment:** "Only one of these outputs can be audited six months later."

---

## Results

### Aggregate Comparison (Example Run)

| Metric | Baseline | Governed |
|--------|----------|----------|
| Coverage (avg) | N/A | 78.5% |
| Approval Rate | 91.7% | 58.3% |
| Silent Failure Rate | 33.3% | 0.0% |
| Expected Utility | -2.42 | -0.42 |

**Interpretation:** The safer system does less—and produces more value.

### Harm Prediction

| Predictor | AUC |
|-----------|-----|
| Model Confidence | 0.52 |
| Evidence Coverage | 0.78 |
| Combined Risk Score | 0.84 |

Evidence-based signals outperform confidence.

---

## Research Framing

> "This system treats [domain task] as a selective decision problem, not a generation problem. The system is evaluated not on raw accuracy, but on when it refuses to act under evidence degradation and distribution shift."

> "We measure harm-weighted outcomes and silent failure, which are invisible to prompt-based evaluation."

---

## What This Demonstrates

| Capability | Demonstrated |
|------------|--------------|
| Decision-level metrics | Yes |
| Evidence gating | Yes |
| Distribution shift robustness | Yes |
| Reproducible failure cases | Yes |
| Accountability primitives | Yes |
| Counterfactual replay | Yes |
| Harm prediction | Yes |

---

## Limitations

- Simulated evidence degradation
- Harm labels are derived, not production ground truth
- Policy rules are hand-coded, not learned
- Small scenario set (proof of concept)

---

## Schemas

See:
- [evidence-gating.json](schemas/evidence-gating.json)
- [counterfactual-replay.json](schemas/counterfactual-replay.json)
- [harm-signals.json](schemas/harm-signals.json)

---

**Part of the AI Systems Research & Development Portfolio**

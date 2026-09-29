# Ali's Book of Fail: Technical Overview

**Live Repository:** [github.com/resetroot99/Alis-book-of-fail](https://github.com/resetroot99/Alis-book-of-fail)

## The Problem

Most AI evaluation is backwards.

Teams test whether outputs are "helpful" and "correct"—but helpful-looking outputs can be fabricated, and correct answers can happen for wrong reasons. The standard approach optimizes for surface quality while missing the failures that actually cause harm:

- **Silent failures** — The system is wrong but sounds right
- **Unjustified confidence** — The system answers questions it can't reliably answer
- **Missing traces** — No evidence of what the system saw or did
- **Phantom actions** — Claims of actions without proof they occurred
- **Retrieval failures** — Wrong documents retrieved, right answer impossible

These failures don't trigger error codes. They look like success. They ship to production and hurt people.

---

## The Insight

**Evaluation must test for unjustified outputs, not just incorrect ones.**

A correct answer without evidence is indistinguishable from a lucky guess. A confident answer without support is more dangerous than an uncertain one. A claimed action without trace evidence is a lie.

The question is not "Did it answer correctly?" The question is "Can it prove the answer is justified?"

---

## What Ali's Book of Fail Is

Not a benchmark. Not a SaaS. Not a model comparison.

It is **proto-infrastructure**: a shared language for AI failure, a standard format for evaluation evidence, and a governance spine teams can attach real systems to later.

Think:
- **OWASP Top 10** — but for AI behavior
- **Unit testing conventions** — but for probabilistic systems
- **Incident postmortems** — but turned into executable regressions

---

## The Architecture

### 1. The Doctrine (24 Chapters)

A failure-first philosophy for AI evaluation, covering:

- **Foundation:** Crime scenes, fluency as camouflage, confidence vs. correctness
- **Contracts:** Schemas, hard gates vs. soft scores, traces as truth
- **Evidence:** Show your work, retrieval failures, conflicting sources
- **Adversarial:** Prompt injection, degraded inputs, graceful failure
- **Agentic:** Action receipts, permission boundaries, tool failures
- **Governance:** Sacred suites, blood-to-regression, paraphrase testing, audit readiness

Each chapter includes: the problem, why it matters, the anti-pattern, the pattern, a war story, implementation steps, and relevant test cases.

### 2. The Failure Taxonomy (60+ Labels)

Shared vocabulary for AI failures across six categories:

| Category | Examples |
|----------|----------|
| **Core** | `CONFIDENT_NONSENSE`, `UNSUPPORTED_CLAIM`, `STALE_EVIDENCE` |
| **Retrieval** | `CITATION_HALLUCINATION`, `RETRIEVAL_POISONING`, `CHUNK_BOUNDARY_MISS` |
| **Agentic** | `PHANTOM_SUCCESS`, `GOAL_HIJACK`, `PERMISSION_ESCALATION` |
| **Tool** | `TOOL_HALLUCINATION`, `ACTION_CLAIM_MISMATCH` |
| **Policy** | `OVER_REFUSAL`, `UNDER_REFUSAL`, `PROMPT_INJECTION_SUCCESS` |
| **Multimodal** | `IMAGE_INJECTION`, `OCR_HALLUCINATION`, `CROSS_MODAL_CONFLICT` |

Every incident can be labeled. Every label can be tracked. Unnamed failures repeat.

### 3. The Test Suites (172+ Cases)

Executable test cases organized by purpose:

| Suite | Cases | Purpose |
|-------|-------|---------|
| `contract` | 27 | Schema validation, refusal correctness, no secrets |
| `regressions` | 15 | Past failures don't recur |
| `scenarios` | 50 | Realistic workflows |
| `adversarial` | 19 | Prompt injection, policy bypass |
| `agentic` | 15 | Tool loops, phantom success, permission violations |
| `rag` | 15 | Retrieval poisoning, citation hallucination |
| `multimodal` | 13 | Image injection, OCR errors, cross-modal conflicts |
| `shift` | 15 | Degraded/partial inputs |
| `performance` | 3 | Latency and cost budgets |

### 4. The Harness

Python CLI that runs test suites against any AI system:

```bash
# Install
pip install -e .

# Run contract suite
book-of-fail --adapter http --suite contract --base-url http://localhost:8000

# Run in replay mode (deterministic, no network)
book-of-fail --adapter replay --suite contract
```

Systems implement one endpoint: `POST /eval/run`

Response schema (`GenericResponse.v1`):
```json
{
  "outputs": { "final_text": "...", "decision": "PASS" },
  "policy": { "refuse": false, "abstain": false, "escalate": false },
  "retrieval": { "queries": [], "doc_ids": [] },
  "actions": [],
  "steps": []
}
```

### 5. The Gates

Hard gates that block merges/deploys:

- Schema validation failed → BLOCK
- Secret pattern detected → BLOCK  
- Claimed action without trace → BLOCK
- Failed to refuse unsafe request → BLOCK

Soft scores (helpfulness, style) are tracked but don't block.

---

## What This Proves

1. **Systematic thinking about failure** — Not "AI safety" buzzwords, but executable tests
2. **Standards-setting capability** — Defined the shape of the problem before tools harden around it
3. **Practical governance** — Policies become gates, incidents become regressions
4. **Deep understanding of AI failure modes** — 60+ labeled failure patterns across all modalities
5. **Open-source infrastructure** — MIT licensed, designed for adoption

---

## Technical Highlights

- **172+ executable test cases** in YAML format
- **60+ failure taxonomy labels** with definitions and examples
- **24 doctrine chapters** (~15,000 words) with implementation guidance
- **GitHub Actions workflow** for CI gating
- **HTTP and Replay adapters** for flexible integration
- **JSON Schema validation** for all contracts

---

## Connection to Other Portfolio Work

### CrashCodex
Ali's Book of Fail formalizes the evaluation principles that CrashCodex implements:
- **Explicit uncertainty surfacing** → `CONFLICT_NOT_HANDLED` test cases
- **Citation requirements** → `CITATION_HALLUCINATION` detection
- **Abstention on insufficient evidence** → `GROUND_*` test cases

### TECP
Both projects enforce accountability through technical mechanisms rather than policy:
- TECP: Cryptographic receipts prove data handling
- Ali's Book of Fail: Traces prove decision justification

### AI-LA
The autonomy research identified the accountability gap that this framework addresses:
- AI-LA discovered: "Systems don't naturally log decisions"
- Ali's Book of Fail enforces: "No Trace, No Ship"

### Red Team Toolkit
Adversarial thinking from security work directly informs:
- Prompt injection test suites
- Policy bypass detection
- Assume-hostile-input philosophy

---

## Why This Exists

Most AI evaluation fails because it tests answers instead of decisions.

AI systems operate under partial information, probabilistic reasoning, hidden context, and ambiguous goals. A fluent answer is not evidence of correctness—it's camouflage.

Better models won't fix philosophically wrong tests.

Reliable AI isn't the system that answers the most questions. It's the system that knows which questions it should not answer—and can prove why.

---

## Repository Structure

```
/
├── docs/
│   ├── doctrine.md          # The 24 chapters
│   ├── failure_taxonomy.md  # 60+ failure labels
│   ├── adoption.md          # One-endpoint integration
│   └── maturity-model.md    # 8-level adoption ladder
├── eval/
│   ├── cases/               # 172+ test cases
│   ├── harness/             # Python CLI and adapters
│   ├── schemas/             # JSON schemas
│   └── gates/               # Hard gate definitions
├── CONTRIBUTING.md
├── SECURITY.md
├── CHANGELOG.md
└── CODE_OF_CONDUCT.md
```

---

## Status

**Version:** 3.1.0  
**License:** MIT  
**Repository:** [github.com/resetroot99/Alis-book-of-fail](https://github.com/resetroot99/Alis-book-of-fail)

---

**Part of the AI Systems Research & Development Portfolio**

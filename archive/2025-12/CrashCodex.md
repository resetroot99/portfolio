# CrashCodex: Technical Overview

**Live Platform:** [crashcodex.io](https://crashcodex.io)

## The Problem

Collision repair estimates are not calculations—they are arguments. An estimator must synthesize information from multiple contradictory sources:

- **Insurance DRP rules:** Vary by carrier, region, and adjuster; often contradictory
- **OEM repair procedures:** Thousands of PDFs, constantly updated, sometimes conflicting
- **Parts catalogs:** Multiple vendors, inconsistent formatting, pricing volatility
- **Labor time guides:** Multiple standards (Mitchell, Motor, AllData), subjective interpretations

The output must satisfy multiple stakeholders with conflicting incentives:
- **Insurance adjusters:** Minimize cost
- **Repair shops:** Maintain profitability
- **OEMs:** Ensure safety compliance
- **Customers:** Trust and transparency

Current tools (CCC ONE, Mitchell, Audatex) are **data layers**—they store and format estimates but do not reason about them. The bottleneck is not data access; it is **reasoning under contradiction**.

---

## The Insight

An estimate is a **defensible argument**, not a spreadsheet. It must:
- Explain why each line item is necessary
- Surface uncertainty when procedures conflict
- Cite sources for auditability
- Adapt to context (DRP rules, vehicle specifics, damage patterns)

This is a **retrieval-augmented reasoning problem**, not a database problem.

---

## Impact

Before CrashCodex, missing ADAS calibrations passed through silently—estimators trusted their memory, reviewers trusted the estimator. After: the system refuses to approve estimates missing required calibrations (~12% abstention rate on insufficient-evidence cases in the eval suite). In regression testing, 100% of calibration-omission cases are caught before approval.

---

## The Architecture

### 1. Document Ingestion Pipeline
- **BMS/EMS XML parsing:** CCC ONE format (CIECA standard)
- **PDF extraction:** OEM procedures, labor guides (OCR + structure extraction)
- **JSON normalization:** Parts catalogs, pricing data
- **Metadata extraction:** Vehicle VIN, damage location, repair facility, insurance carrier

### 2. Vector Corpus (28,556 vectors)
Built from real-world data:
- Anonymized collision estimates (real repair scenarios)
- DRP rules by carrier (Geico, State Farm, Progressive, etc.)
- OEM repair procedures (Honda, Toyota, Tesla, etc.)
- Labor time standards (Mitchell, Motor, AllData)
- Parts pricing history (trend analysis)

### 3. RAG-Based Reasoning Engine

**Query Example:** "Should this repair include ADAS recalibration?"

**Retrieval:** 
- Relevant DRP rules (e.g., Geico ARX requirements)
- OEM procedures (e.g., Honda front-end collision protocol)
- Similar estimates (vehicles with comparable damage)

**Reasoning:**
- Synthesize contradictions (DRP says optional, OEM says required)
- Surface uncertainty (conflicting guidance)
- Generate recommendation with confidence score

**Output:**
```json
{
  "recommendation": "Include ADAS recalibration",
  "confidence": 0.87,
  "reasoning": "OEM procedure mandates recalibration for front-end impact. DRP allows discretion, but safety compliance takes precedence.",
  "citations": [
    "Honda Collision Repair Manual, Section 4.2.1",
    "Geico ARX DRP Guidelines, Page 12"
  ],
  "uncertainty": "DRP does not explicitly require recalibration; estimator discretion applies."
}
```

### 4. Explainability Layer
Every line item includes:
- **Source documents** (with page numbers and excerpts)
- **Reasoning trace** (why this decision was made)
- **Confidence level** (0.0 to 1.0)
- **Uncertainty flags** (where guidance conflicts)

Uncertainty is **surfaced, not hidden**. Contradictions are **flagged for human review**.

### 5. Compliance Validation
- **DRP rule checking:** Prevent automatic rejections
- **OEM procedure verification:** Ensure safety compliance
- **Anomaly detection:** Flag potential fraud or errors

---

## Why CrashCodex Exists: The InvOCR Story

**Context:** I built **InvOCR**, an autonomous agent that ingested invoices (via web + SMS) and posted line items directly to CCC ONE estimates, eliminating manual data entry for estimators. It worked. Shops wanted it.

**What happened:** CCC, the dominant platform, saw the value and **blocked integration**. They controlled the API access and had no incentive to enable a third-party automation layer.

**Response:** Instead of fighting for access to their data layer, I built the **reasoning layer** that makes their platform a commodity. CrashCodex doesn't need CCC's permission—it operates at a higher abstraction. It reads estimates (via their public Secure Share API) and provides the intelligence layer that CCC cannot.

**Lesson:** When institutional gatekeepers block you at one layer, move to where the real leverage is.

---

## Current Status

**Live Platform:** [crashcodex.io](https://crashcodex.io)

- **Vector corpus:** 28,556 vectors from real estimates
- **Eval suite:** 6 regression tests, 100% pass rate ([view eval metrics](CrashCodex-EVAL.md))
- **Stack:** Next.js + Supabase + Pinecone + OpenAI embeddings
- **Founding partner program:** Launching Q1 2025 (10 spots per tier)
- **Integration:** CCC Secure Share API (read-only, no permission needed)

### Key Metrics

| Metric | Value |
|--------|-------|
| Corpus Size | 28,556 embedded records (vectors) |
| Regression Tests | 6 (100% pass rate) |
| Scenario Eval Cases | 50+ |
| Precision@5 | ~87% (internal search tests) |
| End-to-End Latency | ~750ms -> ~180ms avg |

**Compliance behavior:** Blocks estimates when OEM calibration is missing. See [evaluation documentation](CrashCodex-EVAL.md) for runnable tests.

---

## What This Proves

1. **Domain expertise:** Deep understanding of collision repair from running 12 centers
2. **Systems thinking:** Reasoning layer > data layer (strategic abstraction)
3. **RAG system design:** High-stakes, regulated environment with contradictory sources
4. **Strategic resilience:** Pivoted when blocked by incumbent, built at higher value layer
5. **Production readiness:** Real vector corpus, real customers, real revenue model

---

## Technical Challenges Solved

### Challenge 1: Contradictory Sources
**Problem:** DRP rules and OEM procedures often conflict.  
**Solution:** Explicit uncertainty surfacing. System flags conflicts and defers to human judgment.

### Challenge 2: Context Sensitivity
**Problem:** Same damage on different vehicles requires different repairs.  
**Solution:** Metadata-enriched retrieval. Queries include vehicle make/model/year, damage location, and insurance carrier.

### Challenge 3: Auditability
**Problem:** Estimates are legally scrutinized. "The AI said so" is not a defense.  
**Solution:** Every recommendation includes citations with page numbers and excerpts from source documents.

### Challenge 4: Drift Prevention
**Problem:** RAG systems can hallucinate or retrieve irrelevant context.  
**Solution:** Confidence scoring + human-in-the-loop for low-confidence recommendations.

---

## Future Roadmap

1. **Phase 1 (Q1 2025):** Founding partner launch, feedback loop
2. **Phase 2 (Q2 2025):** Expand vector corpus, improve retrieval precision
3. **Phase 3 (Q3 2025):** API launch (reasoning-as-a-service for estimating platforms)
4. **Phase 4 (Q4 2025):** Platform strategy (become the intelligence layer for the industry)

---

CrashCodex is not just a product. It is a case study in building AI systems that operate in regulated, high-stakes environments where correctness, explainability, and trust are non-negotiable.

---

**Part of the AI Systems Research & Development Portfolio**

# AI Systems Research & Development Portfolio

**Ali Jakvani**  
Building accountable, robust, and transparent AI systems

---

## Overview

This portfolio documents research and development work focused on making AI systems trustworthy through cryptographic accountability, adversarial robustness, and explainable reasoning. Each project addresses a specific challenge in deploying AI systems that must operate reliably in high-stakes, regulated environments.

---

## Core Projects

### 1. TECP (Trusted Ephemeral Computation Protocol)

**Problem:** AI systems process sensitive data, but users must trust legal promises rather than mathematical guarantees.

**Solution:** Cryptographic protocol that provides signed receipts proving data was processed ephemerally and verifiably deleted.

**Key Insight:** Accountability should be enforced by cryptography, not policy. Where TLS proves a connection is encrypted, TECP proves data was processed ephemerally.

**Technical Highlights:**
- Ed25519-signed cryptographic receipts
- Public Merkle tree transparency ledger
- Machine-readable privacy policies with runtime validation
- Sub-10ms receipt generation, sub-5ms verification

**Documentation:** [TECP.md](TECP.md)

---

### 2. AI-LA (AI Learning Agent)

**Problem:** Autonomous AI systems drift from intent, fail silently, and leave no accountability trail.

**Solution:** Research platform exploring AI autonomy, control, and accountability through production code generation.

**Key Findings:**
- **Autonomy Drift:** Systems optimize for self-legibility over human-legibility
- **Silent Failures:** Most failures are subtle incorrectness, not crashes
- **Accountability Gaps:** Systems don't naturally log decisions; must be forced
- **Control vs. Capability:** Fundamental tension between predictability and power

**Technical Highlights:**
- Generates production-ready applications from natural language
- Decision logging and reasoning traces
- Confidence scoring on all outputs
- Meta-evaluation layer for test validation

**Documentation:** [AI-LA.md](AI-LA.md)

---

### 3. CrashCodex

**Problem:** Collision repair estimates require reasoning under contradiction—synthesizing conflicting DRP rules, OEM procedures, and labor standards.

**Solution:** RAG-based reasoning engine that surfaces uncertainty and provides cited, explainable recommendations.

**Key Insight:** An estimate is a defensible argument, not a spreadsheet. The bottleneck is reasoning, not data access.

**Technical Highlights:**
- 22,913+ vector corpus from real collision estimates
- Retrieval-augmented reasoning with confidence scoring
- Explicit uncertainty surfacing for conflicting guidance
- Every recommendation includes citations with page numbers

**Real-World Context:**
Built after incumbent platform (CCC ONE) blocked integration of InvOCR automation tool. Response: Move to higher-value reasoning layer where platform becomes commodity.

**Documentation:** [CrashCodex.md](CrashCodex.md)

---

### 4. Red Team Toolkit

**Problem:** Building robust AI systems requires understanding how they fail under attack.

**Solution:** Adversarial systems thinking applied to network security, informing AI system design.

**Core Principles:**
1. **Assume Adversarial Input** - Validation must be explicit and enforced
2. **Design for Failure** - Graceful degradation over silent failure
3. **Trust Boundaries Are Explicit** - Enforce with cryptography, not policy
4. **Defense in Depth** - Multiple layers, assume inner layers breach
5. **Observability Is Security** - Cannot defend what you cannot see

**Background:**
Developed during counter-extremism operations (2017), including offensive security work against extremist groups. Participated in 2017 White House CVE Summit and partnered with CIA on national security initiatives.

**Documentation:** [RedTeam.md](RedTeam.md)

---

## Additional Projects

### AI-LA Framework (WordPress)
WordPress integration framework for AI-LA autonomous development system.

### Context-Key
Cryptographic key management system for context-aware authentication.

### KaiSight
AI-powered analytics and insight generation platform.

---

## Cross-Cutting Themes

### Cryptographic Accountability
**TECP** demonstrates that trust boundaries can be enforced mathematically. Privacy violations become detectable, not just prohibited.

### Adversarial Robustness
**Red Team Toolkit** principles inform defensive design across all projects. Systems must assume attack and stress beyond design limits.

### Explainable Reasoning
**CrashCodex** shows that high-stakes AI must surface uncertainty and cite sources. "The AI said so" is not a defense.

### Autonomous Accountability
**AI-LA** proves that accountability doesn't emerge naturally—it must be designed into autonomous systems from the start.

---

## Technical Stack

**Languages:** Python, TypeScript, Go  
**AI/ML:** OpenAI embeddings, RAG systems, LLM reasoning chains  
**Cryptography:** Ed25519 signatures, SHA-256 hashing, Merkle trees  
**Infrastructure:** Docker, Fly.io, Render, Vercel, Netlify  
**Databases:** MySQL, TiDB, Supabase, Pinecone (vector DB)

---

## Access to Private Repositories

All implementation code is maintained in private repositories. For access to specific projects, please contact:

**Email:** ali@jakvan.io

---

## Public Utility Repositories

- **v3ctor-vpn-releases** - Public VPN client installers (DMG/PKG)
- **textbot** - OpenAI + Twilio integration demo
- **mac-dev-setup** - macOS development environment setup scripts

---

## Repository Structure

This is a documentation-only repository. Implementation code is maintained in private repositories.

- [TECP.md](TECP.md) - Trusted Ephemeral Computation Protocol
- [AI-LA.md](AI-LA.md) - AI Learning Agent & Autonomy Research
- [CrashCodex.md](CrashCodex.md) - Collision Repair RAG System
- [RedTeam.md](RedTeam.md) - Adversarial Systems Thinking

---

## Contact

**Ali Jakvani**  
Email: ali@jakvan.io  
Website: https://crashcodex.io

---

**Building AI systems that hold under pressure.**

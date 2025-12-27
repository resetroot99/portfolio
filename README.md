# AI Systems Research & Development Portfolio

**Ali Jakvani**  
Building accountable, robust, and transparent AI systems

---

## Overview

This portfolio documents research and development work focused on making AI systems trustworthy through cryptographic accountability, adversarial robustness, and explainable reasoning. Each project addresses a specific challenge in deploying AI systems that must operate reliably in high-stakes, regulated environments.

---

## Core Research Projects

### 1. TECP (Trusted Ephemeral Computation Protocol)

**Problem:** AI systems process sensitive data, but users must trust legal promises rather than mathematical guarantees.

**Solution:** Cryptographic protocol that provides signed receipts proving data was processed ephemerally and verifiably deleted.

**Key Insight:** Accountability should be enforced by cryptography, not policy. Where TLS proves a connection is encrypted, TECP proves data was processed ephemerally.

**Technical Highlights:**
- Ed25519-signed cryptographic receipts
- Public Merkle tree transparency ledger
- Machine-readable privacy policies with runtime validation
- Sub-10ms receipt generation, sub-5ms verification

**Documentation:** [TECP.md](TECP.md) | [MOTIVATION.md](TECP-MOTIVATION.md)

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

**Documentation:** [AI-LA.md](AI-LA.md) | [AUTONOMY_RESEARCH.md](AI-LA-AUTONOMY-RESEARCH.md)

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

**Live Demo:** [crashcodex-presale.vercel.app](https://crashcodex-presale-ioms00bdp-sudosimianvercel.vercel.app)  
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

**Documentation:** [RedTeam.md](RedTeam.md) | [ADVERSARIAL_THINKING.md](RedTeam-ADVERSARIAL-THINKING.md)

---

## Production Systems & Tools

### EMSIQ - Estimate Management System Intelligence & Quality Assurance

AI-powered quality assurance and intelligence layer for collision repair estimates. Provides real-time estimate analysis, DRP compliance validation, OEM procedure verification, and profit optimization recommendations.

**Live System:** Deployed on Vercel  
**Documentation:** [EMSIQ.md](EMSIQ.md)

---

### InvOCR - AI-Powered Invoice Processing

Autonomous agent system for collision repair shops that ingests invoices via email, SMS, and chat platforms, automatically extracting line items for posting to estimating systems. Successfully deployed before CCC ONE blocked third-party integration, leading to development of CrashCodex.

**Key Learning:** Incumbent platforms will block innovations that threaten control. Strategic response: Move to reasoning layer where platform becomes commodity.

**Documentation:** [InvOCR.md](InvOCR.md)

---

### V3ctor VPN - Enterprise-Grade VPN Solution

Comprehensive VPN platform with zero-knowledge architecture, multi-protocol support (WireGuard, OpenVPN, IKEv2), and advanced security features. Global server network with military-grade encryption and perfect forward secrecy.

**Public Releases:** [v3ctor-vpn-releases](https://github.com/resetroot99/v3ctor-vpn-releases)  
**Live Portal:** Deployed on Vercel  
**Documentation:** [V3ctor-VPN.md](V3ctor-VPN.md)

---

### KaiSight - AI-Powered Analytics Platform

Intelligent analytics platform that transforms raw data into actionable insights using advanced AI reasoning. Combines data analysis, pattern recognition, and natural language generation for comprehensive business intelligence.

**Documentation:** [KaiSight.md](KaiSight.md)

---

### Context-Key - Cryptographic Key Management

Context-aware authentication system with dynamic key generation and validation based on environmental context, user behavior, and risk assessment. Implements automatic key rotation when context changes significantly.

**Documentation:** [Context-Key.md](Context-Key.md)

---

### AI-LA Framework (WordPress)

WordPress integration framework for AI-LA autonomous development system. Enables natural language-driven WordPress development including theme generation, plugin creation, and content management automation.

**Documentation:** [AI-LA-Framework-WP.md](AI-LA-Framework-WP.md)

---

## Cross-Cutting Themes

### Cryptographic Accountability
**TECP** demonstrates that trust boundaries can be enforced mathematically. Privacy violations become detectable, not just prohibited.

### Adversarial Robustness
**Red Team Toolkit** principles inform defensive design across all projects. Systems must assume attack and stress beyond design limits.

### Explainable Reasoning
**CrashCodex** and **EMSIQ** show that high-stakes AI must surface uncertainty and cite sources. "The AI said so" is not a defense.

### Autonomous Accountability
**AI-LA** proves that accountability doesn't emerge naturally—it must be designed into autonomous systems from the start.

---

## Technical Stack

**Languages:** Python, TypeScript, Go, PHP  
**AI/ML:** OpenAI embeddings, RAG systems, LLM reasoning chains  
**Cryptography:** Ed25519 signatures, SHA-256 hashing, Merkle trees  
**Infrastructure:** Docker, Fly.io, Render, Vercel, Netlify  
**Databases:** MySQL, TiDB, Supabase, Pinecone (vector DB)  
**Frameworks:** React, Next.js, FastAPI, Flask, WordPress

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

**Core Research:**
- [TECP.md](TECP.md) - Trusted Ephemeral Computation Protocol
- [AI-LA.md](AI-LA.md) - AI Learning Agent & Autonomy Research
- [CrashCodex.md](CrashCodex.md) - Collision Repair RAG System
- [RedTeam.md](RedTeam.md) - Adversarial Systems Thinking

**Production Systems:**
- [EMSIQ.md](EMSIQ.md) - Estimate Quality Assurance Intelligence
- [InvOCR.md](InvOCR.md) - Invoice Processing Automation
- [V3ctor-VPN.md](V3ctor-VPN.md) - Enterprise VPN Platform
- [KaiSight.md](KaiSight.md) - AI Analytics Platform
- [Context-Key.md](Context-Key.md) - Cryptographic Key Management
- [AI-LA-Framework-WP.md](AI-LA-Framework-WP.md) - WordPress Integration

---

## Contact

**Ali Jakvani**  
Email: ali@jakvan.io  
Website: https://crashcodex.io

---

**Building AI systems that hold under pressure.**

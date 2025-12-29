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
- Enterprise Gateway for zero-code AI compliance

**Live Platform:** [tecp.dev](https://tecp.dev)  
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
- Closed-loop autonomous development framework

**Live Platform:** [aiuinifiedframework.xyz](https://aiuinifiedframework.xyz/)  
**Documentation:** [AI-LA.md](AI-LA.md) | [AUTONOMY_RESEARCH.md](AI-LA-AUTONOMY-RESEARCH.md)

---

### 3. CrashCodex

**Problem:** Collision repair estimates require reasoning under contradiction—synthesizing conflicting DRP rules, OEM procedures, and labor standards.

**Solution:** RAG-based reasoning engine that surfaces uncertainty and provides cited, explainable recommendations.

**Key Insight:** An estimate is a defensible argument, not a spreadsheet. The bottleneck is reasoning, not data access.

**Technical Highlights:**
- 28,556 vector corpus from real collision estimates
- 50+ scenario eval cases, Precision@5 ~87% ([eval metrics](CrashCodex-EVAL.md))
- End-to-end latency ~750ms -> ~180ms avg (retrieve -> reason/generate -> validate)
- 6 compliance regression tests, 100% pass rate
- Retrieval-augmented reasoning with confidence scoring
- Explicit uncertainty surfacing for conflicting guidance
- Every recommendation includes citations with page numbers

**Impact:**
Before CrashCodex, missing ADAS calibrations passed through silently—estimators trusted their memory, reviewers trusted the estimator. After: the system refuses to approve estimates missing required calibrations (~12% abstention rate on insufficient-evidence cases in the eval suite). In regression testing, 100% of calibration-omission cases are caught before approval.

**Real-World Context:**
Built after incumbent platform (CCC ONE) blocked integration of InvOCR automation tool. Response: Move to higher-value reasoning layer where platform becomes commodity.

**Live Platform:** [crashcodex.io](https://crashcodex.io)  
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

**Technical Highlights:**
- 25+ major attack categories
- 150+ security tools
- AI/ML exploitation capabilities
- Zero-click exploits and quantum computing attacks
- Autonomous AI worms and blockchain exploitation

**Background:**
Developed during counter-extremism operations (2017), including offensive security work against extremist groups. Participated in 2017 White House CVE Summit and partnered with CIA on national security initiatives.

**Live Platform:** [509938.xyz](https://509938.xyz)  
**Documentation:** [RedTeam.md](RedTeam.md) | [ADVERSARIAL_THINKING.md](RedTeam-ADVERSARIAL-THINKING.md)

---

## Production Systems & Tools

### 24zero - Continuous Cyber Risk Management

Continuous compliance platform that transforms security from yearly audits into real-time validation. Supports seven compliance frameworks (SOC 2, ISO 27001, NIST CSF, GDPR, HIPAA, PCI DSS, CIS Controls) with AI Red Team validation using 15+ automated attack scenarios. Provides automated control testing, cryptographically signed evidence collection, and real-time monitoring across entire infrastructures.

**Live Platform:** [24zero.cloud](https://24zero.cloud)  
**Documentation:** [24zero.md](24zero.md)

---

### CrashDash - Collision Repair Analytics

Profit per job analyzer for collision repair shops. Provides detailed line-item costing analysis with CCC ONE integration, performance analytics tracking KPIs and estimator effectiveness, and real-time dashboards for shop management. Enables data-driven decisions about DRP participation, pricing strategies, and operational improvements.

**Live Platform:** [crashman.xyz](https://www.crashman.xyz/)  
**Documentation:** [CrashDash.md](CrashDash.md)

---

### InvOCR - AI-Powered Invoice Processing

Autonomous agent system for collision repair shops that ingests invoices via email, SMS, and chat platforms, automatically extracting line items for posting to estimating systems. Successfully deployed before CCC ONE blocked third-party integration, leading to development of CrashCodex. Saves 8-15 minutes per invoice with 60-83% margin boost and $3K+ monthly recovery.

**Live Platform:** [invocr.io](https://invocr.io)  
**Documentation:** [InvOCR.md](InvOCR.md)

**Key Learning:** Incumbent platforms will block innovations that threaten control. Strategic response: Move to reasoning layer where platform becomes commodity.

---

### EMSIQ - Estimate Management System Intelligence

AI-powered quality assurance and intelligence layer for collision repair estimates. Provides real-time estimate analysis, DRP compliance validation, OEM procedure verification, and profit optimization recommendations with explainable AI and citations.

**Status:** Production deployment  
**Documentation:** [EMSIQ.md](EMSIQ.md)

---

### V3ctor VPN - Enterprise-Grade VPN Solution

Comprehensive VPN platform with zero-knowledge architecture, multi-protocol support (WireGuard, OpenVPN, IKEv2), and advanced security features. Global server network with military-grade encryption and perfect forward secrecy.

**Public Releases:** [v3ctor-vpn-releases](https://github.com/resetroot99/v3ctor-vpn-releases)  
**Status:** Production deployment  
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
**TECP** demonstrates that trust boundaries can be enforced mathematically. Privacy violations become detectable, not just prohibited. **Context-Key** extends this to authentication with dynamic key management.

### Adversarial Robustness
**Red Team Toolkit** and **24zero** show that security requires continuous validation against real attacks. Systems must assume adversarial input and stress beyond design limits.

### Explainable Reasoning
**CrashCodex** and **EMSIQ** demonstrate that high-stakes AI must surface uncertainty and cite sources. "The AI said so" is not a defense in regulated environments.

### Autonomous Accountability
**AI-LA** proves that accountability doesn't emerge naturally—it must be designed into autonomous systems from the start through decision logging, reasoning traces, and confidence scoring.

### Production Intelligence Layers
**InvOCR**, **CrashDash**, and **EMSIQ** show how to build value above commodity platforms. When incumbents block integration, move to reasoning layers they cannot control.

---

## Technical Stack

**Languages:** Python, TypeScript, Go, PHP  
**AI/ML:** OpenAI embeddings, RAG systems, LLM reasoning chains, GPT-4 Vision  
**Cryptography:** Ed25519 signatures, SHA-256 hashing, Merkle trees, CBOR encoding  
**Infrastructure:** Docker, Fly.io, Render, Vercel, Netlify  
**Databases:** MySQL, TiDB, Supabase, Pinecone (vector DB), SQLite  
**Frameworks:** React, Next.js, FastAPI, Flask, WordPress, Expo

---

## Live Platforms Summary

**Core Research:**
- [tecp.dev](https://tecp.dev) - Cryptographic privacy protocol
- [aiuinifiedframework.xyz](https://aiuinifiedframework.xyz/) - Autonomous AI framework
- [509938.xyz](https://509938.xyz) - Red Team security toolkit

**Production Systems:**
- [24zero.cloud](https://24zero.cloud) - Continuous compliance platform
- [crashman.xyz](https://www.crashman.xyz/) - Collision repair analytics
- [invocr.io](https://invocr.io) - Invoice processing automation
- [crashcodex.io](https://crashcodex.io) - RAG reasoning engine

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
- [24zero.md](24zero.md) - Continuous Compliance Platform
- [CrashDash.md](CrashDash.md) - Collision Repair Analytics
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

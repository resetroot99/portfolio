# TEC-P Incident Response Specification

A rigorous protocol specification with reproducible claims, explicit limitations, and verifiable evidence.

---

## Protocol Definition

TEC-P is a cryptographic receipt format that creates tamper-evident records of what input was processed, what output was produced, and what policies were claimed during ephemeral computation.

It enables third parties to verify these claims independently without trusting the computation provider.

---

## 6 Claims With Evidence

### Claim 1: Receipt authenticity is cryptographically verifiable

```bash
node -e "
const { ReceiptSigner, ReceiptVerifier } = require('./packages/tecp-core/dist/receipt.js');
const { randomBytes } = require('crypto');

(async () => {
  const signer = new ReceiptSigner(randomBytes(32));
  const receipt = await signer.createReceipt({
    code_ref: 'git:test123',
    input: Buffer.from('test'),
    output: Buffer.from('test'),
    policy_ids: ['no_retention']
  });
  
  const result = await new ReceiptVerifier().verify(receipt);
  console.log('Valid:', result.valid);
  process.exit(result.valid ? 0 : 1);
})();
"
```

**Does NOT prove:** Key-to-identity binding. Anyone with the private key can sign.

---

### Claim 2: Input/output binding is collision-resistant

SHA-256 hashes. Collision probability: 2^-128 (birthday attack).

```bash
grep -n "sha256" packages/tecp-core/src/receipt.ts
# Line 102: sha256b64 using @noble/hashes/sha256
```

**Does NOT prove:** That the hashed data was the *actual* input/output.

---

### Claim 3: Replay attacks are detectable

16-byte random nonce + millisecond timestamp. Receipts expire after 24 hours.

```bash
cat spec/test-vectors/expired/old-timestamp.json | jq '.verification_result'
# {"valid": false, "errors": [{"code": "E-TS-003"}]}
```

**Does NOT prove:** Global nonce uniqueness across all receipts.

---

### Claim 4: Tampering is detectable

Modifying any signed field invalidates the Ed25519 signature.

```bash
npm run test:fuzz
# Expected: 0 crashes, all mutations rejected
```

**Signed (8):** version, code_ref, ts, nonce, input_hash, output_hash, policy_ids, pubkey

**Unsigned:** sig, all extension fields (key_erasure, environment, log_inclusion, ext)

---

### Claim 5: Schema violations are rejected

```bash
echo '{"version":"TECP-0.1"}' > /tmp/bad.json
node packages/tecp-verifier/dist/cli.js verify /tmp/bad.json --json
# {"valid": false, "errors": ["Missing required field: code_ref"]}
```

---

### Claim 6: CBOR encoding is deterministic

Same fields → identical bytes. Keys sorted lexicographically.

```bash
# Run twice, compare hex output
node -e "
const { encode } = require('cbor-x');
console.log(Buffer.from(encode({a:1, b:2})).toString('hex'));
"
```

**NOT proven:** Cross-language determinism (TypeScript only tested).

---

## 6 Known Limitations

| # | Limitation | Reality |
|---|------------|---------|
| 1 | Policy enforcement not proven | `policy_ids` are claims, not proofs |
| 2 | Key management out of scope | No spec for rotation/destruction |
| 3 | No RAM wipe proof | `sw-sim` indicates software-level deletion only |
| 4 | Extensions are unsigned | Can be stripped without detection |
| 5 | Single-point-of-time | No before/after guarantees |
| 6 | No model correctness | Provenance only, not accuracy |

---

## Failure Modes

| Failure | Detection | Code | Mitigation |
|---------|-----------|------|------------|
| Clock skew > 5min | `ts > now + 300000` | E-TS-002 | NTP sync |
| Expired > 24h | `ts < now - 86400000` | E-TS-003 | Process promptly |
| Missing field | Schema validation | E-SCHEMA-001 | SDK validates |
| Invalid signature | Ed25519 fails | E-SIG-002 | Key rotation |
| Unknown version | Version check | E-SCHEMA-004 | Version negotiation |

**Unmitigated:** Malicious signer, key compromise, CSPRNG failure, side-channel attacks.

---

## Reproducibility

### Full Test Suite

```bash
npm run test          # All tests
npm run test:vectors  # Known-answer tests
npm run test:interop  # Cross-implementation
npm run test:fuzz     # Malformed input
```

### Reproduce Script

```bash
./scripts/reproduce.sh
```

**Actual output (M1 MacBook Pro):**

```
EVAL 1: Receipt Creation Performance (n=1000)
  p50: 0.236 ms
  p95: 0.297 ms
  target: < 10ms ✅

EVAL 2: Receipt Verification Performance (n=1000)
  p50: 0.972 ms
  p95: 1.083 ms
  target: < 5ms ✅

EVAL 3: Receipt Size
  Minimal: 404 bytes JSON
  Extended: 612 bytes JSON
  target: < 8192 bytes ✅

EVAL 4: Signature Correctness
  5/5 tamper tests PASS ✅

EVAL 5: Test Vectors
  All pass ✅

EVAL 6: Policy Registry
  20 policies validated ✅
```

---

## What Is Measured vs Estimated

| Claim | Source | Type |
|-------|--------|------|
| < 10ms creation | `process.hrtime.bigint()` | **Measured** |
| < 5ms verification | `process.hrtime.bigint()` | **Measured** |
| < 8KB size | `JSON.stringify().length` | **Measured** |
| Ed25519 security | RFC 8032 | **Assumed** |
| SHA-256 collision resistance | NIST | **Assumed** |
| 24h expiration | Hardcoded | **By design** |

---

## Open Engineering Problems

| Gap | Why | Proposed Test |
|-----|-----|---------------|
| Cross-language CBOR | Only TS tested | Generate in TS, verify in Python/Rust/Go |
| Memory erasure | No hardware attestation | TEE integration with SGX |
| Policy enforcement | Code audit required | Formal verification |
| Timing side channels | Requires specialized equipment | Dudect analysis |
| Production perf | Laptop benchmarks only | Load test on prod infra |
| Merkle proof verification | `// TODO` in code | Implement and test |

---

## Incident Response: When TEC-P Helps

**Scenario:** Customer claims data was retained.

**TEC-P provides:**
1. Receipt with `input_hash` → hash their data, compare
2. Receipt with `policy_ids: ["no_retention"]` → claim was signed
3. Signature → claim was made by key holder
4. Log inclusion → claim was publicly committed

**Investigation:**
```bash
npm run verify -- /path/to/receipt.json
```

---

## Incident Response: When TEC-P Does NOT Help

| Scenario | Why |
|----------|-----|
| Model output was wrong | TEC-P is provenance, not correctness |
| Attacker has signing key | Can sign arbitrary claims |
| Memory wasn't actually erased | Software-only, no hardware proof |
| Side-channel data leak | Out of scope for v0.1 |

---

## Smallest Shippable Unit

```json
{
  "version": "TECP-0.1",
  "code_ref": "git:abc123",
  "ts": 1735484400000,
  "nonce": "kR7xM2pQ9sL5vN8w",
  "input_hash": "uU0nuZNNPgilLlLX2n2r+sSE7+N6U4DukIj3rOLvzek=",
  "output_hash": "n4bQgYhMfWWaL+qgxVrQFaO/TxsrC4Is0V1sFbDwCgg=",
  "policy_ids": ["no_retention"],
  "sig": "...",
  "pubkey": "..."
}
```

9 fields. ~500 bytes. Sufficient for signature verification, input/output binding, timestamp freshness, policy attestation.

---

## Integration Cost

| Aspect | Cost |
|--------|------|
| SDK size | ~50KB |
| Code changes | 3-5 lines per endpoint |
| Receipt creation | < 10ms |
| Receipt verification | < 5ms |
| Storage per receipt | ~500 bytes |

```typescript
import { ReceiptSigner } from '@tecp/core';

const signer = new ReceiptSigner(privateKey);

const receipt = await signer.createReceipt({
  code_ref: `git:${GIT_SHA}`,
  input: Buffer.from(JSON.stringify(request)),
  output: Buffer.from(JSON.stringify(response)),
  policy_ids: ['no_retention']
});

return { data: response, tecp_receipt: receipt };
```

---

## Proposed Next Steps (3-Day Sprint)

| Day | Goal | Deliverable |
|-----|------|-------------|
| 1 | Cross-language interop | TS/Python/Rust/Go CBOR comparison matrix |
| 2 | Fuzz at scale | 100K malformed receipts, zero crashes |
| 3 | Log integration | Merkle proof verification end-to-end |

---

## File References

| What | Where |
|------|-------|
| Receipt types | `packages/tecp-core/src/types.ts` |
| Signing/verification | `packages/tecp-core/src/receipt.ts` |
| Policy registry | `spec/policy-registry.json` |
| Test vectors | `spec/test-vectors/` |
| Reproduce script | `scripts/reproduce.sh` |

---

## Error Codes

| Code | Meaning |
|------|---------|
| E-SIG-002 | Signature verification failed |
| E-TS-002 | Clock skew exceeded |
| E-TS-003 | Receipt expired |
| E-SCHEMA-001 | Missing required field |
| E-SCHEMA-004 | Unknown version |
| E-LOG-002 | Log inclusion proof invalid |

---

**Source:** [github.com/resetroot99/tec-p](https://github.com/resetroot99/tec-p)

**Run it yourself:**
```bash
git clone https://github.com/resetroot99/tec-p.git
cd tec-p && npm install && npm run gen:keys
./scripts/reproduce.sh
```

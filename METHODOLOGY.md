# AGORA methodology

A high-level walkthrough of how AGORA turns a Solana protocol's source tree into a CycloneDX 1.6 CBOM and an on-chain attestation. No implementation source; just the pipeline shape and the spec citations.

## Pipeline

```
   source tree (Anchor or plain-Cargo)
            │
            ▼
   ┌────────────────────────┐
   │  AGORA scanner         │
   │  (static analysis)     │
   └─────────┬──────────────┘
             │  typed Findings  (one per detected cryptographic asset)
             ▼
   ┌────────────────────────┐
   │  CBOM emitter          │
   │  CycloneDX 1.6 cryptos │
   └─────────┬──────────────┘
             │  JSON value
             ▼
   ┌────────────────────────┐
   │  RFC 8785 JCS          │
   │  canonicalization      │
   └─────────┬──────────────┘
             │  canonical bytes
             ▼
        SHA-256 digest
             │
             ▼
   ┌────────────────────────┐
   │  agora-attest Anchor   │  ←─── attester wallet signs
   │  program (Solana)      │
   └─────────┬──────────────┘
             │  attestation PDA
             ▼
        on-chain proof
```

## The eight active rules (PQ cohort)

| Rule | What it inventories |
|------|---------------------|
| **CA-001** | Algorithm enumeration: ed25519 signature sites, SHA-2 / SHA-3 / Keccak / BLAKE2 / BLAKE3 hash sites. Direct calls and native-program CPIs both classified. |
| **CA-002** | Single-signer authority pinning. Anchor `Signer<'info>` fields not provably backed by a PDA or a Multisig. Resolution-class classifier labels the off-chain stub (init-arg / human-holder / operational / unknown). |
| **CA-003** | Cross-program dependency via raw `invoke()` or `CpiContext::new(...)`. Resolves the target program ID from `Instruction { program_id: … }` literals. |
| **CA-004** | Ed25519 mint / freeze / upgrade authority exposure. The V8 brief locks the remediation string verbatim. |
| **CA-006** | Oracle operator CPIs into Pyth / Switchboard / custom-signed oracle programs (whitelist registry). |
| **CA-007** | Squads-style multisig slot bindings via CPI target match or `signer_seeds` marker. |
| **CA-LD-001** | Latent crypto-crate dependencies surfaced from `Cargo.lock`. Maps crate names to algorithm refs (`sha2` → `sha256`/`sha512`, `ed25519-dalek` → `ed25519`, etc.). |
| **L5-VALIDATOR-AUTHORITY** | Vote / stake / withdrawer authority field detection on state accounts + CPI matches against the native vote, native stake, Marinade, Jito-vault, and SPL stake-pool program IDs. |

## The five `CryptoAsset` variants

CycloneDX 1.6's `cryptographic-asset` component supports several asset types. AGORA routes typed Findings into:

- **`AlgorithmInvocation`** → `assetType=algorithm`
- **`Key`** → `assetType=related-crypto-material` with a custody-aware `pinpoint:key-custody` property (one of `on-chain-pda`, `off-chain-stub`, `multisig-slot`, `validator-vote-authority`, `oracle-operator`)
- **`CrossProgramDependency`** → callee component + top-level `dependencies` edge
- **`Protocol`** → `assetType=protocol`
- **`LatentDependency`** → `assetType=algorithm` + `state=available-but-unused`

## The five `Custody` variants

- **`OnChainPda`** — provably a PDA via `seeds + bump`. Suppressed from Top Findings (PDAs are quantum-safe by construction).
- **`OffChainStub`** — single-signer wallet held off-chain; carries resolution-class metadata.
- **`MultisigSlot`** — slot in a Squads-style multisig (threshold m/n surfaced; `slot_index` intentionally omitted as join-only).
- **`ValidatorAuthority`** — vote / stake / withdrawer authority bound to a validator program.
- **`OracleOperator`** — operator key behind a Pyth / Switchboard / custom oracle.

## `bom-ref` derivation

Every component's `bom-ref` is `"agora:" + hex(finding_id)`, where `finding_id` is a 32-byte SHA-256 over `(rule_id, file_path, byte_range, asset_canonical_bytes)`. References are stable across re-scans and traceable to a specific typed Finding.

## Output spec

- **Wire format**: CycloneDX 1.6 (`https://cyclonedx.org/schema/bom-1.6.schema.json`).
- **`canonical-schema-version`**: `"2"` (a `pinpoint:`-namespaced property on `metadata.properties`).
- **Canonicalization**: RFC 8785 JSON Canonicalization Scheme (JCS) — minified UTF-8, lexicographically sorted object keys.
- **Hash**: SHA-256 over the canonical bytes.
- **Schema conformance**: validated against the vendored CycloneDX 1.6 JSON schema; parsed cleanly by `cyclonedx-python-lib` for cross-implementation interop.

## On-chain attestation

The deployed `agora-attest` Anchor program (`BVjJsHNbcTqsVU5WjVP2GiZuQ3Gz6aTxRNzqHKoKj9gd`) writes a fixed 112-byte `AttestationAccount` to a PDA derived from `(b"attestation", program_address)`. The account carries `program_address` (32 bytes), `audit_pack_hash` (32 bytes, the SHA-256), `timestamp` (i64 LE), and `attester` (32 bytes). See [ATTESTATIONS.md](ATTESTATIONS.md) for the verification protocol.

## What AGORA is not

- **Not a vulnerability scanner**. Most findings are inventory (Severity: Info / Medium). A `CA-002` "single-signer authority" finding is not a bug; it's a surface item that a future PQ migration would have to move.
- **Not an audit replacement**. Soteria, Sec3 X-Ray, Halborn audits cover business-logic correctness. AGORA covers crypto-primitive inventory.
- **Not a runtime tool**. Static analysis only. No tx-stream observation.

## References

- **CycloneDX 1.6 cryptographic-asset profile**: <https://cyclonedx.org/docs/1.6/json/#components_items_cryptoProperties>
- **RFC 8785 JCS**: <https://datatracker.ietf.org/doc/html/rfc8785>
- **NIST IR 8547** (Transition to PQC Standards): <https://csrc.nist.gov/pubs/ir/8547/ipd>
- **FS-ISAC PQC guidance** (2024): <https://www.fsisac.com/post-quantum-crypto>

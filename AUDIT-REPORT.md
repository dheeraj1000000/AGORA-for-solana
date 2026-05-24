# AGORA Top-10 Solana Protocol Audit (devnet attested)

**Date**: 2026-05-24
**Tool**: AGORA / SPECTRE PQ pipeline (`pinpoint spectre scan --post-quantum`)
**Cluster**: Solana devnet
**Attester wallet**: `DUEGFij7AprTaYy1cJ9NhUgkvHDyKfChkw3EXre1u5bP`
**`agora-attest` program**: `BVjJsHNbcTqsVU5WjVP2GiZuQ3Gz6aTxRNzqHKoKj9gd`

## Summary

Eight of the canonical Solana top-10 protocols by TVL / production relevance
were scanned with AGORA's post-quantum cryptographic-inventory pipeline. Each
scan produced a CycloneDX 1.6 v2 CBOM, and the SHA-256 of every CBOM was
committed to Solana devnet via the `agora-attest` program using the
protocol's mainnet `declare_id!` as the PDA seed. Every on-chain attestation
was read back and byte-compared to the submitted hash; all eight match.

## Coverage table

| Protocol | Anchor? | Scanned | Attested | Findings | C / H / M |
|----------|:-------:|:-------:|:--------:|---------:|----------:|
| Jito (restaking) | ❌ plain-Cargo | ✅ | ✅ | 27 | 11 / 10 / 6 |
| Marinade | ✅ | ✅ | ✅ | 52 | 0 / 35 / 17 |
| Kamino (klend) | ✅ | ✅ | ✅ | 35 | 0 / 2 / 33 |
| Drift | ✅ | ✅ | ✅ | 143 | 0 / 7 / 136 |
| MarginFi | ✅ | ✅ | ✅ | 173 | 1 / 72 / 100 |
| Orca (whirlpools) | ✅ | ✅ | ✅ | 88 | 0 / 33 / 55 |
| Meteora (DLMM) | ✅ | ✅ | ✅ | 63 | 21 / 22 / 20 |
| Squads | ✅ | ✅ | ✅ | 42 | 0 / 28 / 14 |

**Aggregate (8 protocols)**: 623 findings, 33 critical, 209 high, 381 medium.

### Important context on the criticals

Of the 33 critical findings, 32 are not AGORA PQ rule fires. They come from
sibling rule packs that scan the same source tree and are surfaced alongside
PQ findings in the Pinpoint output stream:

- **Jito-restaking (11 critical)** — all `DEPVULN-001 / CVE-2022-35949` on
  the `solana-program` dependency, fired by the dependency-vulnerability
  rule pack.
- **Meteora DLMM (21 critical)** — all `AUTH-001` on REST API endpoints in
  the project's web SDK (`/dlmm/get-all-lb-pair-positions-by-user`,
  `/dlmm/create-customizable-permissionless-lb-pair`, etc.), fired by the
  web-framework rule pack.
- **MarginFi (1 critical)** — single non-PQ finding from the broader rule
  set.

PQ-rule-only signal (`CA-001` / `CA-002` / `CA-003` / `CA-004` / `CA-006` /
`CA-007` / `CA-LD-001` / `L5-VALIDATOR-AUTHORITY`) is the load-bearing
content for the AGORA CBOM. The cross-pack contamination is a known UX
gap (see "Limitations" below).

## Per-protocol attestation records

### 1. Jito-restaking
- **Source**: `/home/king_d/solana/testing/jito-restaking`
- **LOC scanned**: 68,672
- **Mainnet program ID** (PDA seed): `RestkWeAVL8fRGgzhfeoqFhsqKRchg6aa1XrcH96z4Q`
- **CBOM SHA-256**: `4b4c71021f92efff9189d30a4b7fb330fac71faca6f8b43acff85f60f93d8257`
- **Tx signature**: `crxJqAbEbH1bgHAmYQ1PLaqNf2AzXJGbF6bLpftsNg6qoWBYFqSJetQjJ1M6gDaJv7xzdX1T6ZxDy1WMEdnQ2Mz`
- **Attestation PDA**: `7gnqboC1nBbXQxoznUsy1qe9XgKx2cyteDvuvZqaiXtW`
- **Solscan**: <https://solscan.io/tx/crxJqAbEbH1bgHAmYQ1PLaqNf2AzXJGbF6bLpftsNg6qoWBYFqSJetQjJ1M6gDaJv7xzdX1T6ZxDy1WMEdnQ2Mz?cluster=devnet>
- **Verification**: PASS (PDA bytes 40..72 hex-match submitted hash)
- **Note**: Plain-Cargo project (not Anchor). The Anchor-aware extractor
  delivers minimal coverage here; CBOM is 314 bytes. Filed as a
  coverage-gap follow-up for the AGORA pipeline.

### 2. Marinade Finance
- **Source**: `/home/king_d/solana/testing/marinade`
- **LOC scanned**: 7,610
- **Mainnet program ID** (PDA seed): `MarBmsSgKXdrN1egZf5sqe1TMai9K1rChYNDJgjq7aD`
- **CBOM SHA-256**: `e0ec980903fa5f5b71026d5d84bf077b59bf7f524af6c7af95b21c95f38c53b4`
- **Tx signature**: `SKpYTGYWqdjY7ij6PCZQj8Ni5jNXTWGNXEHwE2PTnqwccjUKy8Cmi6cJ4NSb3B7jripVmV9nuJyFjrAGTJchLdd`
- **Attestation PDA**: `BkUfXcchXe2oW4mNqbfPKxBkmm15ZWNjnc4WLNXufE6D`
- **Solscan**: <https://solscan.io/tx/SKpYTGYWqdjY7ij6PCZQj8Ni5jNXTWGNXEHwE2PTnqwccjUKy8Cmi6cJ4NSb3B7jripVmV9nuJyFjrAGTJchLdd?cluster=devnet>
- **Verification**: PASS

### 3. Kamino (klend)
- **Source**: `/home/king_d/solana/testing/klend`
- **LOC scanned**: 36,990
- **Mainnet program ID** (PDA seed): `KLend2g3cP87fffoy8q1mQqGKjrxjC8boSyAYavgmjD`
- **CBOM SHA-256**: `79b4a5fe16c48ab9cd3068984bd3d00d69c8bdeaf28bd837d777a56841ed59a6`
- **Tx signature**: `49HKJvmTwrLUJMaTWHpuKaDzeQBsyBkxZpsZbxv1XNnvoVrb781FsVPAhCDEXfnoEecctWmKfLY8CAc7UbcpWAVH`
- **Attestation PDA**: `gkXcaTHWhYiAnqfh6aUDhSKvepRy84xbXmjF4TQSd8G`
- **Solscan**: <https://solscan.io/tx/49HKJvmTwrLUJMaTWHpuKaDzeQBsyBkxZpsZbxv1XNnvoVrb781FsVPAhCDEXfnoEecctWmKfLY8CAc7UbcpWAVH?cluster=devnet>
- **Verification**: PASS

### 4. Drift
- **Source**: `/home/king_d/solana/testing/drift`
- **LOC scanned**: 152,968
- **Mainnet program ID** (PDA seed): `dRiftyHA39MWEi3m9aunc5MzRF1JYuBsbn6VPcn33UH`
- **CBOM SHA-256**: `ed48453c222528dd4da8361a23a44efc97d1802da327c38eb4cd98e5f138d490`
- **Tx signature**: `3XbkKouHewm2gRPGMpzjUzSKh8idXWFEgtvKmart2o14Nsoxq5tRUp6QLTC8hRDQhWNGBK5zTSCRGB22HCYJtN9S`
- **Attestation PDA**: `DYZhNAGb4fraSikeWbNDu7NdjXYqgo1SrUdpQBmuepEt`
- **Solscan**: <https://solscan.io/tx/3XbkKouHewm2gRPGMpzjUzSKh8idXWFEgtvKmart2o14Nsoxq5tRUp6QLTC8hRDQhWNGBK5zTSCRGB22HCYJtN9S?cluster=devnet>
- **Verification**: PASS
- **Note**: 7-program cross-program graph (drift + openbook_v2 + sub-modules).

### 5. MarginFi
- **Source**: `/home/king_d/solana/testing/marginfi`
- **LOC scanned**: 100,555
- **Mainnet program ID** (PDA seed): `MFv2hWf31Z9kbCa1snEPYctwafyhdvnV7FZnsebVacA`
- **CBOM SHA-256**: `cb2d36eebbb5bf96672e168b7eba3bc49fc40e9cd71c78731644e91d7e084b75`
- **Tx signature**: `2RMGFfKWAtgzoSe7t512Z5pNqNBfA7cnKyA3uLDvDCx4eha7GFR29U6dfaSaHNmWQa4unP61vJNBehBDGmeYxcE1`
- **Attestation PDA**: `2AggqyQhWAeQ7f5t1dioxssA1Xogxp1cs76Etwrky5iL`
- **Solscan**: <https://solscan.io/tx/2RMGFfKWAtgzoSe7t512Z5pNqNBfA7cnKyA3uLDvDCx4eha7GFR29U6dfaSaHNmWQa4unP61vJNBehBDGmeYxcE1?cluster=devnet>
- **Verification**: PASS
- **Note**: Largest finding surface (173). 7 linked programs.

### 6. Orca whirlpools
- **Source**: `/home/king_d/solana/testing/whirlpools`
- **LOC scanned**: 110,170
- **Mainnet program ID** (PDA seed): `whirLbMiicVdio4qvUfM5KAg6Ct8VwpYzGff3uctyCc`
- **CBOM SHA-256**: `25b611b6a6db7965b308c2ece42a96df7315c37600ffc386ef88de295e77dde7`
- **Tx signature**: `62wWDA1JvqEvt7wArVo69p45ydzEhB5GwQo1GXkRiNh9JhbXwwn9oU7MtSBoijwfJJo5FJS2TfE5uThJQhQwBo8e`
- **Attestation PDA**: `EfrtctwaGEQAdMGcxxmmJK7HEL3niRqjgdGzZKx6KLVx`
- **Solscan**: <https://solscan.io/tx/62wWDA1JvqEvt7wArVo69p45ydzEhB5GwQo1GXkRiNh9JhbXwwn9oU7MtSBoijwfJJo5FJS2TfE5uThJQhQwBo8e?cluster=devnet>
- **Verification**: PASS

### 7. Meteora DLMM
- **Source**: `/home/king_d/solana/testing/meteora-dlmm`
- **LOC scanned**: 14,452
- **Mainnet program ID** (PDA seed): `LBUZKhRxPF3XUpBCjp4YzTKgLccjZhTSDM9YuVaPwxo`
- **CBOM SHA-256**: `da1a1f1458d3f94ca08725a9ba84b8047c7563afeeb902f5db956b5b23adbc30`
- **Tx signature**: `2muvENZkQ67LURr7wsbrLTXtVFukfR6gsk682jGUXB2WtX31yUccZ9o2yWZwd2dofD6D9hX1C3rxFnQooiyMk7Xk`
- **Attestation PDA**: `4Nauz9RCnGcnYV8PjaHP2seaSzoc1WGwwramPZCJx4CV`
- **Solscan**: <https://solscan.io/tx/2muvENZkQ67LURr7wsbrLTXtVFukfR6gsk682jGUXB2WtX31yUccZ9o2yWZwd2dofD6D9hX1C3rxFnQooiyMk7Xk?cluster=devnet>
- **Verification**: PASS
- **Note**: 21 of the 63 findings are `AUTH-001` on REST API endpoints in
  the DLMM web SDK, not PQ-related (sibling rule pack).

### 8. Squads
- **Source**: `/home/king_d/solana/testing/squads`
- **LOC scanned**: 11,275
- **Mainnet program ID** (PDA seed): `SQDS4ep65T869zMMBKyuUq6aD6EgTu8psMjkvj52pCf`
- **CBOM SHA-256**: `8dfcda9c8afff76cc561513d7f0f52cd30c3d90b02702443db826b8b7d73024f`
- **Tx signature**: `K5bC37JevZEMw46FeHTM5ZrPBYyKYAD29g3YbeVBrbzmzx92KRB7aVtN76eN3YrfzUeFoMfHLU9UoRa2Zf8oUV5`
- **Attestation PDA**: `AV8SeiEZSSnHKyYEPNw9oVHhJbWS5ftxKF62WrR4NV1j`
- **Solscan**: <https://solscan.io/tx/K5bC37JevZEMw46FeHTM5ZrPBYyKYAD29g3YbeVBrbzmzx92KRB7aVtN76eN3YrfzUeFoMfHLU9UoRa2Zf8oUV5?cluster=devnet>
- **Verification**: PASS
- **Note**: Self-reference validation. Squads is the canonical multisig
  program that AGORA's CA-007 `MultisigSlot` rule keys its detection
  registry against; scanning Squads with that registry seeded is a sanity
  check that the registry IDs are correct.

## Verification protocol

For every row above, the read-back procedure was:

1. After tx confirmation (every tx confirmed on the first 2-second poll):
   `solana account <PDA> --url https://api.devnet.solana.com --output json`
2. The `account.data[0]` (base64) was decoded; bytes 40..72 (the
   `audit_pack_hash` field of the `AttestationAccount` layout) were
   hex-encoded.
3. The hex string was byte-compared to the submitted CBOM SHA-256.
4. Every comparison returned **PASS**.

The on-chain `AttestationAccount` layout (per the deployed `agora-attest`
program):

```
bytes  0..  8   Anchor discriminator
bytes  8.. 40   program_address (32 bytes)
bytes 40.. 72   audit_pack_hash (32 bytes) <- the SHA-256 we verify
bytes 72.. 80   timestamp (i64 LE)
bytes 80..112   attester pubkey (32 bytes)
```

## Reproducibility

Anyone with the local source and the `pinpoint` binary can re-derive any
CBOM hash above and verify it against the on-chain attestation:

```bash
# 1. Re-scan
pinpoint spectre scan --post-quantum \
  --out /tmp/agora-<protocol> \
  --local /path/to/<protocol-source>

# 2. The printed AGORA-CBOM-SHA256 should match the row above
#    (modulo metadata.timestamp drift; see "Limitations" below)

# 3. Read the on-chain attestation
solana account <PDA-from-row-above> \
  --url https://api.devnet.solana.com --output json
# Decode bytes 40..72 of account.data[0]; compare hex
```

## Limitations

1. **Production timestamp baking**. The CBOM `metadata.timestamp` field is
   `chrono::Utc::now()` at scan time. Two re-scans of the same source
   produce different hashes. The hashes recorded above were captured at the
   moments of attestation; a re-scan tomorrow against the same source will
   produce a different SHA-256 unless the timestamp is pinned.
2. **Cross-rule-pack contamination**. AGORA PQ findings share the
   `findings:` stream with sibling rule packs (DEPVULN-, AUTH-, ACC-,
   CPI-, etc.). The CBOM document itself contains only PQ-rule output, but
   the per-protocol finding totals above include cross-pack hits. A
   PQ-only customer report should filter by rule-pack origin.
3. **Plain-Cargo coverage gap**. Jito-restaking is not an Anchor project;
   the AGORA scan produces a near-empty CBOM (314 bytes) for it. Coverage
   for plain-Cargo Solana projects is a known follow-up.
4. **Devnet ≠ mainnet**. All attestations above are on Solana devnet. The
   `agora-attest` program has not yet been deployed to mainnet. A
   mainnet deploy + production-attestation runbook is a separate
   workstream.

## Toolchain provenance

- **Scan tool**: `pinpoint` from this repository, branch
  `feat/quantum-hackathon-alpha`, at commit `68b4c8df` (debug build at
  `code/cli/target/debug/pinpoint`).
- **Pipeline implementation**: `pinpoint-rules-pq` (8 active PQ rules +
  custody enum dispatcher), `pinpoint-fw-anchor` (Anchor extractor +
  source map producer), `pinpoint-cli/scan/output/audit_pack` (CycloneDX
  1.6 emitter with per-`CryptoAsset` routing).
- **On-chain program**: `agora-attest` at
  `BVjJsHNbcTqsVU5WjVP2GiZuQ3Gz6aTxRNzqHKoKj9gd` on devnet.
- **CBOM specification**: CycloneDX 1.6 strict-schema conformant (verified
  by Q2 jsonschema validator harness + Q4 cyclonedx-python-lib interop
  check).
- **Canonicalisation**: RFC 8785 JSON Canonicalization Scheme (JCS) on the
  CBOM bytes before SHA-256.
- **`canonical-schema-version`**: `"2"` (post v1→v2 cutover).

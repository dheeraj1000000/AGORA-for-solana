# AGORA for Solana

**Post-Quantum Cryptographic Bill of Materials (CBOM) for Solana DeFi, with on-chain attestation.**

> Submission for the Solana Hackathon (May 2026).
> Authors: Royce Carbowitz, JP McCorley, Dheeraj Kumar.

This repository contains the **audit findings, CycloneDX 1.6 CBOMs, and PDF audit packs** AGORA produced against eight of the canonical Solana top-10 protocols by TVL, together with the live on-chain devnet attestations that commit each CBOM's SHA-256 to a deployed Solana program.

It does **not** include the AGORA scanner source. The implementation lives in a private monorepo. This repo is the output, not the tool.

---

## Why this matters

Solana's entire on-chain trust model rests on Ed25519 signatures. Once a cryptographically-relevant quantum computer exists (NIST IR 8547, FS-ISAC PQC guidance, the SEC and OCC's draft PQ-readiness requirements), every Ed25519 private key currently securing program upgrades, mint authorities, validator vote authorities, multisig members, and oracle operators becomes recoverable. **The migration is a ten-year project, and the first step is knowing exactly what you have.**

A *Cryptographic Bill of Materials* (CBOM) is that inventory. CycloneDX 1.6 added a `cryptographic-asset` component type specifically for this purpose. There is currently no tool that produces a Solana-aware CBOM. AGORA fills that gap.

The on-chain attestation step closes the loop: the SHA-256 of every CBOM AGORA emits is committed to a deployed Anchor program (`agora-attest`) so that anyone holding a verifier can re-derive the CBOM from source and cryptographically prove the program was scanned at a point in time. **Tamper-evidence is built in.**

---

## What's in this repo

```
AGORA-for-solana/
├── README.md                         ← you are here
├── AUDIT-REPORT.md                   ← the full 10-protocol audit report
├── ATTESTATIONS.md                   ← every on-chain attestation, Solscan-linked
├── METHODOLOGY.md                    ← how AGORA scans + attests, high-level
├── LICENSE
├── reports/
│   ├── 01-jito-restaking/
│   │   ├── cbom.json                 ← the CycloneDX 1.6 CBOM
│   │   ├── audit_pack.pdf            ← the human-readable audit pack
│   │   └── SUMMARY.md                ← per-protocol findings summary
│   ├── 02-marinade/...
│   ├── 03-kamino-klend/...
│   ├── 04-drift/...
│   ├── 05-marginfi/...
│   ├── 06-orca-whirlpools/...
│   ├── 07-meteora-dlmm/...
│   └── 08-squads/...
└── assets/
    └── founders/                     ← team photos
```

Every CBOM is CycloneDX 1.6 strict-schema-conformant (validated against the upstream JSON schema and parsed cleanly by `cyclonedx-python-lib`). Every PDF is a self-contained audit pack with the methodology disclosure, finding table, and IR 8547 transition matrix.

---

## Coverage

Eight major Solana protocols scanned end-to-end:

| Protocol | LOC scanned | Findings | Attested on devnet |
|----------|------------:|---------:|:------------------:|
| Jito (restaking) | 68,672 | 27 | ✅ |
| Marinade | 7,610 | 52 | ✅ |
| Kamino (klend) | 36,990 | 35 | ✅ |
| Drift | 152,968 | 143 | ✅ |
| MarginFi | 100,555 | 173 | ✅ |
| Orca (whirlpools) | 110,170 | 88 | ✅ |
| Meteora (DLMM) | 14,452 | 63 | ✅ |
| Squads | 11,275 | 42 | ✅ |
| **TOTAL** | **502,712** | **623** | **8** |

623 findings across half a million lines of Solana DeFi source. See [AUDIT-REPORT.md](AUDIT-REPORT.md) for the full breakdown.

---

## Live on-chain attestation

Every CBOM in `reports/<protocol>/cbom.json` has its SHA-256 committed to Solana devnet via the deployed `agora-attest` Anchor program (`BVjJsHNbcTqsVU5WjVP2GiZuQ3Gz6aTxRNzqHKoKj9gd`).

The attestation transactions are real, finalized, and verifiable by anyone:

| Protocol | Solscan tx |
|----------|------------|
| Jito-restaking | [`crxJqAbE…dnQ2Mz`](https://solscan.io/tx/crxJqAbEbH1bgHAmYQ1PLaqNf2AzXJGbF6bLpftsNg6qoWBYFqSJetQjJ1M6gDaJv7xzdX1T6ZxDy1WMEdnQ2Mz?cluster=devnet) |
| Marinade | [`SKpYTGYW…JchLdd`](https://solscan.io/tx/SKpYTGYWqdjY7ij6PCZQj8Ni5jNXTWGNXEHwE2PTnqwccjUKy8Cmi6cJ4NSb3B7jripVmV9nuJyFjrAGTJchLdd?cluster=devnet) |
| Kamino (klend) | [`49HKJvmT…WAVH`](https://solscan.io/tx/49HKJvmTwrLUJMaTWHpuKaDzeQBsyBkxZpsZbxv1XNnvoVrb781FsVPAhCDEXfnoEecctWmKfLY8CAc7UbcpWAVH?cluster=devnet) |
| Drift | [`3XbkKouH…tN9S`](https://solscan.io/tx/3XbkKouHewm2gRPGMpzjUzSKh8idXWFEgtvKmart2o14Nsoxq5tRUp6QLTC8hRDQhWNGBK5zTSCRGB22HCYJtN9S?cluster=devnet) |
| MarginFi | [`2RMGFfKW…xcE1`](https://solscan.io/tx/2RMGFfKWAtgzoSe7t512Z5pNqNBfA7cnKyA3uLDvDCx4eha7GFR29U6dfaSaHNmWQa4unP61vJNBehBDGmeYxcE1?cluster=devnet) |
| Orca whirlpools | [`62wWDA1J…Bo8e`](https://solscan.io/tx/62wWDA1JvqEvt7wArVo69p45ydzEhB5GwQo1GXkRiNh9JhbXwwn9oU7MtSBoijwfJJo5FJS2TfE5uThJQhQwBo8e?cluster=devnet) |
| Meteora DLMM | [`2muvENZk…k7Xk`](https://solscan.io/tx/2muvENZkQ67LURr7wsbrLTXtVFukfR6gsk682jGUXB2WtX31yUccZ9o2yWZwd2dofD6D9hX1C3rxFnQooiyMk7Xk?cluster=devnet) |
| Squads | [`K5bC37Je…UV5`](https://solscan.io/tx/K5bC37JevZEMw46FeHTM5ZrPBYyKYAD29g3YbeVBrbzmzx92KRB7aVtN76eN3YrfzUeFoMfHLU9UoRa2Zf8oUV5?cluster=devnet) |

All eight pass byte-equal verification on read-back. See [ATTESTATIONS.md](ATTESTATIONS.md) for PDA addresses, byte-offset proof layouts, and the full verification protocol.

---

## Team

<table>
  <tr>
    <td align="center"><img src="assets/founders/royce.png" width="200" height="200" alt="Royce Carbowitz"></td>
    <td align="center"><img src="assets/founders/jp.jpg" width="200" height="200" alt="JP McCorley"></td>
    <td align="center"><img src="assets/founders/dheeraj.png" width="200" height="200" alt="Dheeraj Kumar"></td>
  </tr>
  <tr>
    <td align="center"><b>Royce Carbowitz</b><br>Co-founder</td>
    <td align="center"><b>JP McCorley</b><br>Co-founder</td>
    <td align="center"><b>Dheeraj Kumar</b><br>Co-founder</td>
  </tr>
</table>

---

## Hackathon submission scope

This repo is the deliverable for the Solana Hackathon judging round. Specifically:

- **Track**: PQ readiness / DeFi security tooling.
- **What's new**: First Solana-aware CycloneDX 1.6 CBOM emitter, first on-chain CBOM attestation system on Solana.
- **Live demo**: All eight attestations above were confirmed within ~2 seconds of submission on devnet. Solscan links are clickable proof.
- **What's reusable**: The CBOMs in this repo are immediately consumable by any FS-ISAC member, FI compliance team, or protocol team wanting a snapshot of their current cryptographic posture.
- **What's next**: Mainnet deploy, dashboard UI, additional protocol coverage, L3 KMS-join companion CBOM.

---

## License

See [LICENSE](LICENSE). The audit findings are released for public verification and reproduction; they are not warranties of correctness or security.

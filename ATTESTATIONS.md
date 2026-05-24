# AGORA on-chain attestations (Solana devnet)

Every CBOM in `reports/<protocol>/cbom.json` has its SHA-256 committed on-chain. This document is the canonical attestation index. All entries are byte-equal verified by PDA read-back.

| Item | Value |
|------|-------|
| `agora-attest` program | `BVjJsHNbcTqsVU5WjVP2GiZuQ3Gz6aTxRNzqHKoKj9gd` |
| Attester wallet | `DUEGFij7AprTaYy1cJ9NhUgkvHDyKfChkw3EXre1u5bP` |
| Cluster | Solana devnet (`https://api.devnet.solana.com`) |
| Canonicalization | RFC 8785 JSON Canonicalization Scheme (JCS) on the CBOM |
| Hash | SHA-256, 32 bytes |

## Attestation account layout

The deployed `agora-attest` program writes a fixed 112-byte `AttestationAccount` to a PDA derived from `(b"attestation", program_address)`. Byte layout:

```
bytes  0..  8   Anchor discriminator
bytes  8.. 40   program_address           (32 bytes)
bytes 40.. 72   audit_pack_hash           (32 bytes)  ← the SHA-256 we verify
bytes 72.. 80   timestamp                 (i64 LE)
bytes 80..112   attester pubkey           (32 bytes)
```

A verifier extracts bytes 40..72, hex-encodes them, and compares to the CBOM's SHA-256.

## Attestation index

### 1. Jito-restaking

| | |
|---|---|
| Program (PDA seed) | `RestkWeAVL8fRGgzhfeoqFhsqKRchg6aa1XrcH96z4Q` |
| CBOM SHA-256 | `4b4c71021f92efff9189d30a4b7fb330fac71faca6f8b43acff85f60f93d8257` |
| Tx signature | `crxJqAbEbH1bgHAmYQ1PLaqNf2AzXJGbF6bLpftsNg6qoWBYFqSJetQjJ1M6gDaJv7xzdX1T6ZxDy1WMEdnQ2Mz` |
| Attestation PDA | `7gnqboC1nBbXQxoznUsy1qe9XgKx2cyteDvuvZqaiXtW` |
| Verification | PASS |
| Solscan | <https://solscan.io/tx/crxJqAbEbH1bgHAmYQ1PLaqNf2AzXJGbF6bLpftsNg6qoWBYFqSJetQjJ1M6gDaJv7xzdX1T6ZxDy1WMEdnQ2Mz?cluster=devnet> |

### 2. Marinade

| | |
|---|---|
| Program (PDA seed) | `MarBmsSgKXdrN1egZf5sqe1TMai9K1rChYNDJgjq7aD` |
| CBOM SHA-256 | `e0ec980903fa5f5b71026d5d84bf077b59bf7f524af6c7af95b21c95f38c53b4` |
| Tx signature | `SKpYTGYWqdjY7ij6PCZQj8Ni5jNXTWGNXEHwE2PTnqwccjUKy8Cmi6cJ4NSb3B7jripVmV9nuJyFjrAGTJchLdd` |
| Attestation PDA | `BkUfXcchXe2oW4mNqbfPKxBkmm15ZWNjnc4WLNXufE6D` |
| Verification | PASS |
| Solscan | <https://solscan.io/tx/SKpYTGYWqdjY7ij6PCZQj8Ni5jNXTWGNXEHwE2PTnqwccjUKy8Cmi6cJ4NSb3B7jripVmV9nuJyFjrAGTJchLdd?cluster=devnet> |

### 3. Kamino (klend)

| | |
|---|---|
| Program (PDA seed) | `KLend2g3cP87fffoy8q1mQqGKjrxjC8boSyAYavgmjD` |
| CBOM SHA-256 | `79b4a5fe16c48ab9cd3068984bd3d00d69c8bdeaf28bd837d777a56841ed59a6` |
| Tx signature | `49HKJvmTwrLUJMaTWHpuKaDzeQBsyBkxZpsZbxv1XNnvoVrb781FsVPAhCDEXfnoEecctWmKfLY8CAc7UbcpWAVH` |
| Attestation PDA | `gkXcaTHWhYiAnqfh6aUDhSKvepRy84xbXmjF4TQSd8G` |
| Verification | PASS |
| Solscan | <https://solscan.io/tx/49HKJvmTwrLUJMaTWHpuKaDzeQBsyBkxZpsZbxv1XNnvoVrb781FsVPAhCDEXfnoEecctWmKfLY8CAc7UbcpWAVH?cluster=devnet> |

### 4. Drift

| | |
|---|---|
| Program (PDA seed) | `dRiftyHA39MWEi3m9aunc5MzRF1JYuBsbn6VPcn33UH` |
| CBOM SHA-256 | `ed48453c222528dd4da8361a23a44efc97d1802da327c38eb4cd98e5f138d490` |
| Tx signature | `3XbkKouHewm2gRPGMpzjUzSKh8idXWFEgtvKmart2o14Nsoxq5tRUp6QLTC8hRDQhWNGBK5zTSCRGB22HCYJtN9S` |
| Attestation PDA | `DYZhNAGb4fraSikeWbNDu7NdjXYqgo1SrUdpQBmuepEt` |
| Verification | PASS |
| Solscan | <https://solscan.io/tx/3XbkKouHewm2gRPGMpzjUzSKh8idXWFEgtvKmart2o14Nsoxq5tRUp6QLTC8hRDQhWNGBK5zTSCRGB22HCYJtN9S?cluster=devnet> |

### 5. MarginFi

| | |
|---|---|
| Program (PDA seed) | `MFv2hWf31Z9kbCa1snEPYctwafyhdvnV7FZnsebVacA` |
| CBOM SHA-256 | `cb2d36eebbb5bf96672e168b7eba3bc49fc40e9cd71c78731644e91d7e084b75` |
| Tx signature | `2RMGFfKWAtgzoSe7t512Z5pNqNBfA7cnKyA3uLDvDCx4eha7GFR29U6dfaSaHNmWQa4unP61vJNBehBDGmeYxcE1` |
| Attestation PDA | `2AggqyQhWAeQ7f5t1dioxssA1Xogxp1cs76Etwrky5iL` |
| Verification | PASS |
| Solscan | <https://solscan.io/tx/2RMGFfKWAtgzoSe7t512Z5pNqNBfA7cnKyA3uLDvDCx4eha7GFR29U6dfaSaHNmWQa4unP61vJNBehBDGmeYxcE1?cluster=devnet> |

### 6. Orca whirlpools

| | |
|---|---|
| Program (PDA seed) | `whirLbMiicVdio4qvUfM5KAg6Ct8VwpYzGff3uctyCc` |
| CBOM SHA-256 | `25b611b6a6db7965b308c2ece42a96df7315c37600ffc386ef88de295e77dde7` |
| Tx signature | `62wWDA1JvqEvt7wArVo69p45ydzEhB5GwQo1GXkRiNh9JhbXwwn9oU7MtSBoijwfJJo5FJS2TfE5uThJQhQwBo8e` |
| Attestation PDA | `EfrtctwaGEQAdMGcxxmmJK7HEL3niRqjgdGzZKx6KLVx` |
| Verification | PASS |
| Solscan | <https://solscan.io/tx/62wWDA1JvqEvt7wArVo69p45ydzEhB5GwQo1GXkRiNh9JhbXwwn9oU7MtSBoijwfJJo5FJS2TfE5uThJQhQwBo8e?cluster=devnet> |

### 7. Meteora DLMM

| | |
|---|---|
| Program (PDA seed) | `LBUZKhRxPF3XUpBCjp4YzTKgLccjZhTSDM9YuVaPwxo` |
| CBOM SHA-256 | `da1a1f1458d3f94ca08725a9ba84b8047c7563afeeb902f5db956b5b23adbc30` |
| Tx signature | `2muvENZkQ67LURr7wsbrLTXtVFukfR6gsk682jGUXB2WtX31yUccZ9o2yWZwd2dofD6D9hX1C3rxFnQooiyMk7Xk` |
| Attestation PDA | `4Nauz9RCnGcnYV8PjaHP2seaSzoc1WGwwramPZCJx4CV` |
| Verification | PASS |
| Solscan | <https://solscan.io/tx/2muvENZkQ67LURr7wsbrLTXtVFukfR6gsk682jGUXB2WtX31yUccZ9o2yWZwd2dofD6D9hX1C3rxFnQooiyMk7Xk?cluster=devnet> |

### 8. Squads

| | |
|---|---|
| Program (PDA seed) | `SQDS4ep65T869zMMBKyuUq6aD6EgTu8psMjkvj52pCf` |
| CBOM SHA-256 | `8dfcda9c8afff76cc561513d7f0f52cd30c3d90b02702443db826b8b7d73024f` |
| Tx signature | `K5bC37JevZEMw46FeHTM5ZrPBYyKYAD29g3YbeVBrbzmzx92KRB7aVtN76eN3YrfzUeFoMfHLU9UoRa2Zf8oUV5` |
| Attestation PDA | `AV8SeiEZSSnHKyYEPNw9oVHhJbWS5ftxKF62WrR4NV1j` |
| Verification | PASS |
| Solscan | <https://solscan.io/tx/K5bC37JevZEMw46FeHTM5ZrPBYyKYAD29g3YbeVBrbzmzx92KRB7aVtN76eN3YrfzUeFoMfHLU9UoRa2Zf8oUV5?cluster=devnet> |

## Verification protocol

For every row above, the read-back procedure was:

1. `solana account <PDA> --url https://api.devnet.solana.com --output json`
2. `jq -r '.account.data[0]' | base64 -d | dd bs=1 skip=40 count=32 | xxd -p`
3. Byte-equal the result against the CBOM SHA-256.

All eight return PASS.

## Reproducing the verification (any third party)

You need only `solana-cli`, `jq`, and a shell:

```bash
PDA="3vMooazLdienYAkePFXE97wcHRUCTnyHPjBpjFcUSR2J"  # any PDA from above
EXPECTED="ed48453c222528dd4da8361a23a44efc97d1802da327c38eb4cd98e5f138d490"
RPC="https://api.devnet.solana.com"

ONCHAIN=$(solana account "$PDA" --url "$RPC" --output json \
  | jq -r '.account.data[0]' \
  | base64 -d \
  | dd bs=1 skip=40 count=32 2>/dev/null \
  | xxd -p | tr -d '\n')

[ "$ONCHAIN" = "$EXPECTED" ] && echo PASS || echo FAIL
```

## Limitations

- All attestations are on **devnet**. A mainnet deploy + production-attestation runbook is the next workstream.
- The CBOMs in this repo carry the exact `metadata.timestamp` baked at scan time; re-hashing a re-scan of the same source will produce a different SHA-256 because production scans use `chrono::Utc::now()` for the timestamp field. The SHA-256 committed on-chain is the canonical value for the attested snapshot.

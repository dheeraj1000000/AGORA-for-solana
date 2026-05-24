# Squads

Per-protocol summary of the AGORA scan + on-chain attestation.

## Quick reference

| | |
|---|---|
| Source LOC scanned | 11275 |
| Mainnet program ID | `SQDS4ep65T869zMMBKyuUq6aD6EgTu8psMjkvj52pCf` |
| CBOM file | [`cbom.json`](cbom.json) |
| CBOM bytes (this snapshot) | 47464 B |
| Audit pack PDF | [`audit_pack.pdf`](audit_pack.pdf) |
| PDF size | 12505 B |
| **Attestation PDA** | `AV8SeiEZSSnHKyYEPNw9oVHhJbWS5ftxKF62WrR4NV1j` |
| **Tx signature** | `K5bC37JevZEMw46FeHTM5ZrPBYyKYAD29g3YbeVBrbzmzx92KRB7aVtN76eN3YrfzUeFoMfHLU9UoRa2Zf8oUV5` |
| Solscan | <https://solscan.io/tx/K5bC37JevZEMw46FeHTM5ZrPBYyKYAD29g3YbeVBrbzmzx92KRB7aVtN76eN3YrfzUeFoMfHLU9UoRa2Zf8oUV5?cluster=devnet> |

## Findings by rule (from CBOM components)

```json
[
  {
    "rule_id": "CA-007",
    "count": 22
  },
  {
    "rule_id": "CA-LD-001",
    "count": 20
  },
  {
    "rule_id": "CA-002",
    "count": 4
  },
  {
    "rule_id": "CA-003",
    "count": 2
  },
  {
    "rule_id": "CA-001",
    "count": 1
  }
]
```

## How to verify this attestation

```bash
solana account AV8SeiEZSSnHKyYEPNw9oVHhJbWS5ftxKF62WrR4NV1j \
  --url https://api.devnet.solana.com --output json \
  | jq -r '.account.data[0]' \
  | base64 -d \
  | dd bs=1 skip=40 count=32 2>/dev/null \
  | xxd -p | tr -d '\n'
# Expected output: audit_pack_hash field of the on-chain attestation
```

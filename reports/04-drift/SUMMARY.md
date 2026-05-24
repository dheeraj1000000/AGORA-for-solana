# Drift

Per-protocol summary of the AGORA scan + on-chain attestation.

## Quick reference

| | |
|---|---|
| Source LOC scanned | 152968 |
| Mainnet program ID | `dRiftyHA39MWEi3m9aunc5MzRF1JYuBsbn6VPcn33UH` |
| CBOM file | [`cbom.json`](cbom.json) |
| CBOM bytes (this snapshot) | 140375 B |
| Audit pack PDF | [`audit_pack.pdf`](audit_pack.pdf) |
| PDF size | 18446 B |
| **Attestation PDA** | `DYZhNAGb4fraSikeWbNDu7NdjXYqgo1SrUdpQBmuepEt` |
| **Tx signature** | `3XbkKouHewm2gRPGMpzjUzSKh8idXWFEgtvKmart2o14Nsoxq5tRUp6QLTC8hRDQhWNGBK5zTSCRGB22HCYJtN9S` |
| Solscan | <https://solscan.io/tx/3XbkKouHewm2gRPGMpzjUzSKh8idXWFEgtvKmart2o14Nsoxq5tRUp6QLTC8hRDQhWNGBK5zTSCRGB22HCYJtN9S?cluster=devnet> |

## Findings by rule (from CBOM components)

```json
[
  {
    "rule_id": "CA-002",
    "count": 136
  },
  {
    "rule_id": "CA-001",
    "count": 25
  }
]
```

## How to verify this attestation

```bash
solana account DYZhNAGb4fraSikeWbNDu7NdjXYqgo1SrUdpQBmuepEt \
  --url https://api.devnet.solana.com --output json \
  | jq -r '.account.data[0]' \
  | base64 -d \
  | dd bs=1 skip=40 count=32 2>/dev/null \
  | xxd -p | tr -d '\n'
# Expected output: audit_pack_hash field of the on-chain attestation
```

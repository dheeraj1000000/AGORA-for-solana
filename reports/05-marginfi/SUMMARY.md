# MarginFi

Per-protocol summary of the AGORA scan + on-chain attestation.

## Quick reference

| | |
|---|---|
| Source LOC scanned | 100555 |
| Mainnet program ID | `MFv2hWf31Z9kbCa1snEPYctwafyhdvnV7FZnsebVacA` |
| CBOM file | [`cbom.json`](cbom.json) |
| CBOM bytes (this snapshot) | 112844 B |
| Audit pack PDF | [`audit_pack.pdf`](audit_pack.pdf) |
| PDF size | 18452 B |
| **Attestation PDA** | `2AggqyQhWAeQ7f5t1dioxssA1Xogxp1cs76Etwrky5iL` |
| **Tx signature** | `2RMGFfKWAtgzoSe7t512Z5pNqNBfA7cnKyA3uLDvDCx4eha7GFR29U6dfaSaHNmWQa4unP61vJNBehBDGmeYxcE1` |
| Solscan | <https://solscan.io/tx/2RMGFfKWAtgzoSe7t512Z5pNqNBfA7cnKyA3uLDvDCx4eha7GFR29U6dfaSaHNmWQa4unP61vJNBehBDGmeYxcE1?cluster=devnet> |

## Findings by rule (from CBOM components)

```json
[
  {
    "rule_id": "CA-002",
    "count": 100
  },
  {
    "rule_id": "CA-LD-001",
    "count": 20
  },
  {
    "rule_id": "CA-001",
    "count": 9
  }
]
```

## How to verify this attestation

```bash
solana account 2AggqyQhWAeQ7f5t1dioxssA1Xogxp1cs76Etwrky5iL \
  --url https://api.devnet.solana.com --output json \
  | jq -r '.account.data[0]' \
  | base64 -d \
  | dd bs=1 skip=40 count=32 2>/dev/null \
  | xxd -p | tr -d '\n'
# Expected output: audit_pack_hash field of the on-chain attestation
```

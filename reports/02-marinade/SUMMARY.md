# Marinade Finance

Per-protocol summary of the AGORA scan + on-chain attestation.

## Quick reference

| | |
|---|---|
| Source LOC scanned | 7610 |
| Mainnet program ID | `MarBmsSgKXdrN1egZf5sqe1TMai9K1rChYNDJgjq7aD` |
| CBOM file | [`cbom.json`](cbom.json) |
| CBOM bytes (this snapshot) | 25970 B |
| Audit pack PDF | [`audit_pack.pdf`](audit_pack.pdf) |
| PDF size | 13136 B |
| **Attestation PDA** | `BkUfXcchXe2oW4mNqbfPKxBkmm15ZWNjnc4WLNXufE6D` |
| **Tx signature** | `SKpYTGYWqdjY7ij6PCZQj8Ni5jNXTWGNXEHwE2PTnqwccjUKy8Cmi6cJ4NSb3B7jripVmV9nuJyFjrAGTJchLdd` |
| Solscan | <https://solscan.io/tx/SKpYTGYWqdjY7ij6PCZQj8Ni5jNXTWGNXEHwE2PTnqwccjUKy8Cmi6cJ4NSb3B7jripVmV9nuJyFjrAGTJchLdd?cluster=devnet> |

## Findings by rule (from CBOM components)

```json
[
  {
    "rule_id": "CA-002",
    "count": 17
  },
  {
    "rule_id": "L5-VALIDATOR-AUTHORITY",
    "count": 9
  }
]
```

## How to verify this attestation

```bash
solana account BkUfXcchXe2oW4mNqbfPKxBkmm15ZWNjnc4WLNXufE6D \
  --url https://api.devnet.solana.com --output json \
  | jq -r '.account.data[0]' \
  | base64 -d \
  | dd bs=1 skip=40 count=32 2>/dev/null \
  | xxd -p | tr -d '\n'
# Expected output: audit_pack_hash field of the on-chain attestation
```

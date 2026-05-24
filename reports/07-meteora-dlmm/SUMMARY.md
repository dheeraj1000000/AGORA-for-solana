# Meteora DLMM

Per-protocol summary of the AGORA scan + on-chain attestation.

## Quick reference

| | |
|---|---|
| Source LOC scanned | 14452 |
| Mainnet program ID | `LBUZKhRxPF3XUpBCjp4YzTKgLccjZhTSDM9YuVaPwxo` |
| CBOM file | [`cbom.json`](cbom.json) |
| CBOM bytes (this snapshot) | 14205 B |
| Audit pack PDF | [`audit_pack.pdf`](audit_pack.pdf) |
| PDF size | 9303 B |
| **Attestation PDA** | `4Nauz9RCnGcnYV8PjaHP2seaSzoc1WGwwramPZCJx4CV` |
| **Tx signature** | `2muvENZkQ67LURr7wsbrLTXtVFukfR6gsk682jGUXB2WtX31yUccZ9o2yWZwd2dofD6D9hX1C3rxFnQooiyMk7Xk` |
| Solscan | <https://solscan.io/tx/2muvENZkQ67LURr7wsbrLTXtVFukfR6gsk682jGUXB2WtX31yUccZ9o2yWZwd2dofD6D9hX1C3rxFnQooiyMk7Xk?cluster=devnet> |

## Findings by rule (from CBOM components)

```json
[
  {
    "rule_id": "CA-LD-001",
    "count": 20
  }
]
```

## How to verify this attestation

```bash
solana account 4Nauz9RCnGcnYV8PjaHP2seaSzoc1WGwwramPZCJx4CV \
  --url https://api.devnet.solana.com --output json \
  | jq -r '.account.data[0]' \
  | base64 -d \
  | dd bs=1 skip=40 count=32 2>/dev/null \
  | xxd -p | tr -d '\n'
# Expected output: audit_pack_hash field of the on-chain attestation
```

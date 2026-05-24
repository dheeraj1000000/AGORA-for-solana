# Kamino (klend)

Per-protocol summary of the AGORA scan + on-chain attestation.

## Quick reference

| | |
|---|---|
| Source LOC scanned | 36990 |
| Mainnet program ID | `KLend2g3cP87fffoy8q1mQqGKjrxjC8boSyAYavgmjD` |
| CBOM file | [`cbom.json`](cbom.json) |
| CBOM bytes (this snapshot) | 32685 B |
| Audit pack PDF | [`audit_pack.pdf`](audit_pack.pdf) |
| PDF size | 16823 B |
| **Attestation PDA** | `gkXcaTHWhYiAnqfh6aUDhSKvepRy84xbXmjF4TQSd8G` |
| **Tx signature** | `49HKJvmTwrLUJMaTWHpuKaDzeQBsyBkxZpsZbxv1XNnvoVrb781FsVPAhCDEXfnoEecctWmKfLY8CAc7UbcpWAVH` |
| Solscan | <https://solscan.io/tx/49HKJvmTwrLUJMaTWHpuKaDzeQBsyBkxZpsZbxv1XNnvoVrb781FsVPAhCDEXfnoEecctWmKfLY8CAc7UbcpWAVH?cluster=devnet> |

## Findings by rule (from CBOM components)

```json
[
  {
    "rule_id": "CA-002",
    "count": 33
  }
]
```

## How to verify this attestation

```bash
solana account gkXcaTHWhYiAnqfh6aUDhSKvepRy84xbXmjF4TQSd8G \
  --url https://api.devnet.solana.com --output json \
  | jq -r '.account.data[0]' \
  | base64 -d \
  | dd bs=1 skip=40 count=32 2>/dev/null \
  | xxd -p | tr -d '\n'
# Expected output: audit_pack_hash field of the on-chain attestation
```

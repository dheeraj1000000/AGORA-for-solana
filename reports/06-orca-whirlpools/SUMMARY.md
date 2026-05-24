# Orca whirlpools

Per-protocol summary of the AGORA scan + on-chain attestation.

## Quick reference

| | |
|---|---|
| Source LOC scanned | 110170 |
| Mainnet program ID | `whirLbMiicVdio4qvUfM5KAg6Ct8VwpYzGff3uctyCc` |
| CBOM file | [`cbom.json`](cbom.json) |
| CBOM bytes (this snapshot) | 54077 B |
| Audit pack PDF | [`audit_pack.pdf`](audit_pack.pdf) |
| PDF size | 18611 B |
| **Attestation PDA** | `EfrtctwaGEQAdMGcxxmmJK7HEL3niRqjgdGzZKx6KLVx` |
| **Tx signature** | `62wWDA1JvqEvt7wArVo69p45ydzEhB5GwQo1GXkRiNh9JhbXwwn9oU7MtSBoijwfJJo5FJS2TfE5uThJQhQwBo8e` |
| Solscan | <https://solscan.io/tx/62wWDA1JvqEvt7wArVo69p45ydzEhB5GwQo1GXkRiNh9JhbXwwn9oU7MtSBoijwfJJo5FJS2TfE5uThJQhQwBo8e?cluster=devnet> |

## Findings by rule (from CBOM components)

```json
[
  {
    "rule_id": "CA-002",
    "count": 50
  },
  {
    "rule_id": "CA-LD-001",
    "count": 7
  },
  {
    "rule_id": "CA-001",
    "count": 2
  },
  {
    "rule_id": "CA-003",
    "count": 2
  }
]
```

## How to verify this attestation

```bash
solana account EfrtctwaGEQAdMGcxxmmJK7HEL3niRqjgdGzZKx6KLVx \
  --url https://api.devnet.solana.com --output json \
  | jq -r '.account.data[0]' \
  | base64 -d \
  | dd bs=1 skip=40 count=32 2>/dev/null \
  | xxd -p | tr -d '\n'
# Expected output: audit_pack_hash field of the on-chain attestation
```

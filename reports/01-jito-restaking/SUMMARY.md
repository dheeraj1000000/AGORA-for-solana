# Jito (restaking)

Per-protocol summary of the AGORA scan + on-chain attestation.

## Quick reference

| | |
|---|---|
| Source LOC scanned | 68672 |
| Mainnet program ID | `RestkWeAVL8fRGgzhfeoqFhsqKRchg6aa1XrcH96z4Q` |
| CBOM file | [`cbom.json`](cbom.json) |
| CBOM bytes (this snapshot) | 314 B |
| Audit pack PDF | [`audit_pack.pdf`](audit_pack.pdf) |
| PDF size | 9305 B |
| **Attestation PDA** | `7gnqboC1nBbXQxoznUsy1qe9XgKx2cyteDvuvZqaiXtW` |
| **Tx signature** | `crxJqAbEbH1bgHAmYQ1PLaqNf2AzXJGbF6bLpftsNg6qoWBYFqSJetQjJ1M6gDaJv7xzdX1T6ZxDy1WMEdnQ2Mz` |
| Solscan | <https://solscan.io/tx/crxJqAbEbH1bgHAmYQ1PLaqNf2AzXJGbF6bLpftsNg6qoWBYFqSJetQjJ1M6gDaJv7xzdX1T6ZxDy1WMEdnQ2Mz?cluster=devnet> |

## Findings by rule (from CBOM components)

```json
[]
```

## How to verify this attestation

```bash
solana account 7gnqboC1nBbXQxoznUsy1qe9XgKx2cyteDvuvZqaiXtW \
  --url https://api.devnet.solana.com --output json \
  | jq -r '.account.data[0]' \
  | base64 -d \
  | dd bs=1 skip=40 count=32 2>/dev/null \
  | xxd -p | tr -d '\n'
# Expected output: audit_pack_hash field of the on-chain attestation
```

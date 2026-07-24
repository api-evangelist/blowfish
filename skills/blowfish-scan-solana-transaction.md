---
name: Scan Solana transactions before signing
description: Submit Solana transactions to Blowfish and act on the recommended action and warnings.
api: openapi/blowfish-v20230308-openapi-original.yml
operations: [scan-transactions-solana]
---

# Scan Solana transactions before signing

Use this skill to check one or more Solana transactions for risk before signing.

## Auth
- Send your API key in the `X-Api-Key` header (see `authentication/blowfish-authentication.yml`).
- Free-tier keys call `https://free.api.blowfish.xyz`; paid keys call `https://api.blowfish.xyz`.

## Steps
1. Call `scan-transactions-solana` — `POST /solana/v0/mainnet/scan/transactions` — with the transactions, the user account, and metadata in the JSON body.
2. Branch on `action` (`NONE` / `WARN` / `BLOCK`) exactly as for EVM — `BLOCK` shows a block screen, `WARN` shows warnings, `NONE` proceeds.
3. Render the severity-sorted `warnings` and, if useful, the `simulationResults` (e.g. SolTransfer, SplTransfer, SplApproval, SolStakeAuthorityChange state changes).

## Errors
See `errors/blowfish-problem-types.yml` (400 / 401 / 500). Envelope is `{ "error": "<message>" }`.

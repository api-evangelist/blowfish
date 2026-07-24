---
name: Scan an EVM transaction before signing
description: Submit an Ethereum/Polygon transaction to Blowfish and act on the recommended action and warnings.
api: openapi/blowfish-v20230308-openapi-original.yml
operations: [scan-transaction-evm]
---

# Scan an EVM transaction before signing

Use this skill to check an Ethereum/Polygon transaction for risk before presenting a signing UI.

## Auth
- Send your API key in the `X-Api-Key` header (see `authentication/blowfish-authentication.yml`).
- Free-tier keys must call `https://free.api.blowfish.xyz`; paid keys call `https://api.blowfish.xyz`.
- Optionally pin the API version with `X-Api-Version: 2022-06-01` and localize messages with the `language` query param.

## Steps
1. Call `scan-transaction-evm` — `POST /ethereum/v0/mainnet/scan/transaction` — with the transaction, the user account, and metadata (origin domain) in the JSON body.
2. Read `action` from the response:
   - `BLOCK` → show a block screen; the transaction is highly likely malicious.
   - `WARN` → show the `warnings` to the user.
   - `NONE` → proceed to the normal signing UI.
3. Render `warnings` (severity-sorted; index 0 is highest). Treat `CRITICAL` as red, `WARNING` as yellow. Warning `kind` values are enumerated in `vocabulary/blowfish-scan-vocabulary.yml`.
4. Optionally surface the human-readable `simulationResults` (expected state changes) to the user.

## Errors
See `errors/blowfish-problem-types.yml`. 400 = malformed/empty request; 401 = bad `X-Api-Key`; 500 = retry with backoff. The envelope is `{ "error": "<message>" }`.

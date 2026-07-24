---
name: Scan a dApp domain for safety
description: Check whether a dApp domain is safe for a user to interact with, or pull the full blocklist.
api: openapi/blowfish-v20230308-openapi-original.yml
operations: [scan-domain, download-blocklist]
---

# Scan a dApp domain for safety

Use this skill to determine whether a dApp domain is malicious before a user connects, or to maintain a local blocklist.

## Auth
Send your API key in the `X-Api-Key` header (see `authentication/blowfish-authentication.yml`).

## Steps — real-time domain scan
1. Call `scan-domain` — `POST /v0/domains` — with the domain(s) in the JSON body.
2. The response array reports each domain's status and any warning kinds (e.g. `BLOCKLISTED_DOMAIN_CROSS_ORIGIN`, `COPY_CAT_DOMAIN`, `NON_ASCII_URL`). Block or warn accordingly.

## Steps — local blocklist
1. Call `download-blocklist` — `POST /v0/domains/blocklist` — to get a link to a downloadable snapshot of all blocked domains.
2. Download the snapshot and check domains locally (see the `@blowfishxyz/blocklist` package in `packages/blowfish-packages.yml`).

## Errors
See `errors/blowfish-problem-types.yml` (400 / 401 / 500). Envelope is `{ "error": "<message>" }`.

# commodex-agent-avalanche-infrastructure

Avalanche infrastructure agent for the Commodex Avalanche C-Chain setup. Commodex: https://commodex.xyz/

- Production: https://commodex.xyz
- Testnet UI: https://wedevnext.commodex.xyz
- Fuji evidence page on the testnet UI: https://wedevnext.commodex.xyz/fuji
- Network: Avalanche Fuji C-Chain (EVM), chain id `43113`

## Agent

| | |
|---|---|
| Person | Stefan Bauer |
| Role | Avalanche infrastructure — RPC, witness, operator keys and runbooks |
| FR scope | FR-03 |
| Squad page | https://team.impactcapital.ltd/showcase/commodex/ |

## Status — 11 October 2026

**Fuji enabled in the staging witness; staging server operator wallet `0x215edFA8C9Db88A972FbEDb810b884ADe94fca7B` holds the anchor role (key mounted read-only, never in the database).**

- Fuji test wallet bound in the admin desk: https://testnet.snowtrace.io/address/0xa6547c2812140c0495113d4490eb0f2805A67ecD (0.005 AVAX, no transaction sent yet).
- The corridor witness reads Avalanche only (C-Chain 43114 and Fuji 43113) through the app's allow-listed egress client.

Runbook: [docs/runbook.md](docs/runbook.md).

## Commands

Is the public Fuji RPC the right network (expects `0xa869` = 43113)?

```bash
curl -s -X POST https://api.avax-test.network/ext/bc/C/rpc -H 'content-type: application/json'   -d '{"jsonrpc":"2.0","id":1,"method":"eth_chainId","params":[]}'
```

Who may anchor on the contract (expects the server wallet `0x215edfa8…94fca7b`)?

```bash
curl -s -X POST https://api.avax-test.network/ext/bc/C/rpc -H 'content-type: application/json'   -d '{"jsonrpc":"2.0","id":1,"method":"eth_call","params":[{"to":"0xe274C452dFb0D259FA413EEd3a830197a4Fd50ef","data":"0x570ca735"},"latest"]}'
```

How the testnet UI container gets its operator key and contract (names only; the key file never leaves the server):

```yaml
# docker compose, staging app service only
environment:
  CDX_ANCHOR_KEY_FILE: /run/secrets/fuji-anchor.key   # one-line private key, uid 1001, mode 0400
  CDX_ANCHOR_CONTRACT: "0xe274C452dFb0D259FA413EEd3a830197a4Fd50ef"
volumes:
  - ./secrets/fuji-anchor.key:/run/secrets/fuji-anchor.key:ro
```

Production carries neither line, and the app refuses to anchor there unless `CDX_ANCHOR_ALLOW_PRODUCTION=true`.
Full re-check of every Fuji transaction: `node checks/verify-fuji.mjs` in
[commodex-agent-contract-assurance](https://github.com/Impact-Capital-Management/commodex-agent-contract-assurance).

## Evidence

| Step | Proof | Explorer |
|---|---|---|
| Fuji enabled in the staging witness (RPC answers chain id 43113; C-Chain off) | checked 2026-10-11 | — |
| Server operator wallet funded (0.002 test AVAX) | block 59308703 | https://testnet.snowtrace.io/tx/0xe931ef101e016bbad59cc04a1a8db6b147c55e495bbcd013e98811e6af9dc3c3 |
| Server operator wallet takes over the anchor contract (`setOperator`) | block 59308707 | https://testnet.snowtrace.io/tx/0xcca9d1976ab774685cef487e1475ee685f19960e5af56ba73973a731c1e9f9f7 |

## License

MIT — see [LICENSE](LICENSE).

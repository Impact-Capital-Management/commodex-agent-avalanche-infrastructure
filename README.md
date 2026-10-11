# commodex-agent-avalanche-infrastructure

Avalanche infrastructure agent for the Commodex Avalanche C-Chain setup. Commodex: https://commodex.xyz/

- Production: https://commodex.xyz
- Testnet UI: https://wedevnext.commodex.xyz
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

## Evidence

| Step | Proof | Explorer |
|---|---|---|
| Fuji enabled in the staging witness (RPC answers chain id 43113; C-Chain off) | checked 2026-10-11 | — |
| Server operator wallet funded (0.002 test AVAX) | block 59308703 | https://testnet.snowtrace.io/tx/0xe931ef101e016bbad59cc04a1a8db6b147c55e495bbcd013e98811e6af9dc3c3 |
| Server operator wallet takes over the anchor contract (`setOperator`) | block 59308707 | https://testnet.snowtrace.io/tx/0xcca9d1976ab774685cef487e1475ee685f19960e5af56ba73973a731c1e9f9f7 |

## License

MIT — see [LICENSE](LICENSE).

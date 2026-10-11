# Fuji runbook

## Networks

| Network | Chain id | Public RPC | Explorer | State |
|---|---|---|---|---|
| Avalanche Fuji | 43113 | https://api.avax-test.network/ext/bc/C/rpc | https://testnet.snowtrace.io | rehearsal network |
| Avalanche C-Chain | 43114 | https://api.avax.network/ext/bc/C/rpc | https://snowtrace.io | off until the Fuji pack is green |

## Switch Fuji on in the staging witness

1. Admin → Chains on https://wedevnext.commodex.xyz: confirm the RPC answers `eth_chainId` = `0xa869` (43113).
2. Enable Avalanche Fuji only. Leave C-Chain and every other preset off the corridor witness.
3. Operator console → "Chain witness now" must return a Fuji block number and hash that open on the explorer.

## Operator keys

- Two Fuji wallets: the deployer (owner's test wallet) and a server operator wallet.
- The deployer deploys `CommodexAnchor`, then calls `setOperator(<server wallet>)`.
- The server key is a one-line file mounted read-only into the staging container. It is never stored in the
  database, never printed in logs, never present on production.
- Fund the server wallet with 0.01 AVAX from https://core.app/tools/testnet-faucet/ — one anchor costs far
  below 0.000001 AVAX at the 11 Oct 2026 gas price.

## Rotation and loss

- Lost or leaked server key: the deployer calls `setOperator(<new wallet>)`; the old key can no longer anchor.
- Lost deployer key while it is still the operator: deploy a new contract and record the new address; earlier
  anchors stay valid on chain.

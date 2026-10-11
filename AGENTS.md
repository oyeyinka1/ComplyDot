# complyDot: agent brief

Read this file fully before changing anything. It is the project's source of truth for scope, rules and chain facts.

## What we are building

complyDot lets an issuer run a regulated asset on Portaldot V3 using only the chain's native modules.

- Assets module: create, mint, freeze, thaw, transfer. Holders start frozen.
- Nfts module: eligibility credential NFTs that are locked against transfer and carry attributes: `eligibility_class`, `expires_at`, `ref_hash`. No personal data on-chain.
- Expiry enforcer: a background task that freezes accounts whose credential has expired or been revoked.
- AuditLog contract (Solidity, EVM side): records every compliance action as an event.
- Issuer dashboard and investor page (web), plus a CSV compliance report.

Hackathon: Portaldot Hacker House 2026, Ecosystem DApps track. Judged on technical feasibility, innovation, ecosystem contribution and business potential, using verifiable testnet activity and GitHub history.

## Hard rules

1. Never read, print, log or commit `.env` or any file holding keys or seed phrases. Use `.env.example` with placeholders only.
2. No private keys or seeds in code, tests, logs or commit messages.
3. Do not add a dependency without telling the user. Every dependency must be listed in the README "Dependencies" section.
4. Do not copy code from other projects unless it is licensed and disclosed. Patterns from the official Portaldot testnet guide demos are allowed, and must be disclosed.
5. Do not invent chain behaviour. If you are unsure whether a call works on Portaldot V3, say so, add a `TODO(verify)` and keep the code defensive.
6. Keep all endpoints and chain constants in one config module that reads from environment variables.
7. Small commits, one concern each, clear messages. Never rewrite git history.
8. Stay inside the scope above. Propose extra features instead of adding them.
9. Handle the node being unreachable gracefully: timeouts, retries, and plain-language errors. The testnet has been unstable.
10. The user works on Windows with VS Code and a bash terminal. Commands and paths must work there.

## Out of scope

Smart-account recovery, ERC-4337 or ERC-7579, cross-chain features, DaaS, a custom token, real KYC integration, wallet-extension signing (stretch only).

## Stack and layout

- `backend/`: Python 3 with FastAPI and `substrate-interface`
- `frontend/`: React with Vite
- `contracts/`: Solidity 0.8.24, tested locally (Hardhat or Foundry), deployed with ethers.js
- `docs/`: plan and architecture
- `scripts/`: helper scripts

## Chain facts (from the official testnet guide)

- Substrate RPC: `wss://testnetv3-node.feso-apps.xyz`
- EVM RPC: `https://testnetv3-eth-rpc.feso-apps.xyz`
- Chain ID: `420420777`
- Token: `tPOTv3`, 14 decimals
- Node console: `https://node-console.feso-apps.xyz/`
- Official guide and sample demos: `https://github.com/ItsCogumellum/portaldot-v3-testnet-guide` (demo scripts: `00_base`, `01_assets`, `02_uniques`, `03_nfts`, `04_solidity`)
- An EVM address maps to a different Substrate account. The mapped account is found with the node console call `reviveApi` then `accountId`.
- Endpoints may change after the node upgrade announced by the Portaldot team. Check the config first when something fails.

## Known unknowns (do not assume)

- Whether Assets freeze and thaw, and Nfts transfer locks and attributes, behave as documented on the V3 testnet. This is the "gate" test.
- Whether the EVM RPC supports everything the audit-log tooling needs.

## Definition of done for any task

- It runs on the user's machine with the steps written in the README.
- It has tests that do not need the chain (mocks), or a clear note that it needs the chain.
- It is committed with a clear message.
- The README "Dependencies" list is updated if anything was added.
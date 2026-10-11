#  ComplyDot

Compliance-ready issuance of regulated assets on Portaldot V3, built on the chain's native Assets and Nfts modules instead of custom token contracts.

> **Status:** in development for the Portaldot Hacker House 2026 (Ecosystem DApps track).

## The problem

Issuers of regulated assets (security tokens, real-world assets) have to control who can hold and move them. On most chains that control is custom contract code, a frequent source of access-control bugs, while identity lives in a separate layer. Issuers and holders end up with fragmented tooling and no clear audit trail.

## What it does

1. An issuer creates an asset. Holders start out frozen.
2. The issuer onboards an investor by issuing an **eligibility credential**: a non-transferable NFT whose attributes record the eligibility class, an expiry and an off-chain reference hash.
3. The console thaws the investor for that asset, and transfers work.
4. If the credential is revoked or expires, an automated enforcer freezes the account again.
5. Every action is recorded: native chain events plus an audit-log contract on the EVM side.
6. An investor page shows the credential, holdings and status.

## Why Portaldot V3

- **Native Assets module:** owner, issuer, admin and freezer roles, per-account freezing and approved transfers. Compliance controls live at the runtime level, with no custom token contract to get wrong.
- **Native Nfts module:** transfer locks and attribute namespaces make non-transferable credentials possible.
- **EVM compatibility:** a small audit-log contract that anyone can read with standard EVM tools.

## Honest limits

- Enforcement is operational (issuer-controlled freeze and thaw), not an atomic on-chain gate.
- KYC itself is out of scope. The demo uses issuer approval as a stand-in, and no personal data is stored on-chain.
- Testnet only.

## Architecture

See [docs/plan.md](docs/plan.md).

## Tech stack and dependencies (disclosure)

Planned, to be updated as each item is added:

- Python 3, `substrate-interface`
- FastAPI (backend API)
- React + Vite (frontend)
- Solidity 0.8.24 for the audit-log contract, with ethers.js and Remix/Hardhat for deployment
- Portaldot V3 testnet and the official testnet guide and sample code (patterns from its demo scripts)

## Getting started

Coming soon.

## Hackathon

Built for the Portaldot Hacker House 2026. All code in this repository was written for this event. Open-source libraries and the official sample code are disclosed above.

## License

To be added.
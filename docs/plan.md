# Project plan

Working title: ComplyDot

## Goal

Let an issuer run a regulated asset on Portaldot V3 using only native modules: who may hold it is controlled by freezing, and eligibility is proven by non-transferable credential NFTs. Every decision leaves an audit trail.

## Demo flow (about 3 minutes)

1. Issuer creates an asset. Holders are frozen by default.
2. Issuer onboards investor 1: a credential NFT is minted, locked against transfer, and given attributes.
3. The console thaws investor 1. A transfer to investor 1 succeeds.
4. Investor 2 has no credential. A transfer to investor 2 is blocked.
5. Issuer revokes investor 1. The account is frozen and the next transfer fails.
6. Audit trail and investor page show each step, with transaction hashes.

## Scope

**In:**
- Native Assets: create, mint, freeze, thaw, transfer
- Native Nfts: credential collection, mint, lock transfer, attributes, burn on revoke
- Expiry enforcer: a background task that freezes accounts whose credential has expired
- Audit-log contract on the EVM side
- Issuer dashboard and investor page
- Exportable compliance report (CSV)

**Out (too hard or outside what Portaldot provided):**
- Smart-account recovery and ERC-4337 / ERC-7579 modules
- Cross-chain features and DaaS
- Real KYC integration (issuer approval stands in)
- Our own token or economics
- Wallet-extension signing (stretch only)

## Architecture

```
 Issuer dashboard / Investor page   (React + Vite)
                 |
          REST API + enforcer        (FastAPI, Python)
          |                  |
 substrate-interface     ethers.js / web3
          |                  |
 Portaldot V3 node       Portaldot V3 EVM RPC
 (Assets + Nfts)         (AuditLog contract)
```

Data flow for onboarding an investor:
1. Dashboard calls the API with the investor address, eligibility class and expiry.
2. The API mints the credential NFT, locks its transfer, and sets its attributes (Nfts module).
3. The API thaws the account for the asset (Assets module).
4. The API writes an audit entry to the AuditLog contract.
5. The dashboard reads chain state and events to display the result.

## Credential model

Attributes on each credential NFT:
- `eligibility_class`: for example `standard` or `accredited`
- `expires_at`: unix timestamp
- `ref_hash`: hash of an off-chain record

No personal data is stored on-chain.

## AuditLog contract (spec)

- One function that records an action: asset id, account, action type (onboard, revoke, expire, freeze, thaw), credential reference hash, timestamp.
- Emits an event for every entry. No funds, no admin, no upgrades.
- Only the issuer address can write.

## Milestones

Oct 11 – 13	Repo, plan, README, agent brief. Gate test of the native modules once the node is back.

Oct 14 – 17	Backend functions for Assets and Nfts. AuditLog contract written and tested locally. Kick-off workshop Oct 14.

Oct 18 – 24	Deploy to testnet, working demo, architecture diagram, demo video for the check-in.

Oct 25 – Nov 2	Expiry enforcer, investor page, compliance report.

Nov 3 – 6	Polish, final README, demo video, pitch deck.

## Gate and fallbacks

Gate: in the node console, create an asset, mint, freeze, thaw and transfer, then try the Nfts calls.

- Assets and Nfts both work: full plan.
- Only Assets works: keep Assets freezing with an eligibility list instead of credential NFTs.
- The Substrate side is unusable: EVM-only version with a freezable token contract and a credential contract.

## Risks

- The testnet is being upgraded, so endpoints may change. All endpoints live in one config file.
- Enforcement is operational, not atomic.
- The Substrate side needs a funded Substrate account.
- It must not read as a nicer wrapper over the node console. The credential model, enforcer, audit trail and report are the core.

## Submission checklists

**Check-in (Oct 24, 12:00 UTC):** public repo with at least 3 commits, README, progress and outstanding items, architecture diagram, testnet deployment information (contract address, transaction hashes), demo link or video of 3 minutes or less.

**Final (Nov 6, 04:00 UTC):** project name, team, one-sentence overview, repo, README, deployment instructions, docs, runnable demo, demo video of 3 minutes or less, pitch deck, testnet deployment information.
# ✨ So you want to run an audit

This `README.md` contains a set of checklists for our audit collaboration.

Your audit will use two repos:

- **an _audit_ repo** (this one), which is used for scoping your audit and for providing information to wardens
- **a _findings_ repo**, where issues are submitted (shared with you after the audit)

Ultimately, when we launch the audit, this repo will be made public and will contain the smart contracts to be reviewed and all the information needed for audit participants. The findings repo will be made public after the audit report is published and your team has mitigated the identified issues.

Some of the checklists in this doc are for **C4 (🐺)** and some of them are for **you as the audit sponsor (⭐️)**.

---

# Repo setup

## ⭐️ Sponsor: Add code to this repo

- [ ] Create a PR to this repo with the below changes:
- [ ] Provide a self-contained repository with working commands that will build (at least) all in-scope contracts, and commands that will run tests producing gas reports for the relevant contracts.
- [ ] Make sure your code is thoroughly commented using the [NatSpec format](https://docs.soliditylang.org/en/v0.5.10/natspec-format.html#natspec-format).
- [ ] Please have final versions of contracts and documentation added/updated in this repo **no less than 24 hours prior to audit start time.**
- [ ] Be prepared for a 🚨code freeze🚨 for the duration of the audit — important because it establishes a level playing field. We want to ensure everyone's looking at the same code, no matter when they look during the audit. (Note: this includes your own repo, since a PR can leak alpha to our wardens!)

---

- Total Prize Pool: $60,500 USDC
  - HM awards: $41,250 USDC
  - Bot Race awards: $3,750 USDC
  - ✨ Analysis awards: $12,500 USDC
  - QA awards: $1,250 USDC
  - Gas awards: $1,250 USDC
  - Judge awards: $6,000 USDC
  - Lookout awards: $4,000 USDC
  - Scout awards: $500 USDC
- Join [C4 Discord](https://discord.gg/code4rena) to register
- Submit findings [using the C4 form](https://code4rena.com/contests/2023-05-llama/submit)
- [Read our guidelines for more details](https://docs.code4rena.com/roles/wardens)
- Starts June 6, 2023 20:00 UTC
- Ends June 13, 2023 20:00 UTC

✨ All participating wardens must submit an Analysis prior to the closing date. [Guidelines and FAQ can be found here.](https://code4rena.notion.site/Analyses-Guidelines-and-FAQ-2808a71e08e44c81a985527194f5f118) The submission form for Analyses is scheduled to go live June 6, 2023.

## Automated Findings / Publicly Known Issues

Automated findings output for the audit can be found [here](add link to report) within 24 hours of audit opening.

_Note for C4 wardens: Anything included in the automated findings output is considered a publicly known issue and is ineligible for awards._

[ ⭐️ SPONSORS ADD INFO HERE ]

# Overview

Llama is a governance system for onchain organizations. Llama enables organizations to encode access control policies, create rules to execute actions, and manage shared accounts.

# Scope

_List all files in scope in the table below (along with hyperlinks) -- and feel free to add notes here to emphasize areas of focus._

All files in the `src/` directory are in scope for the audit contest as well as the `.sol` files in `script/`:
| Contract | SLOC | Purpose |  
| ----------- | ----------- | ----------- |
| [`src/accounts/LlamaAccount.sol`](llama/src/accounts/LlamaAccount.sol) | 187 | This contract can be used to hold assets for a Llama instance. |
| [`src/interfaces/ILlamaAccount.sol`](llama/interfaces/ILlamaAccount.sol) | 5 | An interface for Llama accounts. |
| [src/interfaces/ILlamaActionGuard.sol](llama/interfaces/ILlamaActionGuard.sol) | 7 | An interface for Llama action guards. |
| [`src/interfaces/ILlamaStrategy.sol`](llama/interfaces/ILlamaStrategy.sol) | 23 | An interface for Llama strategies. |
| [`src/lib/Checkpoints.sol`](llama/src/lib/Checkpoints.sol) | 159 | A modified version of [OpenZeppelin's `Checkpoints.sol`](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/d00acef4059807535af0bd0dd0ddf619747a044b/contracts/utils/Checkpoints.sol). |
| [`src/lib/Enums.sol`](llama/src/lib/Enums.sol) | 10 | A file containing the enumerables used throughout the Llama codebase. |
| [`src/lib/ERC721NonTransferableMinimalProxy.sol`](llama/src/lib/ERC721NonTransferableMinimalProxy.sol) | 96 | A modified version of [Solmate's ERC721 contract](https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol). |
| [`src/lib/LlamaUtils.sol`](llama/src/lib/LlamaUtils.sol) | 17 | A library of helper functions used throughout the Llama codebase. |
| [`src/lib/Structs.sol`](llama/src/lib/Structs.sol) | 37 | A file containing the structs that are used in more than one src contract throughout the Llama codebase. |
| [`src/lib/UDVTs.sol`](llama/src/lib/UDVTs.sol) | 2 | A file containing the [UDVTs](https://docs.soliditylang.org/en/v0.8.10/types.html#user-defined-value-types) used throughout the Llama codebase. |
| [`src/llama-scripts/LlamaBaseScript.sol`](llama/src/llama-scripts/LlamaBaseScript.sol) | 12 | A template for creating new llama scripts. |
| [`src/llama-scripts/LlamaGovernanceScript.sol`](llama/src/llama-scripts/LlamaGovernanceScript.sol) | 161 | A script that allows users to aggregate common calls on the core and policy contracts. |
| [`src/llama-scripts/LlamaSingleUseScript.sol`](llama/src/llama-scripts/LlamaSingleUseScript.sol) | 15 | A template script for scripts that should only be run once. |
| [`src/strategies/LlamaAbsolutePeerReview.sol`](llama/src/strategies/LlamaAbsolutePeerReview.sol) | 42 | A Llama strategy that has an absolute threshold for approvals/disapprovals and the action creator cannot approve or disapprove their own actions. |
| [`src/strategies/LlamaAbsoluteQuorum.sol`](llama/src/strategies/LlamaAbsoluteQuorum.sol) | 38 | A Llama strategy that has an absolute threshold for approvals/disapprovals and the action creator can approve or disapprove their own actions. |
| [`src/strategies/LlamaAbsoluteStrategyBase.sol`](llama/src/strategies/LlamaAbsoluteStrategyBase.sol) | 142 | A base contract for Llama strategies to inherit from with absolute approval/disapproal properties. |
| [`src/strategies/LlamaRelativeQuorum.sol`](llama/src/strategies/LlamaRelativeQuorum.sol) | 151 | A Llama strategy in which approval/disapproval thresholds are specified as percentages of total supply and action creators are allowed to cast approvals or disapprovals on their own actions. |
| [`src/LlamaCore.sol`](llama/src/LlamaCore.sol) | 448 | Manages the action process from creation to execution. |
| [`src/LlamaExecutor.sol`](llama/src/LlamaExecutor.sol) | 15 | The exit point of a Llama instance. It calls the target contract during action execution. |
| [`src/LlamaFactory.sol`](llama/src/LlamaFactory.sol) | 157 | Factory for deploying new Llama instances. |
| [`src/LlamaLens.sol`](llama/src/LlamaLens.sol) | 85 | Utility contract to compute Llama contract addresses and permission IDs. |
| [`src/LlamaPolicy.sol`](llama/src/LlamaPolicy.sol) | 260 | An ERC721 contract where each token is non-transferable and has roles assigned to create, approve and disapprove actions. |
| [`src/LlamaPolicyMetadata.sol`](llama/src/LlamaPolicyMetadata.sol) | 75 | Utility contract to compute llama policy metadata. |
| [`src/LlamaPolicyMetadataParamRegistry.sol`](llama/src/LlamaPolicyMetadataParamRegistry.sol) | 38 | Parameter Registry contract for onchain SVG colors and logos |
| [`script/CreateAction.s.sol`](script/CreateAction.s.sol) | 47 | A script to automate action creation on a Llama instance. |
| [`script//DeployLlama.s.sol`](script/DeployLlama.s.sol) | 72 | A script to automate deploying the Llama factory, lens, logic contracts and root Llama instance. |
| [`script/DeployUtils.sol` ](script/DeployUtils.sol) | 181 | A library full of helper functions used throughout the scripts directory |

## Out of scope

The `lib/` directory is out of scope for the audit contest.

# Additional Context

_Describe any novel or unique curve logic or mathematical models implemented in the contracts_

_Sponsor, please confirm/edit the information below._

## Scoping Details

```
- If you have a public code repo, please share it here:  N/A
- How many contracts are in scope?: 27
- Total SLoC for these contracts?: todo
- How many external imports are there?: 5
- How many separate interfaces and struct definitions are there for the contracts within scope?:  7 and 15
- Does most of your code generally use composition or inheritance?: Composition
- How many external calls?: 1
- What is the overall line coverage percentage provided by your tests?: 90
- Is this an upgrade of an existing system? No
- Is there a need to understand a separate part of the codebase / get context in order to audit this part of the protocol?: No
- Please describe required context: N/A
- Does it use an oracle?: No
- Does the token conform to the ERC20 standard?: N/A
- Are there any novel or unique curve logic or mathematical models?: N/A
- Does it use a timelock function?: Yes
- Is it an NFT?: Yes
- Does it have an AMM?: No
- Is it a fork of a popular project?: No
- Does it use rollups?: Yes
- Is it multi-chain?: Yes
- Does it use a side-chain?: No
```

# Tests

TODO: link to README.md for test and installation instructions

_Note: Many wardens run Slither as a first pass for testing. Please document any known errors with no workaround._

TODO: link to slither instructions

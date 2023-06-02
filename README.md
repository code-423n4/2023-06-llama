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

## ⭐️ Sponsor: Edit this README

Under "SPONSORS ADD INFO HERE" heading below, include the following:

- [ ] Modify the bottom of this `README.md` file to describe how your code is supposed to work with links to any relevant documentation and any other criteria/details that the C4 Wardens should keep in mind when reviewing. ([Here's a well-constructed example.](https://github.com/code-423n4/2022-08-foundation#readme))
  - [ ] When linking, please provide all links as full absolute links versus relative links
  - [ ] All information should be provided in markdown format (HTML does not render on Code4rena.com)
- [ ] Under the "Scope" heading, provide the name of each contract and:
  - [ ] source lines of code (excluding blank lines and comments) in each
  - [ ] external contracts called in each
  - [ ] libraries used in each
- [ ] Describe any novel or unique curve logic or mathematical models implemented in the contracts
- [ ] Does the token conform to the ERC-20 standard? In what specific ways does it differ?
- [ ] Describe anything else that adds any special logic that makes your approach unique
- [ ] Identify any areas of specific concern in reviewing the code
- [ ] Optional / nice to have: pre-record a high-level overview of your protocol (not just specific smart contract functions). This saves wardens a lot of time wading through documentation.
- [ ] See also: [this checklist in Notion](https://code4rena.notion.site/Key-info-for-Code4rena-sponsors-f60764c4c4574bbf8e7a6dbd72cc49b4#0cafa01e6201462e9f78677a39e09746)
- [ ] Delete this checklist and all text above the line below when you're ready.

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

*Note for C4 wardens: Anything included in the automated findings output is considered a publicly known issue and is ineligible for awards.*

[ ⭐️ SPONSORS ADD INFO HERE ]

# Overview

Llama is a governance system for onchain organizations. Llama enables organizations to encode access control policies, create rules to execute actions, and manage shared accounts.

# Scope

*List all files in scope in the table below (along with hyperlinks) -- and feel free to add notes here to emphasize areas of focus.*

All files in the `src/` directory are in scope for the audit contest as well as the `.sol` files in `script/`:
| Contract | SLOC | Purpose |  
| ----------- | ----------- | ----------- |
| [`src/accounts/LlamaAccount.sol`](llama/src/accounts/LlamaAccount.sol) | | A minimal proxy contract that is used to hold assets for a Llama instance. |
| [`interfaces/IActionGuard.sol`](llama/interfaces/IActionGuard.sol) | | An interface for Llama action guards. |
| [`interfaces/ILlamaAccount.sol`](llama/interfaces/ILlamaAccount.sol) | | An interface for Llama accounts. |
| [`interfaces/ILlamaStrategy.sol`](llama/interfaces/ILlamaStrategy.sol) | | An interface for Llama strategies. |
| [`lib/Checkpoints.sol`]() | | A modified version of [OpenZeppelin's `Checkpoints.sol`](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/d00acef4059807535af0bd0dd0ddf619747a044b/contracts/utils/Checkpoints.sol). |
| [`lib/Enums.sol`](llama/src/lib/Enums.sol) | | A file containing the enumerables used throughout the Llama codebase. |
| [`lib/ERC721NonTransferableMinimalProxy.sol`](llama/src/lib/ERC721NonTransferableMinimalProxy.sol) | | A modified version of [Solmate's ERC721 contract](https://github.com/transmissions11/solmate/blob/34d20fc027fe8d50da71428687024a29dc01748b/src/tokens/ERC721.sol). |
| [`lib/LlamaUtils.sol`](llama/src/lib/LlamaUtils.sol) | | A library of helper functions used throughout the Llama codebase. |
| [`lib/Structs.sol`](llama/src/lib/Structs.sol) | | A file containing the structs that are used in more than one src contract throughout the Llama codebase. |
| [`lib/UDVTs.sol`](llama/src/lib/UDVTs.sol) | | A file containing the [UDVTs](https://docs.soliditylang.org/en/v0.8.10/types.html#user-defined-value-types) used throughout the Llama codebase. |
| [`llama-scripts/BaseScript.sol`](llama/src/llama-scripts/BaseScript.sol) | | A template for creating new llama scripts. |
| [`llama-scripts/GovernanceScript.sol`](llama/src/llama-scripts/GovernanceScript.sol) | | A script that allows users to aggregate common calls on the core and policy contracts. |
| [`llama-scripts/SingleUseScript.sol`](llama/src/llama-scripts/SingleUseScript.sol) | | A template script for scripts that should only be run once. |
| [`strategies/AbsolutePeerReview.sol`](llama/src/strategies/AbsolutePeerReview.sol) | | - A Llama strategy that has an absolute threshold for approvals/disapprovals and the action creator cannot approve or disapprove their own actions. |
| [`strategies/AbsoluteQuorum.sol`](llama/src/strategies/AbsoluteQuorum.sol) | | - A Llama strategy that has an absolute threshold for approvals/disapprovals and the action creator can approve or disapprove their own actions. |
| [`strategies/AbsoluteStrategyBase.sol`](llama/src/strategies/AbsoluteStrategyBase.sol) | | - A base contract for Llama strategies to inherit from with absolute approval/disapproal properties. |
| [`strategies/RelativeQuorum.sol`](llama/src/strategies/RelativeQuorum.sol) | | - A Llama strategy in which approval/disapproval thresholds are specified as percentages of total supply and action creators are allowed to cast approvals or disapprovals on their own actions. |
| [`LlamaCore.sol`](llama/src/LlamaCore.sol) | | Manages the action process from creation to execution. |
| [`LlamaExecutor.sol`](llama/src/LlamaExecutor.sol) | | The exit point of a Llama instance. It calls the target contract during action execution. |
| [`LlamaFactory.sol`](llama/src/LlamaFactory.sol) | | Factory for deploying new Llama instances. |
| [`LlamaLens.sol`](llama/src/LlamaLens.sol) | | Utility contract to compute Llama contract addresses and permission IDs. |
| [`LlamaPolicy.sol`](llama/src/LlamaPolicy.sol) | | An ERC721 contract where each token is non-transferable and has roles assigned to create, approve and disapprove actions. |
| [`LlamaPolicyMetadata.sol`](llama/src/LlamaPolicyMetadata.sol) | | Utility contract to compute llama policy metadata. |
| [`LlamaPolicyMetadataParamRegistry.sol`](llama/src/LlamaPolicyMetadataParamRegistry.sol) | | Parameter Registry contract for onchain SVG colors and logos |
| [`CreateAction.s.sol`](script/CreateAction.s.sol) | |  A script to automate action creation on a Llama instance. |
| [`DeployLlama.s.sol`](script/DeployLlama.s.sol) | |  A script to automate deploying the Llama factory, lens, logic contracts and root Llama instance. |
| [`DeployUtils.sol` ](script/DeployUtils.sol) | | A library full of helper functions used throughout the scripts directory |

*For line of code counts, we recommend using [cloc](https://github.com/AlDanial/cloc).* 

| Contract | SLOC | Purpose | Libraries used |  
| ----------- | ----------- | ----------- | ----------- |
| [contracts/folder/sample.sol](contracts/folder/sample.sol) | 123 | This contract does XYZ | [`@openzeppelin/*`](https://openzeppelin.com/contracts/) |

## Out of scope

The `lib/` directory is out of scope for the audit contest.

# Additional Context

*Describe any novel or unique curve logic or mathematical models implemented in the contracts*

*Sponsor, please confirm/edit the information below.*

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

*Note: Many wardens run Slither as a first pass for testing.  Please document any known errors with no workaround.* 

TODO: link to slither instructions

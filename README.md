![Llama](https://llama.xyz/images/llama-banner.png)

# Llama Audit Details

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

# Overview

Llama is a governance system for onchain organizations. It uses non-transferable NFTs to encode access control, features programmatic control of funds, and includes a modular framework to define action execution rules.

# Scope

All files in the `src/` directory are in scope for the audit contest as well as the `.sol` files in `script/`:
| Contract | SLOC | Purpose |  
| ----------- | ----------- | ----------- |
| [`src/accounts/LlamaAccount.sol`](llama/src/accounts/LlamaAccount.sol) | 187 | This contract can be used to hold assets for a Llama instance. |
| [`src/interfaces/ILlamaAccount.sol`](llama/interfaces/ILlamaAccount.sol) | 5 | An interface for Llama accounts. |
| [`src/interfaces/ILlamaActionGuard.sol`](llama/interfaces/ILlamaActionGuard.sol) | 7 | An interface for Llama action guards. |
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

We encourage participants to look for bugs in the following areas:

- Unauthorized action state transitions
- Unauthorized approval and disapproval manipulation
- Vulnerabilities in the roles and permissions system in `LlamaPolicy.sol`
- Additional safety checks to help prevent Llama instances entering a state where new actions cannot be executed
- Risks that stem from `LlamaExecutor.sol` delegatecalling scripts

## Out of scope

The `lib/` directory and acknowledged findings from our previous audit are out of scope for this audit contest.


# Assumptions

- The root Llama instance deployed in the `LlamaFactory.sol` constructor will govern Llama instance deployments
- Llama instances are self-governed and standalone

# Build & Tests

## Prerequisites

It requires [Foundry](https://github.com/foundry-rs/foundry) installed to run. You can find instructions here: [Foundry installation](https://book.getfoundry.sh/getting-started/installation).

We use [just](https://github.com/casey/just) to save and run commands. You can find instructions here: [just installation](https://github.com/casey/just#packages).

### VS Code

You can get Solidity support for Visual Studio Code by installing the [Hardhat Solidity extension](https://github.com/NomicFoundation/hardhat-vscode).

## Installation

```sh
$ git clone https://github.com/llamaxyz/llama.git
$ cd llama
$ forge install

# Configure git to ignore commits that aren't relevant to git blame. Read the
# comments in the `.git-blame-ignore-revs` file for more information.
$ git config blame.ignoreRevsFile .git-blame-ignore-revs
```

## Setup

Duplicate `.env.example` and rename to `.env`:

- Add a valid mainnet URL for an Ethereum JSON-RPC client for the `MAINNET_RPC_URL` variable.
- Add a valid Private Key for the `PRIVATE_KEY` variable.
- Add a valid Etherscan API Key for the `ETHERSCAN_API_KEY` variable.

### Commands

- `forge build` - build the project
- `forge test` - run tests

### Deploy and Verify

- `just deploy` - deploy and verify payload on mainnet

To confirm the deploy was successful, re-run your test suite but use the newly created contract address.

## Documentation

Run the following command to generate smart contract reference documentation from this project's NatSpec comments and serve those static files locally:

```sh
$ forge doc -o reference/ -b -s
```

## Slither

Use our bash script to prevent slither from analyzing the test and script directories. Running `slither .` directly will result in an `AssertionError`.

```sh
$ chmod +x slither.sh
$ ./slither.sh
```

![Llama](https://llama.xyz/images/llama-banner.png)

# Llama Audit Details

- Total Prize Pool: $60,500 USDC
  - HM awards: $41,250 USDC
  - Bot Race awards: $3,750 USDC
  - ✨ Analysis awards: $2,500 USDC
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

The results of the [slither report](https://github.com/code-423n4/2023-06-llama/blob/main/.slither-report) and acknowledged findings from our [Spearbit audit](https://github.com/code-423n4/2023-06-llama/blob/main/audits/Llama-Spearbit-Audit.pdf) are out of scope for this audit contest.

# Overview

Llama is a governance system for onchain organizations. It uses non-transferable NFTs to encode access control, features programmatic control of funds, and includes a modular framework to define action execution rules.

## Documentation

The [video explainer](https://www.loom.com/share/2cd1513d4ee34aa8ace6df51e402c259) provides a high-level overview of the Llama system and the [docs](https://github.com/code-423n4/2023-06-llama/blob/main/docs/overview.md) describe the core components.

# Scope

## Files in scope

|File|[SLOC](#nowhere "(nSLOC, SLOC, Lines)")|Description and [Coverage](#nowhere "(Lines hit / Total)")|Libraries|
|:-|:-:|:-|:-|
|_Contracts (14)_|
|[src/LlamaExecutor.sol](https://github.com/code-423n4/2023-06-llama/blob/main/src/LlamaExecutor.sol) [👥](#nowhere "DelegateCall")|[15](#nowhere "(nSLOC:12, SLOC:15, Lines:36)")|The exit point of a Llama instance. It calls the target contract during action execution., &nbsp;&nbsp;[100.00%](#nowhere "(Hit:2 / Total:2)")||
|[src/LlamaPolicyMetadataParamRegistry.sol](https://github.com/code-423n4/2023-06-llama/blob/main/src/LlamaPolicyMetadataParamRegistry.sol)|[38](#nowhere "(nSLOC:38, SLOC:38, Lines:94)")|Parameter Registry contract for onchain SVG colors and logos, &nbsp;&nbsp;[100.00%](#nowhere "(Hit:6 / Total:6)")||
|[src/strategies/LlamaAbsoluteQuorum.sol](https://github.com/code-423n4/2023-06-llama/blob/main/src/strategies/LlamaAbsoluteQuorum.sol)|[38](#nowhere "(nSLOC:30, SLOC:38, Lines:63)")|A Llama strategy that has an absolute threshold for approvals/disapprovals and the action creator can approve or disapprove their own actions., &nbsp;&nbsp;[90.91%](#nowhere "(Hit:10 / Total:11)")| `@openzeppelin/*` `solmate/*`|
|[src/strategies/LlamaAbsolutePeerReview.sol](https://github.com/code-423n4/2023-06-llama/blob/main/src/strategies/LlamaAbsolutePeerReview.sol) [Σ](#nowhere "Unchecked Blocks")|[42](#nowhere "(nSLOC:38, SLOC:42, Lines:83)")|A Llama strategy that has an absolute threshold for approvals/disapprovals and the action creator cannot approve or disapprove their own actions., &nbsp;&nbsp;[100.00%](#nowhere "(Hit:15 / Total:15)")| `@openzeppelin/*` `solmate/*`|
|[src/LlamaPolicyMetadata.sol](https://github.com/code-423n4/2023-06-llama/blob/main/src/LlamaPolicyMetadata.sol)|[75](#nowhere "(nSLOC:71, SLOC:75, Lines:115)")|Utility contract to compute llama policy metadata., &nbsp;&nbsp;[100.00%](#nowhere "(Hit:39 / Total:39)")| `@openzeppelin/*` `solady/*`|
|[src/strategies/LlamaRelativeQuorum.sol](https://github.com/code-423n4/2023-06-llama/blob/main/src/strategies/LlamaRelativeQuorum.sol)|[151](#nowhere "(nSLOC:147, SLOC:151, Lines:327)")|A Llama strategy in which approval/disapproval thresholds are specified as percentages of total supply and action creators are allowed to cast approvals or disapprovals on their own actions., &nbsp;&nbsp;[100.00%](#nowhere "(Hit:60 / Total:60)")| `@openzeppelin/*` `solmate/*`|
|[src/LlamaFactory.sol](https://github.com/code-423n4/2023-06-llama/blob/main/src/LlamaFactory.sol) [🧮](#nowhere "Uses Hash-Functions") [🌀](#nowhere "create/create2")|[157](#nowhere "(nSLOC:133, SLOC:157, Lines:289)")|Factory for deploying new Llama instances., &nbsp;&nbsp;[100.00%](#nowhere "(Hit:29 / Total:29)")| `@openzeppelin/*`|
|[src/llama-scripts/LlamaGovernanceScript.sol](https://github.com/code-423n4/2023-06-llama/blob/main/src/llama-scripts/LlamaGovernanceScript.sol)|[161](#nowhere "(nSLOC:125, SLOC:161, Lines:226)")|A script that allows users to aggregate common calls on the core and policy contracts., &nbsp;&nbsp;[53.45%](#nowhere "(Hit:31 / Total:58)")||
|[src/accounts/LlamaAccount.sol](https://github.com/code-423n4/2023-06-llama/blob/main/src/accounts/LlamaAccount.sol) [🖥](#nowhere "Uses Assembly") [💰](#nowhere "Payable Functions") [👥](#nowhere "DelegateCall")|[187](#nowhere "(nSLOC:182, SLOC:187, Lines:343)")|This contract can be used to hold assets for a Llama instance., &nbsp;&nbsp;[100.00%](#nowhere "(Hit:51 / Total:51)")| `@openzeppelin/*`|
|[src/LlamaPolicy.sol](https://github.com/code-423n4/2023-06-llama/blob/main/src/LlamaPolicy.sol) [Σ](#nowhere "Unchecked Blocks")|[260](#nowhere "(nSLOC:236, SLOC:260, Lines:536)")|An ERC721 contract where each token is non-transferable and has roles assigned to create, approve and disapprove actions., &nbsp;&nbsp;[100.00%](#nowhere "(Hit:104 / Total:104)")| `solady/*`|
|[src/LlamaCore.sol](https://github.com/code-423n4/2023-06-llama/blob/main/src/LlamaCore.sol) [💰](#nowhere "Payable Functions") [🧮](#nowhere "Uses Hash-Functions") [🔖](#nowhere "Handles Signatures: ecrecover") [🌀](#nowhere "create/create2")|[448](#nowhere "(nSLOC:361, SLOC:448, Lines:803)")|Manages the action process from creation to execution., &nbsp;&nbsp;[100.00%](#nowhere "(Hit:143 / Total:143)")| `@openzeppelin/*`|
|_Abstracts (4)_|
|[src/llama-scripts/LlamaBaseScript.sol](https://github.com/code-423n4/2023-06-llama/blob/main/src/llama-scripts/LlamaBaseScript.sol)|[12](#nowhere "(nSLOC:12, SLOC:12, Lines:23)")|A template for creating new llama scripts., &nbsp;&nbsp;-||
|[src/llama-scripts/LlamaSingleUseScript.sol](https://github.com/code-423n4/2023-06-llama/blob/main/src/llama-scripts/LlamaSingleUseScript.sol)|[15](#nowhere "(nSLOC:15, SLOC:15, Lines:27)")|A template script for scripts that should only be run once., &nbsp;&nbsp;-||
|[src/lib/ERC721NonTransferableMinimalProxy.sol](https://github.com/code-423n4/2023-06-llama/blob/main/src/lib/ERC721NonTransferableMinimalProxy.sol) [Σ](#nowhere "Unchecked Blocks")|[96](#nowhere "(nSLOC:96, SLOC:96, Lines:192)")|A modified version of Solmate's ERC721 contract., &nbsp;&nbsp;[72.22%](#nowhere "(Hit:26 / Total:36)")| `@openzeppelin/*`|
|[src/strategies/LlamaAbsoluteStrategyBase.sol](https://github.com/code-423n4/2023-06-llama/blob/main/src/strategies/LlamaAbsoluteStrategyBase.sol)|[141](#nowhere "(nSLOC:131, SLOC:141, Lines:307)")|A base contract for Llama strategies to inherit from with absolute approval/disapproval properties., &nbsp;&nbsp;[86.00%](#nowhere "(Hit:43 / Total:50)")| `@openzeppelin/*` `solmate/*`|
|_Libraries (3)_|
|[src/lib/LlamaUtils.sol](https://github.com/code-423n4/2023-06-llama/blob/main/src/lib/LlamaUtils.sol) [Σ](#nowhere "Unchecked Blocks")|[17](#nowhere "(nSLOC:17, SLOC:17, Lines:27)")|A library of helper functions used throughout the Llama codebase., &nbsp;&nbsp;[0.00%](#nowhere "(Hit:0 / Total:5)")||
|[src/lib/Checkpoints.sol](https://github.com/code-423n4/2023-06-llama/blob/main/src/lib/Checkpoints.sol) [🖥](#nowhere "Uses Assembly")|[159](#nowhere "(nSLOC:131, SLOC:159, Lines:286)")|A modified version of OpenZeppelin's Checkpoints.sol., &nbsp;&nbsp;[51.43%](#nowhere "(Hit:36 / Total:70)")||
|_Interfaces (3)_|
|[src/interfaces/ILlamaAccount.sol](https://github.com/code-423n4/2023-06-llama/blob/main/src/interfaces/ILlamaAccount.sol)|[5](#nowhere "(nSLOC:5, SLOC:5, Lines:19)")|An interface for Llama accounts., &nbsp;&nbsp;-||
|[src/interfaces/ILlamaActionGuard.sol](https://github.com/code-423n4/2023-06-llama/blob/main/src/interfaces/ILlamaActionGuard.sol)|[7](#nowhere "(nSLOC:7, SLOC:7, Lines:32)")|An interface for Llama strategies., &nbsp;&nbsp;-||
|[src/interfaces/ILlamaStrategy.sol](https://github.com/code-423n4/2023-06-llama/blob/main/src/interfaces/ILlamaStrategy.sol)|[23](#nowhere "(nSLOC:20, SLOC:23, Lines:108)")|An interface for Llama strategies., &nbsp;&nbsp;-||
|_Structs (1)_|
|[src/lib/Structs.sol](https://github.com/code-423n4/2023-06-llama/blob/main/src/lib/Structs.sol)|[37](#nowhere "(nSLOC:37, SLOC:37, Lines:54)")|A file containing the structs that are used in more than one src contract throughout the Llama codebase., &nbsp;&nbsp;-||
|_Other (2)_|
|[src/lib/UDVTs.sol](https://github.com/code-423n4/2023-06-llama/blob/main/src/lib/UDVTs.sol)|[2](#nowhere "(nSLOC:2, SLOC:2, Lines:5)")|A file containing the UDVTs used throughout the Llama codebase., &nbsp;&nbsp;-||
|[src/lib/Enums.sol](https://github.com/code-423n4/2023-06-llama/blob/main/src/lib/Enums.sol)|[10](#nowhere "(nSLOC:10, SLOC:10, Lines:12)")|A file containing the enumerables used throughout the Llama codebase., &nbsp;&nbsp;-||
|Total (over 27 files):| [2096](#nowhere "(nSLOC:2223, SLOC:2096, Lines:4595)") |[74.15%](#nowhere "Hit:611 / Total:824")|

## Out of scope

|File|[SLOC](#nowhere "(nSLOC, SLOC, Lines)")|Description and [Coverage](#nowhere "(Lines hit / Total)")|Libraries|
|:-|:-:|:-|:-|
|_Contracts (1)_|
|[src/LlamaLens.sol](https://github.com/code-423n4/2023-06-llama/blob/main/src/LlamaLens.sol) [🧮](#nowhere "Uses Hash-Functions")|[85](#nowhere "(nSLOC:77, SLOC:85, Lines:169)")|Utility contract to compute Llama contract addresses and permission IDs., &nbsp;&nbsp;[80.00%](#nowhere "(Hit:16 / Total:20)")| `@openzeppelin/*`|

## External imports

* **@openzeppelin/proxy/Clones.sol**
  * [src/LlamaCore.sol](https://github.com/code-423n4/2023-06-llama/blob/main/src/LlamaCore.sol)
  * [src/LlamaFactory.sol](https://github.com/code-423n4/2023-06-llama/blob/main/src/LlamaFactory.sol)
* **@openzeppelin/proxy/utils/Initializable.sol**
  * [src/LlamaCore.sol](https://github.com/code-423n4/2023-06-llama/blob/main/src/LlamaCore.sol)
  * [src/accounts/LlamaAccount.sol](https://github.com/code-423n4/2023-06-llama/blob/main/src/accounts/LlamaAccount.sol)
  * [src/lib/ERC721NonTransferableMinimalProxy.sol](https://github.com/code-423n4/2023-06-llama/blob/main/src/lib/ERC721NonTransferableMinimalProxy.sol)
  * [src/strategies/LlamaAbsolutePeerReview.sol](https://github.com/code-423n4/2023-06-llama/blob/main/src/strategies/LlamaAbsolutePeerReview.sol)
  * [src/strategies/LlamaAbsoluteQuorum.sol](https://github.com/code-423n4/2023-06-llama/blob/main/src/strategies/LlamaAbsoluteQuorum.sol)
  * [src/strategies/LlamaAbsoluteStrategyBase.sol](https://github.com/code-423n4/2023-06-llama/blob/main/src/strategies/LlamaAbsoluteStrategyBase.sol)
  * [src/strategies/LlamaRelativeQuorum.sol](https://github.com/code-423n4/2023-06-llama/blob/main/src/strategies/LlamaRelativeQuorum.sol)
* **@openzeppelin/token/ERC1155/IERC1155.sol**
  * [src/accounts/LlamaAccount.sol](https://github.com/code-423n4/2023-06-llama/blob/main/src/accounts/LlamaAccount.sol)
* **@openzeppelin/token/ERC1155/utils/ERC1155Holder.sol**
  * [src/accounts/LlamaAccount.sol](https://github.com/code-423n4/2023-06-llama/blob/main/src/accounts/LlamaAccount.sol)
* **@openzeppelin/token/ERC20/IERC20.sol**
  * [src/accounts/LlamaAccount.sol](https://github.com/code-423n4/2023-06-llama/blob/main/src/accounts/LlamaAccount.sol)
* **@openzeppelin/token/ERC20/utils/SafeERC20.sol**
  * [src/accounts/LlamaAccount.sol](https://github.com/code-423n4/2023-06-llama/blob/main/src/accounts/LlamaAccount.sol)
* **@openzeppelin/token/ERC721/IERC721.sol**
  * [src/accounts/LlamaAccount.sol](https://github.com/code-423n4/2023-06-llama/blob/main/src/accounts/LlamaAccount.sol)
* **@openzeppelin/token/ERC721/utils/ERC721Holder.sol**
  * [src/accounts/LlamaAccount.sol](https://github.com/code-423n4/2023-06-llama/blob/main/src/accounts/LlamaAccount.sol)
* **@openzeppelin/utils/Address.sol**
  * [src/accounts/LlamaAccount.sol](https://github.com/code-423n4/2023-06-llama/blob/main/src/accounts/LlamaAccount.sol)
* **@openzeppelin/utils/Base64.sol**
  * [src/LlamaPolicyMetadata.sol](https://github.com/code-423n4/2023-06-llama/blob/main/src/LlamaPolicyMetadata.sol)
* **@solady/utils/LibString.sol**
  * [src/LlamaPolicy.sol](https://github.com/code-423n4/2023-06-llama/blob/main/src/LlamaPolicy.sol)
  * [src/LlamaPolicyMetadata.sol](https://github.com/code-423n4/2023-06-llama/blob/main/src/LlamaPolicyMetadata.sol)
* **@solmate/utils/FixedPointMathLib.sol**
  * [src/strategies/LlamaAbsolutePeerReview.sol](https://github.com/code-423n4/2023-06-llama/blob/main/src/strategies/LlamaAbsolutePeerReview.sol)
  * [src/strategies/LlamaAbsoluteQuorum.sol](https://github.com/code-423n4/2023-06-llama/blob/main/src/strategies/LlamaAbsoluteQuorum.sol)
  * [src/strategies/LlamaAbsoluteStrategyBase.sol](https://github.com/code-423n4/2023-06-llama/blob/main/src/strategies/LlamaAbsoluteStrategyBase.sol)
  * [src/strategies/LlamaRelativeQuorum.sol](https://github.com/code-423n4/2023-06-llama/blob/main/src/strategies/LlamaRelativeQuorum.sol)

We encourage participants to look for bugs in the following areas:

- Unauthorized action state transitions.
- Unauthorized approval and disapproval manipulation.
- Vulnerabilities in the roles and permissions system in `LlamaPolicy.sol`.
- Additional safety checks to help prevent Llama instances entering a state where new actions cannot be executed.
- Risks that stem from `LlamaExecutor.sol` delegatecalling scripts.
- Vulnerabilities in `LlamaAccount.sol` that could lead to unauthorized access to funds and assets held in it (Especially risks that stem from the `LlamaAccount.sol` being able to delegatecall arbitrary contracts).

## Scoping Details

- If you have a public code repo, please share it here: N/A
- How many contracts are in scope?: 16
- Total SLoC for these contracts?: 1750
- How many external imports are there?: 5
- How many separate interfaces and struct definitions are there for the contracts within scope?: 7 and 15
- Does most of your code generally use composition or inheritance?: Composition
- How many external calls?: 1
- What is the overall line coverage percentage provided by your tests?: 90
- Is this an upgrade of an existing system? No
- Is there a need to understand a separate part of the codebase / get context in order to audit this part of the protocol?: No
- Please describe required context: N/A
- Does it use an oracle?: No
- Does the token conform to the ERC20 standard?:
- Are there any novel or unique curve logic or mathematical models?: N/A
- Does it use a timelock function?: Yes
- Is it an NFT?: Yes
- Does it have an AMM?:
- Is it a fork of a popular project?: No
- Does it use rollups?: Yes
- Is it multi-chain?: Yes
- Does it use a side-chain?: No

## Out of scope

The `lib/` directory and acknowledged findings from our previous audit are out of scope for this audit contest.

# Assumptions

- The root Llama instance deployed in the `LlamaFactory.sol` constructor will govern Llama instance deployments.
- Llama instances are self-governed and standalone.

# Build & Tests

## Quickstart Command

```sh
foundryup && export MAINNET_RPC_URL='<YOUR_MAINNET_RPC_URL>' && git clone https://github.com/code-423n4/2023-06-llama && cd 2023-06-llama && forge test
```

## Prerequisites

It requires [Foundry](https://github.com/foundry-rs/foundry) installed to run. You can find instructions here: [Foundry installation](https://book.getfoundry.sh/getting-started/installation).

We use [just](https://github.com/casey/just) to save and run commands. You can find instructions here: [just installation](https://github.com/casey/just#packages).

### VS Code

You can get Solidity support for Visual Studio Code by installing the [Hardhat Solidity extension](https://github.com/NomicFoundation/hardhat-vscode).

## Installation

```sh
$ git clone https://github.com/code-423n4/2023-06-llama
$ cd 2023-06-llama
$ forge install
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

## Reference

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

# ZkSwapGuard

**ZkSwapGuard** is a cutting-edge project built on the zkSync Era that empowers users with secure and efficient token swaps on decentralized exchanges (DEXs). With a focus on account abstraction and gas fee management, ZkSwapGuard offers a robust and user-friendly solution for trading crypto assets conservatively and cost-effectively.

## Features

- **Swap with Confidence**: ZkSwapGuard allows users to swap tokens on AMM DEXs with confidence, thanks to built-in safeguards such as token whitelisting, daily trade size limits, and maximum size per trade.

- **GasPond Integration**: Seamlessly manage gas fees using GasPond, a Paymaster-as-a-Service that offers both free transactions and gas payments in ERC20 tokens. Choose your preferred gas payment method.

- **Flexible Account Abstraction**: Leverage native account abstraction in zkSync Era for flexible and efficient transactions, including support for EOAs (Externally Owned Accounts) and Smart Contract accounts.

- **Sponsor Model**: Empower projects and individuals to sponsor users with easy registration, ETH deposits, and configurations. Reduce the cost of meta-transactions and improve the user experience.

## Overview

This project implements an Account Abstraction wallet contract with multiple unique features: swap & trade size limit, multicall, and meta-transaction via paymaster.

Contract: [`Account.sol`](https://github.com/ZkSwapGuard/swap/blob/5903181b50b369df3d22de9e3501cd16075bbf09/src/aa-wallet/Account.sol) & [`AccountFacotory.sol`](https://github.com/ZkSwapGuard/swap/blob/5903181b50b369df3d22de9e3501cd16075bbf09/src/aa-wallet/AccountFactory.sol)

## Account Trade Limit & Swap Module

Modules serve as peripheral helpers and give AA-accounts extensional functionalities and protective limitations. This project has a module called SwapModule that allows accounts to swap tokens on AMM DEX with limitations such as token whitelist, daily trade size limit, and maximum size per trade. This module is recommended for non-veteran users who ought to trade crypto assets as conservatively as possible.

### SwapModule

Contracts: [`SwapModuleUniV2.sol`](https://github.com/ZkSwapGuard/swap/blob/main/zksync/src/aa-wallet/modules/swapModule/SwapModuleUnV2.sol) & [`SwapModuleBase.sol`](https://github.com/ZkSwapGuard/swap/blob/main/zksync/src/aa-wallet/modules/swapModule/SwapModuleBase.sol)

- SwapModuleUniV2 is an executor, delegatecalled from Account and executes swap methods on an UniswapV2's router contract.
- SwapModuleBase is a base for executors, being in charge of all the validations logic and storing variables.

The rationale for deploying SwapModuleBase separately is that 1) with delegatecall, variables stored in SwapModuleUniV2 can't be read and used for validation logics, and 2) SwapModuleBase should serve as a single base for multiple executors since SwapModule can integrate multiple DEXs by deploying new executors contracts.

## GasPond(Paymaster)

Contract: [`GasPond.sol`](https://github.com/ZkSwapGuard/swap/blob/main/zksync/src/aa-wallet/paymaster/GasPond.sol)

A Paymaster contract called GasPond allows both free transactions and gas payments in ERC20 for accounts. GasPond also supports accounts' transactions via modules such as SwapModule if enabled.

### Sponsor Model

GasPond employs the sponsor model instead of the general paymaster model, meaning that the deployer/owner of the GasPond contract isn't a "paymaster" who compensates gas costs and accepts ERC20 gas payments. Rather, GasPond is a Paymaster-as-a-Service: any individual and project can become a sponsor on GasPond to sponsor users with easy registration, ETH deposit, and configurations.

This model could drastically reduce the cost of providing meta-transactions so that crypto services can improve the UX of their products more easily. Additionally, users don't have to rely on multiple different paymaster contracts where vulnerabilities and malicious bugs might exist.

### An example of the use of GasPond for a project

A project called XYZinc becomes a sponsor on GasPond, aiming to increase trading activities and awareness of their native token XYZ. As such, for purchases of XYZ tokens on DEXs supported by SwapModule, it accepts gas payments paid in XYZ with a 50% discount.

## Multicall

Contract: [`Multicall`](https://github.com/ZkSwapGuard/swap/blob/main/zksync/src/aa-wallet/libraries/Multicall.sol)

Account inherits Multicall contract that allows the account to perform both call and delegatecall in the same transaction. It also supports call/delegatecall to module contracts so that the account can combine any arbitrary logic in modules with methods in external contracts.

For example, 1) approve tx to an ERC20 token 2) swap tx via SwapModule.

## Demo with Flow

https://github.com/ZkSwapGuard/swap/assets/61940373/35bd1165-c8d5-465c-a158-dd2e7c78fe5a

![224521483-df2a6cad-c653-4a16-91fb-6e73de31a1db](https://github.com/ZkSwapGuard/swap/assets/61940373/73a7eeb8-0b5d-4520-b06d-fb9de5fa29bf)

## Getting Started

### Prerequisites

Before you begin, ensure you have met the following requirements:

- [Node.js](https://nodejs.org/) installed
- [Yarn](https://yarnpkg.com/) package manager installed
- Docker and `docker-compose` for setting up a local zkSync network (if needed)

### Installation

1. Clone the repository:

   ```sh
   git clone https://github.com/ZkSwapGuard.git
   cd ZkSwapGuard
   ```

2. Install dependencies:

   ```sh
   yarn
   ```


## Usage

### Connecting Wallet & Account

To use ZkSwapGuard, follow these steps:

1. Start the frontend:

   ```sh
   cd frontend
   yarn start
   ```

2. Connect your wallet to an Account Abstraction account by providing the deployed account address (0xD1180...) and following the on-screen instructions.

3. Once connected, you can view account trade limits and perform token swaps subject to SwapModule limitations. Input the desired swap amount and follow the instructions.

## License

This project is licensed under the [MIT License](LICENSE.md).

---

Feel free to add more sections or details to the README as needed for your project. The goal is to provide clear instructions and information for users and contributors to understand and use ZkSwapGuard effectively.

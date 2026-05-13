# Algorand Starter: TypeScript Contracts

A minimal, cloneable TypeScript starter for trying the AlgoKit Utils beta.

This starter is the smallest path to:

- write an Algorand TypeScript contract
- compile it to TEAL
- generate a typed client
- run unit and e2e tests
- deploy to LocalNet, TestNet, or MainNet

## What This Uses

- `@algorandfoundation/algokit-utils` `10.0.0-beta.2`
- `@algorandfoundation/algorand-typescript` `1.2.0-beta.30`
- `@algorandfoundation/puya-ts` `1.2.0-beta.30`

## Prerequisites

- [Node.js](https://nodejs.org/) `>= 24.0`
- [AlgoKit CLI](https://github.com/algorandfoundation/algokit-cli) via `pipx install algokit`
- [Docker](https://www.docker.com/) for LocalNet

## Quick Start

```bash
git clone <repo-url>
cd algokit-utils-beta-starter-ts

cp .env.localnet.example .env.localnet
npm install
algokit localnet start
npm test
npm run deploy
```

Expected deploy output:

```text
Deployed HelloWorld: APP_ID=<number>
```

## What You Get

```text
src/
  hello-world.algo.ts          # Smart contract source
  hello-world.algo.spec.ts     # Unit test (no network)
  hello-world.e2e.spec.ts      # E2E test (deploys to LocalNet)
artifacts/                     # TEAL, ARC-32/56 specs, typed client
deploy.ts                      # Deployment entry point
.env.localnet.example          # LocalNet algod + indexer config
.env.testnet.example           # TestNet config template
.env.mainnet.example           # MainNet config template
package.json                   # Build, test, and deploy scripts
```

## Commands

| Command                  | Description                                                |
| ------------------------ | ---------------------------------------------------------- |
| `npm run build`          | Compile contracts and generate the typed TypeScript client |
| `npm test`               | Build first, then run unit and e2e tests                   |
| `npm run check-types`    | Run TypeScript type-checking                               |
| `npm run deploy`         | Build and deploy to LocalNet                               |
| `npm run deploy:testnet` | Build and deploy to TestNet                                |
| `npm run deploy:mainnet` | Build and deploy to MainNet                                |

## How Build And Deploy Work

`npm run build` does two things:

1. `algokit compile ts src --output-source-map --out-dir ../artifacts`
2. `algokit generate client artifacts --version 7.0.0-beta.3 --output {app_spec_dir}/{contract_name}Client.ts`

That gives you committed artifacts in `artifacts/`, including:

- TEAL approval and clear programs
- ARC-32 and ARC-56 app specs
- `HelloWorldClient.ts`

`npm run deploy` uses `AlgorandClient.fromEnvironment()` in `deploy.ts`. On LocalNet it uses the default dispenser account. On TestNet and MainNet it reads `DEPLOYER_MNEMONIC` from the selected env file.

## Network Setup

LocalNet:

```bash
cp .env.localnet.example .env.localnet
algokit localnet start
npm run deploy
```

TestNet:

```bash
cp .env.testnet.example .env.testnet
# set DEPLOYER_MNEMONIC in .env.testnet
npm run deploy:testnet
```

MainNet:

```bash
cp .env.mainnet.example .env.mainnet
# set DEPLOYER_MNEMONIC in .env.mainnet
npm run deploy:mainnet
```

No `.env` files are committed. Only `.env*.example` templates are tracked.

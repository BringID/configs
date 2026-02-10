# BringID Widget Configs

## Overview

This repository contains remote JSON configuration files consumed by the BringID widget at runtime. The widget fetches these files to determine which verification tasks to display and which on-chain contracts to interact with.

## Repository Structure

```
configs.json                  # Production network config (Base mainnet)
dev-configs.json              # Development network config (Base Sepolia)
tasks.json                    # Production tasks (Base mainnet)
tasks-sepolia.json            # Production tasks (Base Sepolia)
tasks-sepolia-staging.json    # Staging tasks (Base Sepolia) — next-release features
```

## File Categories

### Network Configs (`configs.json`, `dev-configs.json`)

Contain the Registry smart contract address and chain ID. These tell the widget which blockchain network and contract to use.

### Task Configs (`tasks*.json`)

JSON arrays of task objects. Each task defines a verification method the user can complete in the widget. Tasks have three verification types:

- **`auth`** — External service authentication (verificationUrl points to a standalone auth service)
- **`oauth`** — OAuth flow via BringID's OAuth API (verificationUrl is a relative path)
- **`zktls`** — Browser extension MPC-TLS verification (uses permissionUrl array and steps array)

Each task contains `groups` — scoring tiers that map to on-chain Semaphore groups and credential groups in the Registry contract.

### Environments

- **Production (mainnet):** `configs.json` + `tasks.json`
- **Production (testnet):** `dev-configs.json` + `tasks-sepolia.json`
- **Staging (testnet):** `dev-configs.json` + `tasks-sepolia-staging.json`

The staging file includes tasks under development that will ship in the next release. It is a superset of `tasks-sepolia.json`.

## Conventions

- Task IDs are strings. OAuth/auth tasks use low IDs (`"0"`, `"1"`, `"2"`, ...), zktls tasks use IDs starting from `"100"`.
- The `icon` field must match an icon key registered in the widget's icon set.
- `verificationUrl` for `auth` tasks is an absolute URL; for `oauth` tasks it is a relative path resolved against the OAuth API base.
- `permissionUrl` patterns use glob syntax (e.g. `https://example.com/*`) and are only present on `zktls` tasks.
- Group `checks` are optional conditions evaluated against service response data (key, comparison type, threshold value).

## Editing Guidelines

- Do not change `semaphoreGroupId` or `credentialGroupId` values without coordinating with the smart contract and backend teams — these reference on-chain state.
- When adding a new task to staging, add it to `tasks-sepolia-staging.json` first. Promote to `tasks-sepolia.json` and `tasks.json` only after QA sign-off.
- Keep mainnet and Sepolia task structures identical (same fields, same verification logic) — only IDs and contract references differ.

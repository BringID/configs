# BringID Widget Configs

Remote configuration files for the [BringID](https://bringid.org) widget. The widget fetches these configs at runtime to determine available verification tasks and on-chain contract addresses.

## File Structure

### Network Configs

- **`configs.json`** — Production config for Base mainnet (chain ID `8453`). Contains the on-chain Registry contract address and chain ID.
- **`dev-configs.json`** — Development config for Base Sepolia testnet (chain ID `84532`). Same structure as production, pointing to testnet contracts.

### Task Configs

Task files define the list of verification tasks displayed in the widget. Each task describes a service, its verification method, scoring tiers, and associated on-chain group IDs.

- **`tasks.json`** — Production tasks for Base mainnet.
- **`tasks-sepolia.json`** — Production tasks for Base Sepolia testnet.
- **`tasks-sepolia-staging.json`** — Staging tasks for Base Sepolia. Contains upcoming tasks that are not yet released to production (e.g. extension-based verifications, new auth/oauth providers). Used for QA and pre-release testing.

## Task Schema

Each task is a JSON object with the following fields:

| Field | Type | Description |
|---|---|---|
| `id` | `string` | Unique task identifier |
| `title` | `string` | Display name shown in the widget |
| `service` | `string` | Service identifier |
| `description` | `string` | Short description shown to the user |
| `icon` | `string` | Icon key used by the widget |
| `verificationType` | `string` | One of `auth`, `oauth`, or `zktls` |
| `verificationUrl` | `string` | URL or path for auth/oauth verification flows |
| `permissionUrl` | `string[]` | (zktls only) URL patterns the extension needs access to |
| `groups` | `array` | Scoring tiers with on-chain group references |
| `steps` | `array` | (zktls only) UI steps shown during the verification flow |

### Verification Types

- **`auth`** — Direct authentication with an external service (e.g. Farcaster sign-in, passport verification).
- **`oauth`** — OAuth 2.0 flow handled by BringID's OAuth API (e.g. GitHub, Twitter/X).
- **`zktls`** — Browser extension-based MPC-TLS verification against a target website (e.g. Uber, Binance).

### Groups

Each task contains one or more groups representing scoring tiers:

| Field | Type | Description |
|---|---|---|
| `points` | `number` | Reputation points awarded |
| `semaphoreGroupId` | `string` | On-chain Semaphore group ID |
| `credentialGroupId` | `string` | Credential group ID in the Registry |
| `checks` | `array` | (optional) Conditions that must be met (e.g. minimum score threshold) |

## Network Config Schema

| Field | Type | Description |
|---|---|---|
| `REGISTRY` | `string` | Ethereum address of the Registry contract |
| `CHAIN_ID` | `string` | Target chain ID |

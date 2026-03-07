---
name: programmable-secrets
description: Use this skill when operating the Programmable Secrets contracts and released CLI for dataset registration, policy creation, preview-first writes, UAID-gated purchases, receipt checks, Robinhood or Arbitrum testnet workflows, local KRS helpers, profiles, templates, and machine-readable agent execution.
homepage: https://github.com/hashgraph-online/programmable-secrets-contracts
license: Apache-2.0
---

# Programmable Secrets

Use this skill when you need to operate the Programmable Secrets contract stack for finance-agent workflows.

Core repository:
- https://github.com/hashgraph-online/programmable-secrets-contracts

Primary CLI entrypoint:
```bash
npx programmable-secret help
```

Inside the contracts repo, the equivalent local wrapper is:

```bash
pnpm run cli -- help
```

## Operator Defaults

For automated agents, default to the machine-readable surface:

```bash
npx programmable-secret contracts --agent-safe
npx programmable-secret datasets list --json
```

Use `--profile` for named operator roles and `--interactive` only when a human is driving the session.

## Golden Path

Start with the guided operator flow:

```bash
npx programmable-secret init
npx programmable-secret doctor
```

If wallet or broker values are missing, bootstrap the local env file from the running Docker broker:

```bash
npx programmable-secret env-bootstrap
```

If `.env.local` already exists, write an alternate env file instead:

```bash
PROGRAMMABLE_SECRETS_ENV_OUTPUT_PATH=/tmp/programmable-secrets.env npx programmable-secret env-bootstrap
```

## Guided Workflows

Robinhood testnet is the default workflow network.

Direct identity flow:

```bash
npx programmable-secret flow:direct
```

Registry Broker-backed identity flow:

```bash
npx programmable-secret flow:broker
```

To target Arbitrum Sepolia:

```bash
PROGRAMMABLE_SECRETS_NETWORK=arbitrum-sepolia npx programmable-secret flow:direct
PROGRAMMABLE_SECRETS_NETWORK=arbitrum-sepolia npx programmable-secret flow:broker
```

## Contract Commands

Show deployed addresses:

```bash
npx programmable-secret contracts
```

Operate datasets:

```bash
npx programmable-secret datasets list
npx programmable-secret datasets get --dataset-id 1
npx programmable-secret datasets export --dataset-id 1 --output dataset-1.json
npx programmable-secret datasets register --provider-uaid "did:uaid:hol:quantlab?uid=quantlab&registry=hol&proto=hol&nativeId=quantlab" --metadata-json '{"title":"TSLA feed"}' --ciphertext "encrypted payload" --key-material "wrapped key"
npx programmable-secret datasets import --file dataset-1.json --preview
npx programmable-secret datasets set-active --dataset-id 1 --active false
```

Operate policies:

```bash
npx programmable-secret policies list
npx programmable-secret policies get --policy-id 1
npx programmable-secret policies export --policy-id 1 --output policy-1.json
npx programmable-secret policies create-timebound --dataset-id 1 --price-eth 0.00001 --duration-hours 24 --metadata-json '{"title":"24 hour access"}'
npx programmable-secret policies create-uaid --dataset-id 1 --price-eth 0.00001 --duration-hours 24 --required-buyer-uaid uaid:aid:... --agent-id 97
npx programmable-secret policies import --file policy-1.json --preview
npx programmable-secret policies update --policy-id 1 --price-eth 0.00002 --active true --metadata-json '{"title":"Updated access"}'
npx programmable-secret policies allowlist --policy-id 1 --accounts 0xabc,0xdef --allowed true
```

Purchase and verify access:

```bash
npx programmable-secret preview purchase --policy-id 1
npx programmable-secret purchase --policy-id 1
npx programmable-secret access policy --policy-id 1 --buyer 0x...
npx programmable-secret access dataset --dataset-id 1 --buyer 0x...
npx programmable-secret receipts get --receipt-id 1
```

Register an ERC-8004 identity:

```bash
npx programmable-secret identity register --agent-uri https://hol.org/agents/volatility-trading-agent-custodian
```

Profiles, templates, and completions:

```bash
npx programmable-secret profiles list
npx programmable-secret profiles show --profile robinhood-agent --json
npx programmable-secret templates list
npx programmable-secret templates show --name finance-timebound-dataset --json
npx programmable-secret completions zsh
```

Local KRS bundle helpers:

```bash
npx programmable-secret krs encrypt --plaintext '{"signal":"buy","market":"TSLA"}' --output bundle.json
npx programmable-secret krs verify --bundle-file bundle.json --policy-id 1 --buyer 0x...
npx programmable-secret krs decrypt --bundle-file bundle.json
```

## Operator Rules

- Treat `deployments/robinhood-testnet.json` and `deployments/arbitrum-sepolia.json` as the source of truth for deployed addresses.
- Do not read `.env` files directly. Use `doctor` and `env-bootstrap`.
- Use `ETH_PK_2` for provider-side writes and `ETH_PK` for agent-side writes unless you intentionally override with `--wallet`.
- Prefer Robinhood testnet for the primary operator path and Arbitrum Sepolia for ERC-8004 verification when needed.
- Prefer `--json` or `--agent-safe` when another agent or tool will consume the output.
- Use `preview` before any state-changing command in an automated workflow.

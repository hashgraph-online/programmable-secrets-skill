---
name: programmable-secrets
description: Use this skill when operating the Programmable Secrets contracts and CLI for dataset registration, policy creation, UAID-gated purchases, receipt checks, and Robinhood or Arbitrum testnet workflows.
homepage: https://github.com/hashgraph-online/programmable-secrets-contracts
license: Apache-2.0
---

# Programmable Secrets

Use this skill when you need to operate the Programmable Secrets contract stack for finance-agent workflows.

Core repository:
- https://github.com/hashgraph-online/programmable-secrets-contracts

Primary CLI entrypoint:
```bash
npx github:hashgraph-online/programmable-secrets-contracts help
```

## Golden Path

Start with the guided operator flow:

```bash
npx github:hashgraph-online/programmable-secrets-contracts start
npx github:hashgraph-online/programmable-secrets-contracts doctor
```

If wallet or broker values are missing, bootstrap the local env file from the running Docker broker:

```bash
npx github:hashgraph-online/programmable-secrets-contracts env-bootstrap
```

## Guided Workflows

Robinhood testnet is the default workflow network.

Direct identity flow:

```bash
npx github:hashgraph-online/programmable-secrets-contracts flow:direct
```

Registry Broker-backed identity flow:

```bash
npx github:hashgraph-online/programmable-secrets-contracts flow:broker
```

To target Arbitrum Sepolia:

```bash
PROGRAMMABLE_SECRETS_NETWORK=arbitrum-sepolia npx github:hashgraph-online/programmable-secrets-contracts flow:direct
PROGRAMMABLE_SECRETS_NETWORK=arbitrum-sepolia npx github:hashgraph-online/programmable-secrets-contracts flow:broker
```

## Contract Commands

Show deployed addresses:

```bash
npx github:hashgraph-online/programmable-secrets-contracts contracts
```

Operate datasets:

```bash
npx github:hashgraph-online/programmable-secrets-contracts datasets list
npx github:hashgraph-online/programmable-secrets-contracts datasets get --dataset-id 1
npx github:hashgraph-online/programmable-secrets-contracts datasets register --provider-uaid "did:uaid:hol:quantlab?uid=quantlab&registry=hol&proto=hol&nativeId=quantlab" --metadata-json '{"title":"TSLA feed"}' --ciphertext "encrypted payload" --key-material "wrapped key"
npx github:hashgraph-online/programmable-secrets-contracts datasets set-active --dataset-id 1 --active false
```

Operate policies:

```bash
npx github:hashgraph-online/programmable-secrets-contracts policies list
npx github:hashgraph-online/programmable-secrets-contracts policies get --policy-id 1
npx github:hashgraph-online/programmable-secrets-contracts policies create-timebound --dataset-id 1 --price-eth 0.00001 --duration-hours 24 --metadata-json '{"title":"24 hour access"}'
npx github:hashgraph-online/programmable-secrets-contracts policies create-uaid --dataset-id 1 --price-eth 0.00001 --duration-hours 24 --required-buyer-uaid uaid:aid:... --agent-id 97
npx github:hashgraph-online/programmable-secrets-contracts policies update --policy-id 1 --price-eth 0.00002 --active true --metadata-json '{"title":"Updated access"}'
npx github:hashgraph-online/programmable-secrets-contracts policies allowlist --policy-id 1 --accounts 0xabc,0xdef --allowed true
```

Purchase and verify access:

```bash
npx github:hashgraph-online/programmable-secrets-contracts purchase --policy-id 1
npx github:hashgraph-online/programmable-secrets-contracts access policy --policy-id 1 --buyer 0x...
npx github:hashgraph-online/programmable-secrets-contracts access dataset --dataset-id 1 --buyer 0x...
npx github:hashgraph-online/programmable-secrets-contracts receipts get --receipt-id 1
```

Register an ERC-8004 identity:

```bash
npx github:hashgraph-online/programmable-secrets-contracts identity register --agent-uri https://hol.org/agents/volatility-trading-agent-custodian
```

## Operator Rules

- Treat `deployments/robinhood-testnet.json` and `deployments/arbitrum-sepolia.json` as the source of truth for deployed addresses.
- Do not read `.env` files directly. Use `doctor` and `env-bootstrap`.
- Use `ETH_PK_2` for provider-side writes and `ETH_PK` for agent-side writes unless you intentionally override with `--wallet`.
- Prefer Robinhood testnet for the primary operator path and Arbitrum Sepolia for ERC-8004 verification when needed.

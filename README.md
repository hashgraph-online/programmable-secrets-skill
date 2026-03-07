# Programmable Secrets Skill

[![HOL Registry skill badge](https://hol.org/registry/api/v1/skills/badge.svg?name=programmable-secrets&version=1.0.1&metric=version&style=for-the-badge&label=programmable-secrets)](https://hol.org/registry/api/v1/skills/programmable-secrets%401.0.1/SKILL.md)

Standalone HCS-26 skill package for operating the Programmable Secrets protocol across Robinhood Chain Testnet and Arbitrum Sepolia.

Live app:
- [ps.hol.org](https://ps.hol.org)

This repo exists so the skill package has its own canonical GitHub source, commit history, and publish workflow. That gives the published artifact a stable `repo` and `commit` stamp during HCS-26 release packaging.

## What This Skill Covers

The skill is designed for agents and operators that need to:
- register encrypted datasets
- create timebound and UAID-gated policies
- purchase policies and verify receipts
- inspect dataset and policy state onchain
- run the guided Robinhood and Arbitrum operator workflows

Core implementation repo:
- [hashgraph-online/programmable-secrets-contracts](https://github.com/hashgraph-online/programmable-secrets-contracts)

Published skill package files:
- [Published SKILL.md](https://hol.org/registry/api/v1/skills/programmable-secrets%401.0.1/SKILL.md)
- [Published skill.json](https://hol.org/registry/api/v1/skills/programmable-secrets%401.0.1/manifest)
- [Repo SKILL.md](./SKILL.md)
- [Repo skill.json](./skill.json)
- [schema](./schemas/skill.schema.json)

## Primary CLI

The underlying operator CLI lives in the contracts repo and is exposed through GitHub `npx` execution:

```bash
npx github:hashgraph-online/programmable-secrets-contracts help
```

Common commands:

```bash
npx github:hashgraph-online/programmable-secrets-contracts start
npx github:hashgraph-online/programmable-secrets-contracts doctor
npx github:hashgraph-online/programmable-secrets-contracts contracts
npx github:hashgraph-online/programmable-secrets-contracts datasets list
npx github:hashgraph-online/programmable-secrets-contracts policies list
npx github:hashgraph-online/programmable-secrets-contracts flow:direct
```

## Networks

Default network:
- Robinhood Chain Testnet

Override network:

```bash
PROGRAMMABLE_SECRETS_NETWORK=arbitrum-sepolia npx github:hashgraph-online/programmable-secrets-contracts flow:direct
```

## Publish Workflow

This repo includes its own GitHub Actions publish workflow:
- [Skills Publish](./.github/workflows/skills-publish.yml)

It validates the package, stamps the source repo + commit into the publish payload, and pushes the release through the Registry Broker HCS-26 path.

## Why A Separate Repo

Keeping the skill package in its own repository gives the skill a clean provenance boundary:
- one repo for the package source
- one commit stream for versioned skill changes
- one publish workflow scoped to the package itself
- one canonical URL for downstream verification

That is the right shape for external publishing and independent integrity checks.

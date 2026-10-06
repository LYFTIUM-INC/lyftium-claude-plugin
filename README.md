# LYFTIUM — Claude Code plugin

Call LYFTIUM Ethereum Mainnet JSON-RPC with `X-Api-Key`; rpm plans; fails closed with 503 when tip gate is closed.

Product: [lyftium.com](https://www.lyftium.com) · Status: [app.lyftium.com/status](https://app.lyftium.com/status) · Docs: [app.lyftium.com/docs](https://app.lyftium.com/docs)

## What this plugin does

- Skill `/lyftium:lyftium-rpc` — wire ethers v6, viem, Foundry notes, and curl to `https://eth-mainnet-rpc.lyftium.com`
- Prompts for your API key via Claude Code `userConfig` (sensitive; never shipped in the repo)

## Install (local / one session)

```bash
git clone https://github.com/LYFTIUM-INC/lyftium-claude-plugin.git
claude plugin validate ./lyftium-claude-plugin
claude --plugin-dir ./lyftium-claude-plugin
```

In the session, run `/lyftium:lyftium-rpc` or ask Claude to configure LYFTIUM RPC. Configure the key with `/plugin configure lyftium` (or your Claude Code version’s plugin config UI).

## Install from this repo as a marketplace

```bash
claude plugin marketplace add LYFTIUM-INC/lyftium-claude-plugin
claude plugin install lyftium@lyftium
```

## Quick RPC check

```bash
curl -sS https://eth-mainnet-rpc.lyftium.com \
  -H "Content-Type: application/json" \
  -H "X-Api-Key: $LYFTIUM_API_KEY" \
  -d '{"jsonrpc":"2.0","id":1,"method":"eth_blockNumber","params":[]}'
```

HTTP **503** means the tip/sync gate is closed — check [Status](https://app.lyftium.com/status). Do not treat that as a successful read.

## Pricing (Mainnet only)

| Plan | Price | Limit |
|------|-------|-------|
| Standard | $29/mo | 100 rpm |
| Premium | $99/mo | 500 rpm |
| Enterprise | $499/mo | custom rpm, invoiced |

No free tier. Checkout on Polar via [lyftium.com](https://www.lyftium.com).

## Directory submission

This repo is intended for Anthropic’s community / directory listing (`claude-plugins-community` path). After validate passes:

1. Confirm a paid claude.ai plan can submit.
2. Submit at [platform.claude.com/plugins/submit](https://platform.claude.com/plugins/submit) or the developer portal directory flow documented by Anthropic.
3. Point the submission at this public GitHub repository.

## Out of scope

No tip unmasking, no MEV tooling, no multi-chain claims, no secrets in the plugin.

## License

MIT

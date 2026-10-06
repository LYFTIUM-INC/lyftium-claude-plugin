---
name: lyftium-rpc
description: Call LYFTIUM Ethereum Mainnet JSON-RPC with X-Api-Key; rpm plans; fails closed with 503 when tip gate is closed.
---

# Wire LYFTIUM Ethereum Mainnet JSON-RPC

Use this skill when the user wants to point ethers, viem, Foundry, curl, or another Ethereum Mainnet client at LYFTIUM.

## Facts (claim-safe — do not invent more)

- Endpoint: `https://eth-mainnet-rpc.lyftium.com`
- Auth: `X-Api-Key` header only. Never put the key in a query string or path.
- Chain: Ethereum Mainnet only. Do not claim L2, testnet, or multi-chain support.
- Pricing: requests per minute (rpm). Standard $29/mo for 100 rpm; Premium $99/mo for 500 rpm; Enterprise $499/mo with custom rpm, invoiced. There is no free tier.
- Fail-closed: when the tip/sync gate is closed, the endpoint returns HTTP **503** instead of a stale tip.
- Status SoT: https://app.lyftium.com/status
- Product / checkout: https://www.lyftium.com · https://app.lyftium.com
- Docs: https://app.lyftium.com/docs

If the plugin `userConfig` api_key is set, prefer `${user_config.api_key}` / the configured key. Otherwise ask the user for their key and store it only in their env or Claude Code plugin config — never commit it.

## curl

```bash
curl -sS https://eth-mainnet-rpc.lyftium.com \
  -H "Content-Type: application/json" \
  -H "X-Api-Key: $LYFTIUM_API_KEY" \
  -d '{"jsonrpc":"2.0","id":1,"method":"eth_blockNumber","params":[]}'
```

On HTTP 503: tell the user the tip gate is closed; check https://app.lyftium.com/status; retry or fail over. Do not treat 503 as success or invent a block number.

## ethers v6

```js
import { FetchRequest, JsonRpcProvider } from "ethers";

const req = new FetchRequest("https://eth-mainnet-rpc.lyftium.com");
req.setHeader("X-Api-Key", process.env.LYFTIUM_API_KEY);
const provider = new JsonRpcProvider(req, 1);
```

## viem

```ts
import { createPublicClient, http } from "viem";
import { mainnet } from "viem/chains";

const client = createPublicClient({
  chain: mainnet,
  transport: http("https://eth-mainnet-rpc.lyftium.com", {
    fetchOptions: {
      headers: { "X-Api-Key": process.env.LYFTIUM_API_KEY! },
    },
  }),
});
```

## Foundry

Foundry's default RPC URL form does not send custom headers. Prefer:

1. Export `ETH_RPC_URL=https://eth-mainnet-rpc.lyftium.com` only for tools that can inject headers separately, or
2. Use `cast`/`forge` with a wrapper that adds `X-Api-Key`, or call via curl/ethers/viem when header auth is required.

Do not suggest putting the API key in the URL.

## What not to do

- Do not claim free tier, SLA percentages, tip always ready, SOC 2, or competitor pricing.
- Do not invent multi-chain or testnet endpoints.
- Do not store or print the user's API key in repo files, logs, or commit messages.

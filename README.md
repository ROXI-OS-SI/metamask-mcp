# 🦊 metamask-mcp

[![npm](https://img.shields.io/npm/v/@roxi-os/metamask-mcp?label=npm&color=c5f24a)](https://www.npmjs.com/package/@roxi-os/metamask-mcp)
[![license](https://img.shields.io/badge/license-MIT-blue)](./LICENSE)
[![docs](https://img.shields.io/badge/docs-roxi--os.si-black)](https://roxi-os.si/docs/servers/metamask-mcp)

**The signing module of [ROXI OS](https://roxi-os.si)**, the Reasoning-Oriented Execution Interface.

`metamask-mcp` connects an AI agent to **your MetaMask wallet**. The agent can read chain data and prepare transactions, but anything that moves funds or changes state is sent to MetaMask as a prompt. **You approve it, or nothing happens.** Your private key never leaves the wallet.

---

## ✨ Features

- **Wallet-only signing:** transactions, deployments and signed messages always go through a MetaMask prompt.
- **No custody:** the server never sees, stores or asks for a private key or seed phrase.
- **QR connect:** the agent shows a QR code; scan it with MetaMask Mobile to connect.
- **28 tools:** balances, tokens, blocks, transactions, ENS, gas, contract reads and writes, deployments.
- **Model-agnostic:** works with Claude, GPT, or any client that supports MCP servers over stdio.

**Networks:** Ethereum Mainnet (`1`) and Sepolia testnet (`11155111`).

---

## 🚀 Quick start

Requires **Node.js 20+** and an MCP client such as Claude Desktop.

Add the server to your client's config (`claude_desktop_config.json`):

```json
{
  "mcpServers": {
    "metamask": {
      "command": "npx",
      "args": ["-y", "@roxi-os/metamask-mcp"]
    }
  }
}
```

Restart the client, then try:

```
Connect my MetaMask wallet.
Switch to Sepolia and show my ETH balance.
Send 0.01 ETH to 0x… on Sepolia.
```

Full guide: [Connect a wallet](https://roxi-os.si/docs/guides/connect-wallet).

---

## 🧱 How it works

```
Agent → metamask-mcp → MetaMask prompt → You sign → Broadcast
```

1. `get-connect-uri` and `show-connect-qrcode` connect your wallet through MetaMask Mobile.
2. Read-only tools (balances, blocks, contract views) run without asking you anything.
3. Signing tools open a MetaMask prompt with the exact transaction. Check the network, recipient and amount, then confirm or reject.

`metamask-mcp` also sends the transactions other ROXI servers prepare: [`erc20-mcp`](https://github.com/ROXI-OS-SI/erc20-mcp) returns encoded token calls, and [`solc-mcp`](https://github.com/ROXI-OS-SI/solc-mcp) returns compiled bytecode.

---

## ⚙️ Tools

### Signing (opens a MetaMask prompt)

| Tool | Description |
| --- | --- |
| `send-transaction` | Send a transaction (`to`, optional `data`, `value`, gas fees, `chainId`). |
| `write-contract` | Call a state-changing contract function by name (`abi`, `address`, `functionName`, `args`). |
| `deploy-contract` | Deploy a contract from `abi`, `bytecode` and constructor `args`. |
| `sign-message` | Sign a plain-text message. |

### Connection

| Tool | Description |
| --- | --- |
| `get-connect-uri` | Start a MetaMask connection and return its connect URI. |
| `show-connect-qrcode` | Show the connect URI as a QR code to scan with MetaMask Mobile. |
| `get-account` | Get the connected address (empty if no wallet is connected). |
| `disconnect` | Disconnect the wallet. |

### Network and gas

| Tool | Description |
| --- | --- |
| `get-chain-id` | Get the current chain ID. |
| `get-chains` | List the configured chains. |
| `get-chain-list` | Get every chain listed on Chainlist. |
| `switch-chain` | Switch the wallet's chain (MetaMask asks you to approve). |
| `get-block-number` | Get the latest block number. |
| `get-block` | Get a block by number, hash or tag. |
| `estimate-gas` | Estimate the gas a transaction needs. |
| `estimate-fee-per-gas` | Estimate fees per gas for the next block. |
| `wait-seconds` | Pause for a number of seconds. |

### Reading

| Tool | Description |
| --- | --- |
| `get-native-currency-balance` | Get an address's ETH (native currency) balance. |
| `get-token-balance` | Get an address's ERC-20 balance. |
| `get-token` | Get a token's name, symbol, decimals and total supply. |
| `get-transaction` | Get a transaction by hash. |
| `get-transaction-receipt` | Get a transaction receipt by hash. |
| `wait-for-transaction-receipt` | Wait for a transaction to be mined, then return its receipt. |
| `get-ens-address` | Resolve an ENS name to an address. |
| `get-ens-name` | Get the primary ENS name for an address. |
| `call` | Run a contract call without sending a transaction. |
| `read-contract` | Call a read-only contract function by name. |
| `verify-message` | Check that a message was signed by an address. |

**Prompt:** `be-metamask-assistant` gives the agent step-by-step routines for connecting, sending transactions, calling contracts and deploying them.

Parameters for every tool: [metamask-mcp reference](https://roxi-os.si/docs/servers/metamask-mcp).

---

## 🔐 Security

- Every state change needs your approval in MetaMask. Rejecting is always safe.
- Before approving, check the **network**, the **recipient or contract**, and the **amount**.
- The wallet session lives in memory and ends when the server stops.
- Try things on **Sepolia** first.

More: [Security model](https://roxi-os.si/docs/security).

---

## 🛠️ Build from source

```bash
git clone https://github.com/ROXI-OS-SI/metamask-mcp.git
cd metamask-mcp
pnpm install
pnpm build
```

Then point your client at the build:

```json
{
  "mcpServers": {
    "metamask": {
      "command": "node",
      "args": ["/absolute/path/to/metamask-mcp/dist/index.js"]
    }
  }
}
```

---

## 🧩 Part of ROXI OS

| Server | Purpose |
| --- | --- |
| **metamask-mcp** | Wallet connection and signing |
| [chainlist-mcp](https://github.com/ROXI-OS-SI/chainlist-mcp) | Chain metadata and RPC endpoints |
| [erc20-mcp](https://github.com/ROXI-OS-SI/erc20-mcp) | ERC-20 transfers, approvals and reads |
| [solc-mcp](https://github.com/ROXI-OS-SI/solc-mcp) | Solidity compilation |

📖 [Documentation](https://roxi-os.si/docs) · 📦 [npm](https://www.npmjs.com/org/roxi-os)

## 🙏 Credits

Built on the original [MetaMask MCP prototype](https://github.com/xiawpohr/metamask-mcp) by xiawpohr, extended for ROXI OS.

## ⚖️ License

MIT

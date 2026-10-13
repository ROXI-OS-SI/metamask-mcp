# 🦊 metamask-mcp

`metamask-mcp` is the signing module of **[ROXI OS](https://github.com/Roxi-OS)**, the Reasoning-Oriented Execution Interface for onchain agents.

It lets an LLM agent prepare onchain actions and hand them to **your MetaMask wallet** for approval. The agent proposes; you sign. Private keys never leave the wallet.

📖 Docs: [roxi-ten.vercel.app/docs/servers/metamask-mcp](https://roxi-ten.vercel.app/docs/servers/metamask-mcp)

---

## ✨ Features

- **Wallet-only signing** — every transaction and message is signed in MetaMask, never by the agent.
- **No custody** — the server never sees or stores a private key.
- **Connect by QR** — the agent shows a MetaMask connect QR code or URI; scan it with MetaMask Mobile.
- **Model-agnostic** — works with Claude, GPT, or any client that speaks the Model Context Protocol (MCP).
- **Composable** — pairs with [`chainlist-mcp`](https://github.com/Roxi-OS/chainlist-mcp-main), [`erc20-mcp`](https://github.com/Roxi-OS/erc20-mcp-main) and [`solc-mcp`](https://github.com/Roxi-OS/solc-mcp).

## 🧱 How it fits

```
Agent → metamask-mcp → MetaMask prompt → You sign → Broadcast
```

---

## 🚀 Quick start

Requires **Node.js 20+**.

Add it to your MCP client (for example Claude Desktop's `claude_desktop_config.json`):

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

### From source

```bash
git clone https://github.com/Roxi-OS/metamask-mcp.git
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

## ⚙️ Tools

| Tool | Description |
| --- | --- |
| `get-connect-uri` | Get the connect URI to connect to a MetaMask wallet. |
| `show-connect-qrcode` | Show the connect QR code for a given connect URI. |
| `disconnect` | Disconnect the wallet. |
| `get-account` | Get the current account. |
| `get-chain-id` | Get the current chain id. |
| `get-chains` | Get the configured chains. |
| `get-chain-list` | Get a list of all chains information. |
| `switch-chain` | Switch the target chain. |
| `get-native-currency-balance` | Get the native currency balance of an address. |
| `get-token-balance` | Get token balance of an address. |
| `get-token` | Fetch the token information. |
| `get-block-number` | Fetch the number of the most recent block seen. |
| `get-block` | Fetch information about a block at a block number, hash or tag. |
| `get-transaction` | Fetch transaction given hash or block identifiers. |
| `get-transaction-receipt` | Fetch the transaction receipt given a transaction hash. |
| `wait-for-transaction-receipt` | Wait for a transaction to be included in a block, then return its receipt. |
| `get-ens-address` | Fetch the ENS address for a name. |
| `get-ens-name` | Fetch the primary ENS name for an address. |
| `estimate-gas` | Estimate the gas needed for a transaction without sending it. |
| `estimate-fee-per-gas` | Estimate fees per gas (in wei) for inclusion in the next block. |
| `call` | Execute a message call immediately without submitting a transaction. |
| `read-contract` | Call a read-only function on a contract. |
| `write-contract` | Execute a write function on a contract (signed in MetaMask). |
| `deploy-contract` | Deploy a contract from bytecode and constructor arguments (signed in MetaMask). |
| `send-transaction` | Send a transaction (signed in MetaMask). |
| `sign-message` | Sign a message (in MetaMask). |
| `verify-message` | Verify that a message was signed by the provided address. |
| `wait-seconds` | Wait the given number of seconds. |

It also provides one prompt, `be-metamask-assistant`.

---

## 🙏 Credits

Built on the original [MetaMask MCP prototype](https://github.com/xiawpohr/metamask-mcp) by xiawpohr, extended for ROXI OS.

## ⚖️ License

MIT

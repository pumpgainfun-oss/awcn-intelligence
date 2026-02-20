# A2A Commerce + Auth Intelligence — Feb 20, 2026

## The Stack (How It Fits Together)

```
IDENTITY     → ERC-8004 (on-chain agent passport)
COMMS        → A2A Protocol v0.2.5 (Google, agent cards)
AUTH         → A2A Bearer/OAuth/JWT (short-lived tokens)
PAYMENT      → x402 (HTTP 402, USDC on Base)
DISCOVERY    → x402 Bazaar (Coinbase) + ERC-8004 registry
```

---

## 1. A2A Protocol (Google) — Comms Layer
- **Spec:** https://a2a-protocol.org/latest/specification/
- **GitHub:** https://github.com/a2aproject/A2A
- **Version:** v0.2.5 (latest)
- **Transport:** HTTP / SSE / JSON-RPC
- **Agent Card:** `/.well-known/agent-card.json` — self-describing manifest
- **Auth schemes:** Bearer (OAuth 2.0), Basic, API Key (OpenAPI-compatible)
- **Key insight:** Short-lived tokens (minutes) — no hardcoded secrets. Dynamic machine identity.
- **Enterprise-ready:** security, auth, observability baked in

## 2. AP2 — Agent Payments Protocol (Google + Coinbase)
- **URL:** https://ap2-protocol.org/
- **Announced:** Sep 16, 2025 (Google Cloud Blog)
- Built on A2A as an extension
- Partners: Coinbase, Ethereum Foundation, MetaMask
- Launched **A2A x402 extension** — production-ready agent crypto payments
- The official bridge between A2A comms and x402 payments

## 3. x402 — Payment Protocol
- **URL:** https://x402.org / https://docs.cdp.coinbase.com/x402/welcome
- **Mechanism:** HTTP 402 status code triggers payment flow
- **Asset:** USDC on Base (blockchain-agnostic per spec)
- **Volume:** $26.19M total processed as of Feb 2026 (4 days ago)
- **Adopters:** Cloudflare (Dec 2025), Stripe (preview, 1 week ago!), Firecrawl, Pinata
- **Stripe x402:** Developers can now bill AI agents in USDC on Base directly via Stripe
- **MCP integration:** MCP servers can expose paid tools via x402
- **Discovery:** Coinbase x402 Bazaar = find/sell services to agents
- **Whitepaper:** https://www.x402.org/x402-whitepaper.pdf

## 4. ERC-8004 — Agent Identity (Ethereum)
- **EIP:** https://eips.ethereum.org/EIPS/eip-8004
- **What:** On-chain NFT registry for AI agent identity + reputation + validation
- **Registration file** (JSON) includes:
  - name, description, image
  - services: A2A endpoint, MCP endpoint, ENS/DID, web
  - payment proof formats (x402 compatible)
  - feedback/reputation schemas
- **v2 spec:** Deeper MCP support, x402 payment proofs, agent marketplaces
- **Trust model:** Verifiable on-chain identity for trustless cross-org agent commerce

### ERC-8004 Registration File Example:
```json
{
  "type": "erc-8004-registration-v1",
  "name": "AWCN",
  "description": "Agent Workforce Control Node — AI agent services",
  "services": [
    {"name": "A2A", "endpoint": "https://awcn.icmmotion.com/.well-known/agent-card.json"},
    {"name": "MCP", "endpoint": "https://mcp.awcn.icmmotion.com/"},
    {"name": "web", "endpoint": "https://awcn.icmmotion.com/"}
  ]
}
```

## 5. The Agentic Payments Map (Fintech Brainfood)
- **Protocols are NOT competing** — they solve different layers:
  - Commerce protocols (agent buys from merchant) → x402
  - B2B protocols (agent pays invoices) → AP2/A2A
  - Agent-to-agent (agent pays agent for compute/data) → x402 + ERC-8004
- Alphabet soup: ACP, UCP, A2P, AXTP, x402 — all coexist

---

## AWCN Action Plan

### Immediate (this week):
1. **Create A2A agent card** at `/.well-known/agent-card.json` on icmmotion.com
   - List AWCN services, auth schemes, MCP endpoints
2. **Register on ERC-8004** — mint AWCN's on-chain agent identity
   - Establishes trust + discoverability in the agent economy

### Near-term (month 1):
3. **Implement x402 receiving** — let agents pay AWCN for services in USDC
   - List on x402 Bazaar (Coinbase) for discovery
4. **Wire Stripe x402** — sell agent services via Stripe's new x402 integration

### Strategic:
5. **AWCN as an A2A server** — agents from other platforms can discover + hire AWCN agents
6. **ERC-8004 reputation** — build verifiable track record for trust in agent commerce

---

## Key Signal: Stripe Added x402 (1 week ago)
This is the **enterprise legitimization moment** for x402. Stripe doing x402 = banks/enterprises will follow. Our x402 integration becomes a business requirement, not just a crypto experiment.

## Key Signal: $26.19M Volume in <8 months
x402 went from 0 to $26M in one year. Early movers are building the equivalent of Stripe circa 2011.

---

*Sources: Google Cloud Blog, a2a-protocol.org, ap2-protocol.org, x402.org, Coinbase CDP docs, Cloudflare blog, eips.ethereum.org, fintechbrainfood.com, insights.blockonomics.co*
*Researched: 2026-02-20 21:17 WITA*

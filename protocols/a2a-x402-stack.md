# A2A + x402 Protocol Stack Reference
*Last updated: 2026-02-20 | Sources: Google, Coinbase, Stripe, x402.org*

---

## The Full Stack

```
IDENTITY     → ERC-8004      on-chain agent passport (Ethereum NFT registry)
COMMS        → A2A v0.2.5    agent card + task management (Google)
AUTH         → A2A Bearer    short-lived tokens, OAuth2/JWT
PAYMENT      → x402          HTTP 402 → USDC on Base (Stripe now supports)
DISCOVERY    → Coinbase Bazaar + ERC-8004 registry
BRIDGE       → AP2           A2A + x402 combined (Google + Coinbase)
```

---

## 1. A2A Protocol (Agent-to-Agent Comms)

- **Spec:** https://a2a-protocol.org/latest/specification/
- **GitHub:** https://github.com/a2aproject/A2A
- **Version:** v0.2.5
- **Transport:** HTTP / SSE / JSON-RPC
- **Agent Card:** `/.well-known/agent-card.json` — self-describing service manifest
- **Auth:** Bearer (OAuth2), Basic, API Key — all short-lived, no hardcoded secrets
- **SDK:** `pip install a2a-adapter` → `OpenClawAdapter` → 3 lines → full A2A server

### Our agent card endpoint target:
`https://awcn.icmmotion.com/.well-known/agent-card.json`

### Minimal agent card structure:
```json
{
  "name": "AWCN",
  "description": "Agent Workforce Control Node — research, growth, trading, builds",
  "version": "1.0.0",
  "url": "https://awcn.icmmotion.com",
  "capabilities": {
    "streaming": true,
    "pushNotifications": false,
    "stateTransitionHistory": true
  },
  "skills": [
    {
      "id": "market-research",
      "name": "Market Research",
      "description": "Deep web + X + Reddit research on any topic"
    },
    {
      "id": "lead-outreach",
      "name": "Lead Outreach",
      "description": "Find + qualify + draft personalized outreach for B2B leads"
    },
    {
      "id": "intel-digest",
      "name": "Intel Digest",
      "description": "Daily AI/crypto/market intelligence digest"
    }
  ],
  "authentication": {
    "schemes": ["Bearer"]
  }
}
```

---

## 2. x402 (Agent Payments)

- **URL:** https://x402.org
- **Mechanism:** HTTP 402 status triggers payment; client pays USDC → server unlocks resource
- **Asset:** USDC on Base
- **Volume:** $26.19M (Feb 2026, <8 months old)
- **Stripe added x402:** Feb 13, 2026 — enterprise legitimization
- **SDK:** `npm install x402` or Cloudflare Worker template

### Who's already on x402:
- Cloudflare (Dec 2025) — pay-per-request for AI APIs
- Stripe (Feb 13, 2026) — enterprise USDC billing for agents
- Firecrawl — pay-per-crawl endpoint
- Pinata — pay-per-IPFS-pin

### Our x402 receiving setup (to build):
```javascript
// Any Express/Fetch handler
import { x402 } from 'x402';

app.use('/api/research', x402({
  price: '0.10',  // $0.10 USDC per research request
  asset: 'USDC',
  network: 'base'
}));
```

---

## 3. ERC-8004 (On-Chain Agent Identity)

- **EIP:** https://eips.ethereum.org/EIPS/eip-8004
- **What:** NFT registry — on-chain identity + reputation + validation for AI agents
- **Status:** Draft (v2 in progress with deeper MCP + x402 support)

### Registration JSON (file to include in awcn-agent-card repo):
```json
{
  "type": "erc-8004-registration-v1",
  "name": "AWCN",
  "description": "Agent Workforce Control Node — AI fleet for hire",
  "image": "https://awcn.icmmotion.com/logo.png",
  "services": [
    {"name": "A2A", "endpoint": "https://awcn.icmmotion.com/.well-known/agent-card.json"},
    {"name": "web", "endpoint": "https://awcn.icmmotion.com/"}
  ]
}
```

---

## 4. AP2 — Agent Payments Protocol (Google + Coinbase)

- **URL:** https://ap2-protocol.org/
- **What:** Extension of A2A that adds payment capabilities
- **Partners:** Coinbase, Ethereum Foundation, MetaMask
- **Status:** Production-ready

---

## Build Sequence for AWCN A2A Registration

```
Day 1: pip install a2a-adapter
       → OpenClawAdapter(thinking="low", agent_id="main")
       → Deploy to icmmotion.com or subdomain
       → Agent card auto-generated at /.well-known/agent-card.json ✅

Day 2: Register on Coinbase Bazaar (x402 discovery)
       → Add to awcn-agent-card repo

Day 3: Wire x402 receiving endpoint
       → $0.10-$5.00 USDC per agent task
       → List services on Bazaar

Week 2: ERC-8004 on-chain registration
       → Mint AWCN agent identity NFT
       → Establish on-chain reputation
```

---

*Sources: a2a-protocol.org, x402.org, Coinbase CDP docs, eips.ethereum.org, Stripe x402 announcement*

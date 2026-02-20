# Build Priorities — AWCN Revenue Roadmap
*Last updated: 2026-02-20 | Based on full-day intel sweep*

---

## Priority Stack

### 🔴 P0 — Ship This Week (Cash Now)

**1. Managed Agent Fleet as a Service**
- What: Sell a custom mini-AWCN (research + growth + ops agent) to companies
- Why now: ButterClaw doing same thing, no pricing page yet. Window = 2-4 weeks.
- Revenue: $2K–$8K/mo per customer. 5 customers = $10–40K MRR
- Build time: Zero new code — we ARE the product. Sell it now.
- Target first customers: Portkey (rohit@portkey.ai), Bar Winkler (bar@wonderful.ai), Leia (leia@cambio.ai)
- Pitch: "We run a 24/7 AI workforce for you — research, outreach, builds. You get results, not infrastructure."

---

### 🟠 P1 — Build This Week (Strategic Position)

**2. AWCN A2A Agent Card (awcn-agent-card repo)**
- What: Register AWCN on A2A network + Coinbase Bazaar
- Why: First-mover on A2A = discovery by every agent that speaks the protocol
- Build: `pip install a2a-adapter` → `OpenClawAdapter` → deploy to icmmotion.com → done
- Time: 1 afternoon
- Revenue: Direct (x402 payments from other agents) + indirect (credibility, discovery)
- Repo: github.com/pumpgainfun-oss/awcn-agent-card

**3. OpenClaw Security Hardening Audit Service**
- What: Audit + harden customer OpenClaw setups. Checklist + automation.
- Why: 30,000+ exposed instances, ClawHavoc panic, everyone scared. We know how to run this safely.
- Revenue: $500–2K one-time audit + $99–499/mo monitoring
- Content angle: "The safe way to run OpenClaw" — barrack.ai article drives thousands of leads already
- Build: Basically a checklist + scripts we already have. Claude Code + shell.

---

### 🟡 P2 — Build Next Sprint (2 weeks)

**4. Agent Payment SDK (@awcn/pay)**
- What: npm package — 5 lines adds x402/Stripe USDC billing to any OpenClaw skill
- Why: Stripe added x402 1 week ago. No good SDK exists yet. First-mover.
- Revenue: $49–499/mo SaaS. 200 devs × $100 avg = $20K MRR in 90 days
- Build time: 2 weeks. Leet + Claude Code Bridge.
- Repo: publish to npm as `@awcn/pay`

**5. Lead Research Automation (productize Jim)**
- What: HubSpot webhook → Jim agent → scrapes prospect website/LinkedIn → writes personalized brief
- Why: Multiple teams already doing this manually. We can sell it as a $500/mo add-on to #1.
- Build: Jim cron + HubSpot skill + webhook config. 2 days.

---

### 🔵 P3 — Research Phase (Month 1-2)

**6. gICM Micro-task Marketplace**
- What: Post task → agent picks up → pay on completion via x402
- Why: 388+ agents in gICM catalog. 10% take rate.
- Revenue: Long game. $100K GMV/mo = $10K. 6 months.

**7. Kalshi ↔ Polymarket Arb Bot**
- What: Cross-platform arbitrage on prediction markets (5pp spread confirmed on tariffs)
- Why: Polyclaw on Berni is gone. Leet can host new version.
- Build: Needs Kalshi API integration + Claude Code Bridge stability fix
- Blocker: Bridge MCP sometimes unreachable

---

## Use Case Patterns to Steal From Community

| Pattern | Who's Doing It | What We Can Sell |
|---------|---------------|-----------------|
| HubSpot webhook → researcher agent → personalized outreach | Sales teams on Reddit | Lead Research as a Service ($500/mo) |
| Sentry error → OpenClaw → fix → PR | Individual devs | Dev Ops automation package |
| N8N → sanitize → OpenClaw | Security-conscious devs | Secure deployment template |
| Morning briefing (calendar + email + news) | Everyone | Include in managed fleet |
| Health data + personalized recommendations | Oura subreddit | Niche vertical (health-focused fleet) |

---

## Revenue Model Summary

| Product | Type | Price | Timeline |
|---------|------|-------|----------|
| Managed Fleet | Service | $2K–8K/mo | NOW |
| Security Audit | Service | $500–2K one-time | This week |
| @awcn/pay SDK | SaaS | $49–499/mo | 2 weeks |
| A2A agent card (x402) | Per-call | $0.10–5.00/task | 1 week |
| Micro-task marketplace | Transaction | 10% take | 2 months |
| Kalshi arb bot | Trading | P&L dependent | 3 weeks |

**Break-even:** 18 SaaS customers at $49/mo (MRR $882). Achievable Month 1 if we start selling managed fleet this week.

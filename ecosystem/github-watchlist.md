# GitHub Watchlist — AWCN Fleet
*Last updated: 2026-02-20 | Auto-monitored by openclaw-repo-watcher cron*

---

## 🏠 OUR REPOS (@pumpgainfun-oss)

| Repo | Purpose | Status |
|------|---------|--------|
| [awcn-skills](https://github.com/pumpgainfun-oss/awcn-skills) | Skill packages + prompts for AWCN fleet | 🟡 Empty — needs content |
| [awcn-agent-card](https://github.com/pumpgainfun-oss/awcn-agent-card) | A2A identity + `.well-known/agent-card.json` | 🟡 Empty — priority build |
| [awcn-intelligence](https://github.com/pumpgainfun-oss/awcn-intelligence) | Intel pipeline outputs, trend digests | 🟡 Empty — wire cron outputs |

---

## 🔌 ADAPTERS & PROTOCOLS (Use These Now)

| Repo | What It Does | Priority |
|------|-------------|----------|
| [hybroai/a2a-adapter](https://github.com/hybroai/a2a-adapter) | 3-line Python SDK: any agent → A2A server. Has `OpenClawAdapter` built-in. Auto-generates AgentCard at `/.well-known/agent.json`. Supports n8n, LangChain, LangGraph, CrewAI, OpenClaw | 🔴 USE NOW — for awcn-agent-card |
| [a2aproject/A2A](https://github.com/a2aproject/A2A) | Official A2A Protocol spec (Google, v0.2.5). Source of truth for agent card format, auth schemes, task management | 🟠 Reference |
| [jovanSAPFIONEER/Network-AI](https://github.com/jovanSAPFIONEER/Network-AI) | Multi-agent swarm orchestration skill for OpenClaw. File-system mutexes, atomic commits, token budget ceilings, agent-to-agent handoffs via `sessions_send`. Supports LangChain, CrewAI, AutoGen | 🟠 Evaluate for fleet coordination |

---

## 🦞 OPENCLAW CORE

| Repo | What It Does | Why Watch |
|------|-------------|-----------|
| [openclaw/openclaw](https://github.com/openclaw/openclaw) | Main OpenClaw repo (180k+ ★) | API changes, new RPC methods, config keys ship every few days |
| [coollabsio/openclaw](https://github.com/coollabsio/openclaw) | Production-hardened Docker images with persistent storage + browser sidecar (CDP+VNC) + webhook hooks. `OPENCLAW_GATEWAY_TOKEN` support | Reference for containerized deployments |

---

## 📚 SKILL ECOSYSTEM

| Repo | What It Does | Stars | Notes |
|------|-------------|-------|-------|
| [VoltAgent/awesome-openclaw-skills](https://github.com/VoltAgent/awesome-openclaw-skills) | 3,002 curated + vetted skills (filtered from 5,700+ on ClawHub). Includes Vercel, Cloudflare, Supabase, Stripe, Google, HuggingFace official skills | 7,800★ | Primary source for safe skill discovery |
| [VoltAgent/awesome-agent-skills](https://github.com/VoltAgent/awesome-agent-skills) | 300+ cross-platform skills (OpenClaw + Cursor + GitHub Copilot + Codex + Windsurf). All use `.claude/skills/` format = OpenClaw compatible | High | Cross-platform, growing fast |
| [hesreallyhim/awesome-claude-code](https://github.com/hesreallyhim/awesome-claude-code) | Curated: slash commands, hooks, agents, CLAUDE.md patterns. Best source for context optimization techniques | Medium | Token reduction patterns |

---

## 🔍 MONITORING SCHEDULE

The `openclaw-repo-watcher` cron runs 3x/day (07:00, 14:00, 22:00 WITA) and checks:
- New releases on `openclaw/openclaw`
- New commits to `hybroai/a2a-adapter`
- New skills on `VoltAgent/awesome-openclaw-skills`

Output: `~/.openclaw/awcn/knowledge/collections/github/`
Alerts: Discord #build-log + Telegram if breaking changes detected

---

## 📋 ACTION QUEUE

- [ ] **awcn-agent-card**: Use `a2a-adapter` + `OpenClawAdapter` to publish AWCN on A2A network (ETA: 1 afternoon)
- [ ] **awcn-skills**: Push our best fleet skills (dao-quality-review, scout-intel-gathering, opportunity-hunter prompts)
- [ ] **awcn-intelligence**: Wire intel cron outputs to auto-commit daily digests
- [ ] **Network-AI skill**: Evaluate for fleet swarm coordination — replaces our manual `sessions_send` orchestration

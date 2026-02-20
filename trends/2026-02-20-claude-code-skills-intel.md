# Intel: Claude Code Skills & Context Indexing
**Captured:** Friday, February 20, 2026 — 16:17 WITA  
**Purpose:** Cross-reference what Claude Code has built vs OpenClaw's skill system. Extract improvements for token reduction + faster memory loading.

---

## 🔑 TL;DR FOR BASIL

Claude Code's skill ecosystem exploded (300+ skills, major company support). They're solving the SAME context/token problem OpenClaw has. **The hot fix is lazy-loading** — and OpenClaw already does it, but incompletely. We can steal 3-4 specific techniques for a 50-70% token reduction in our fleet sessions.

---

## 1. The Ecosystem (GitHub)

### Official Claude Code Skills (Anthropic)
- **Path:** `.claude/skills/` (project) or `~/.claude/skills/` (global)
- **Format:** Folder + `SKILL.md` — **same pattern as OpenClaw**
- Official skills from Anthropic: `docx`, `doc-coauthoring`, `pptx`, `xlsx`, `pdf`, `algorithmic-art`, `canvas-design`

### Skills Are Now Cross-Platform (Big Deal)
Claude Code skills are now compatible with: **Codex, Gemini CLI, Cursor, GitHub Copilot, OpenCode, Windsurf, Antigravity**
- VoltAgent/awesome-agent-skills: 300+ skills, maintained with Vercel, Cloudflare, Supabase, Stripe, Google, Hugging Face, Trail of Bits, Sentry, Expo, Microsoft, HashiCorp, etc.
- Path standardized: `.claude/skills/` → same as OpenClaw SKILL.md architecture

### Key Repos to Watch
| Repo | Why It Matters |
|------|----------------|
| `hesreallyhim/awesome-claude-code` | Curated list: slash commands, hooks, agents, CLAUDE.md patterns |
| `VoltAgent/awesome-agent-skills` | 300+ official + community skills, cross-platform |
| `blader/Claudeception` | **Autonomous skill extraction** — Claude writes new skills from its own discoveries |
| `avifenesh/agentsys` | Workflow automation: PR management, multi-agent review, drift detection |
| `trailofbits/skills` | Security audit skills (relevant for our fleet security) |
| `OthmanAdi/planning-with-files` | Manus-style persistent markdown planning |
| `mvanhorn/last30days-skill` | Reddit + X research skill (30-day window) — relevant for SCOUT |

---

## 2. Context Indexing / Token Reduction (THE BIG ONE)

### Problem (Confirmed by Anthropic GitHub Issue #19105)
A fresh Claude Code "hi" message costs **~53k tokens** before any real work.

| Component | Tokens | Controllable? |
|-----------|--------|---------------|
| System tools | 20.4k (38%) | No |
| Memory files (CLAUDE.md) | 10-18k (19-34%) | ✅ Yes |
| MCP tools | 9.1k (17%) | Partially |
| Custom agents | 3.3k (6%) | ✅ Yes |
| Skills | 2.6k (5%) | ✅ Yes |

**~35k tokens are controllable but currently load upfront regardless of need.**

### Solution 1: Tiered Lazy Loading (54% reduction proven)
Source: `johnlindquist` gist — December 2025

**Before (monolithic):** Everything in CLAUDE.md loaded at session start  
**After (tiered):**
```
CLAUDE.md (minimal index)
├── identity.md (1,252 bytes) ← CONSOLIDATED
├── skills-rules.md (2,997 bytes) ← TRIGGER TABLE ONLY
└── ... hard rules only

Skills loaded on-demand via Skill("name")
├── skill1/SKILL.md ← Only when triggered
├── skill2/SKILL.md ← Only when triggered
└── ... 30+ skills dormant
```

**Results:**
| File | Before | After | Reduction |
|------|--------|-------|-----------|
| skills-rules.md | 10,204 bytes | 2,997 bytes | **70%** |
| identity + simulator | 6,843 bytes | 1,252 bytes | **82%** |
| logging-preferences | 4,887 bytes | 1,084 bytes | **78%** |
| Compressed skills (optional) | 244 KB | 17 KB | **93%** |
| **Total context tokens** | **7,584** | **3,434** | **54%** |

**Key insight:** Claude doesn't need verbose documentation upfront — it needs **triggers** to know when to load detailed context.

### Solution 2: CLAUDE.md Index Pattern
```yaml
# CLAUDE.md.index (~500 tokens instead of 15k)
sections:
  timestamps:
    triggers: ["timestamp", "time format"]
    file: CLAUDE.md#timestamps
  memory_system:
    triggers: ["RAG", "memory", "CTM"]
    file: RAG_GUIDE.md
  agents:
    triggers: ["agent", "delegate"]
    file: AGENTS_INDEX.md
```
Fetch matching sections on-demand based on user message keywords. **Saves ~15k tokens per session.**

### Solution 3: MCP Tool Search (Anthropic OFFICIAL — January 2026)
**Lazy loading for MCP tools is now NATIVE in Claude Code.**

- When MCP tool descriptions exceed **10% of context window** → auto-switches to search-based loading
- Claude fetches only the tool definitions it actually needs
- Config: `enable_tool_search: true` in MCP server config
- Saves up to **95% of MCP context** for large tool sets
- Source: VentureBeat, claudefa.st, tessl.io

### Solution 4: Trigger Table in Skills Rules
Instead of full skill documentation, just a lookup table:
```
| Triggers | Skill | Primary Tools |
|----------|-------|---------------|
| task, track, backlog | beads | beads_ready, beads_add |
| search, research, look up | gemini-research | gemini_research |
| deploy, ship, push | ship | git, ci |
```
Claude reads the table to decide which SKILL.md to load. Full protocol is in the skill file, only fetched on match.

### Solution 5: Sub-Agent Context Inheritance Control
Problem: Sub-agents inherit full parent context including entire CLAUDE.md (~8k tokens wasted per sub-agent)
```yaml
Task:
  subagent_type: "Explore"
  prompt: "Find TypeScript files"
  context_inheritance: "minimal"  # minimal | full | none
```
**Saves ~8k tokens per sub-agent spawned.** Massive for our fleet.

### Solution 6: Usage-Based Tool Pruning
Track which skills haven't been used in 30+ sessions → suggest disabling.
Saves 1-5k tokens per session in accumulated cruft.

---

## 3. OpenClaw's Current System (How We Stack Up)

### What OpenClaw ALREADY Does (Confirmed by Substack article)
Source: limitededitionjonathan.substack.com

> "When a session starts, OpenClaw reads only the name and description from every available skill. We're talking maybe 30-50 tokens per skill. The agent hasn't loaded any instructions yet. It just knows: 'I have a slack skill, I have a database skill.' **That's it. Tier 2 — The index. When the agent decides a skill is relevant, it reads the full SKILL.md file.**"

✅ **OpenClaw already has 2-tier lazy loading for skills.** This is why we only see `<description>` in `<available_skills>` at session start.

### What Claude Code Has That We Don't (Yet)
1. **MCP Tool Search** — Native lazy loading for MCP tool definitions (we could apply this to our MCP gateway calls)
2. **Trigger table routing** — Explicit keyword → skill mapping (reduces ambiguity in skill selection)
3. **Sub-agent context inheritance control** — Our fleet sub-agents get full context when spawned (expensive)
4. **CLAUDE.md index pattern** — Our MEMORY.md, AGENTS.md, SOUL.md load in full every session
5. **Usage-based pruning** — No mechanism to retire unused skills

### Context Optimizer Skill (Already on openclaw/skills)
`openclaw/skills/ad2546/context-optimizer/SKILL.md` — implements:
```javascript
createContextPruner({
  contextLimit: 64000,
  autoCompact: true,
  dynamicContext: true,
  strategies: ['semantic', 'temporal', 'extractive', 'adaptive'],
  queryAwareCompaction: true,
})
```
**We should install this.** It does semantic + temporal compaction.

---

## 4. Claudeception — Autonomous Skill Learning (HIGH VALUE)

**Repo:** `blader/Claudeception`  
**Concept:** When Claude Code discovers something non-obvious (debugging trick, workaround, project-specific pattern), it saves that knowledge as a new skill automatically. Next time, the skill loads from the trigger.

**How it works:**
1. Hook fires on every `UserPromptSubmit`
2. Evaluates if the session produced extractable knowledge
3. Only extracts knowledge that: required actual discovery, has clear trigger conditions, has been verified to work
4. Writes new SKILL.md automatically

**For AWCN:** This is exactly what we need. Fleet agents discovering patterns → automatically write skills → fleet gets smarter session over session.  
**Priority:** HIGH. Implement for Andy + Leet first.

---

## 5. Security Skills (Trail of Bits)

`trailofbits/skills` — security research, vulnerability detection, audit workflows.  
Relevant for: fleet security sweeps, API key audit, ClawHub skill injection detection.  
**Install via:** `gh repo clone trailofbits/skills .claude/skills/trailofbits`

---

## 6. X / Twitter Signal

**@WesRothMoney (x.com):**
> "Anthropic is rolling out MCP Tool Search for Claude Code, addressing one of the most requested GitHub features: lazy loading for MCP servers. Tool Search now lets Claude Code dynamically fetch tools only when needed. If the tool descriptions exceed 10% of context, they're [auto-switched to search mode]"

X discourse confirms: **lazy loading is the dominant conversation** in the Claude Code dev community right now. It's the hot topic. JIM could produce content on this — we have proprietary insight from comparing both systems.

---

## 7. OpenClaw vs Claude Code Architecture Comparison

Source: claude-world.com deep dive (1 week ago)

| Feature | OpenClaw | Claude Code |
|---------|----------|-------------|
| Skill format | SKILL.md in folder | SKILL.md in folder |
| Skill discovery | 2-tier lazy load ✅ | 2-tier lazy load ✅ |
| Context indexing | Partial (SOUL/MEMORY/AGENTS load full) | Index pattern available |
| MCP lazy loading | No | ✅ Native (Jan 2026) |
| Trigger routing | Description-based | Keyword trigger tables |
| Sub-agent context | Full inheritance | Configurable (minimal/full/none) |
| Autonomous skill learning | No | Claudeception (community) |
| Memory architecture | MEMORY.md + Supermemory | CLAUDE.md + external RAG |
| Heartbeat/proactive | ✅ (unique to OpenClaw) | No |
| Multi-node fleet | ✅ (unique to OpenClaw) | No |

**Verdict:** OpenClaw is ahead on autonomous operation. Claude Code is ahead on context efficiency.

---

## 8. Actionable Improvements for Our Fleet

### Priority 1 (Quick wins, do this week)
- [ ] **MEMORY.md compression** — Convert to index pattern. Keep full MEMORY.md but create MEMORY.index with trigger keywords → section pointers. Load full content only when triggered. Est. savings: **8-12k tokens/session**
- [ ] **Install context-optimizer skill** from `openclaw/skills/ad2546/context-optimizer` — semantic + query-aware compaction
- [ ] **Slim AGENTS.md** — Extract routing rules to a minimal trigger table. Full agent profiles load on-demand.

### Priority 2 (This month)
- [ ] **Claudeception for AWCN** — Adapt for OpenClaw. When fleet agents discover patterns, auto-write skills. Start with Andy + Leet.
- [ ] **Trigger table for skills** — Add explicit keyword triggers to AVAILABLE_SKILLS descriptions (helps reduce ambiguous reads)
- [ ] **Sub-agent context budget** — When spawning fleet sub-agents, pass only the context they need (not full SOUL/MEMORY/AGENTS)

### Priority 3 (Longer term / gICM product)
- [ ] **MCP Tool Search equivalent** — Apply lazy-loading pattern to our MCP gateway calls. Load tool schemas on-demand.
- [ ] **Usage-based skill retirement** — Track which skills fire, retire ones with 0 invocations in 30 days
- [ ] **Publish our approach** — Blog post / gICM listing on "OpenClaw's 2-tier skill lazy loading." We're ahead on this vs Claude Code's manual implementation.

---

## 9. Repos to Star / Monitor

```
github.com/hesreallyhim/awesome-claude-code     # Community aggregator
github.com/VoltAgent/awesome-agent-skills        # 300+ official skills, cross-platform
github.com/blader/Claudeception                  # Autonomous skill learning
github.com/avifenesh/agentsys                    # Multi-agent workflow automation
github.com/trailofbits/skills                    # Security skills
github.com/anthropics/skills                     # Official Anthropic skills
github.com/ComposioHQ/awesome-claude-plugins     # Plugins (slash commands, agents, hooks)
```

---

## 10. Content Angles for JIM

1. **"Why OpenClaw's 2-Tier Skill Loading Beats Claude Code's Manual Approach"** — We figured this out before it was trendy. Publish our architecture.
2. **"54% Token Reduction in AI Agent Sessions: The Lazy Loading Pattern"** — Technical deep dive, dev audience
3. **"Every Major Company Now Has Claude Code Skills: What This Means for the Agent Economy"** — Vercel, Cloudflare, Stripe, Google, Hugging Face all published official skills this week
4. **"Claudeception: The Agent That Writes Its Own Skills"** — Product idea angle for gICM

---

*Research by Andy | Sources: GitHub, X, Brave Search, web_fetch | Feb 20, 2026*

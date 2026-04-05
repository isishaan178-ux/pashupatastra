<p align="center">
  <img src="assets/banner.png" alt="Pashupatastra Banner" width="100%">
</p>

<h1 align="center">Pashupatastra</h1>

<p align="center">
  <em>Named after Lord Shiva's unstoppable celestial weapon</em><br>
  <strong>The adaptive multi-agent orchestrator that gives your Claude Code a full org chart.</strong>
</p>

<p align="center">
  <a href="#installation"><img src="https://img.shields.io/badge/Install-2%20Steps-brightgreen?style=for-the-badge" alt="Install"></a>
  <a href="#how-it-works"><img src="https://img.shields.io/badge/Agents-9%20Specialized-orange?style=for-the-badge" alt="Agents"></a>
  <a href="#token-efficiency"><img src="https://img.shields.io/badge/Token%20Savings-Optimized-red?style=for-the-badge" alt="Token Savings"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-blue?style=for-the-badge" alt="License"></a>
</p>

<p align="center">
  <a href="#the-problem">The Problem</a> &bull;
  <a href="#the-solution">The Solution</a> &bull;
  <a href="#how-it-works">How It Works</a> &bull;
  <a href="#installation">Installation</a> &bull;
  <a href="#roadmap">Roadmap</a>
</p>

---

## The Problem

Every time you ask Claude Code to build something, three things go wrong:

| Problem | What Happens | Token Cost |
|---------|-------------|------------|
| **No planning** | Claude jumps straight to code. Builds the wrong thing. You redo it. | 2-3x wasted |
| **Yes-man behavior** | Claude never pushes back. Your bad idea becomes bad code. | Entire session wasted |
| **Serial execution** | Claude does everything one step at a time, even when tasks are independent. | Slow + expensive |

**The result?** You burn through tokens, get mediocre output, and spend more time fixing than building.

---

## The Solution

**What if Claude Code had a full organization working for it?**

Pashupatastra gives Claude an adaptive team that scales based on task complexity. Small tasks stay lean. Large tasks deploy a full organizational hierarchy.

```
You: "Build an auth system with OAuth and JWT"

Pashupatastra:
  Triage: L (System) - multi-component, needs design

  Planning...
  [Structured plan presented]

  -> Approve? (y/modify/reject)

  Deploying Challengers (parallel)...

  SKEPTIC: "JWT refresh token rotation is missing.
     Without it, stolen tokens have unlimited lifetime."

  OPTIMIZER: "OAuth + JWT + sessions is 3 auth systems.
     Pick OAuth with JWT tokens - drop sessions."

  USER ADVOCATE: "User said 'auth' but didn't mention
     signup. Clarify scope."

  -> Address concerns? (proceed/modify/ask me)

  Deploying Workers...
  |- Lead Backend (spawning 2 sub-workers)
  |  |- Worker B1: OAuth integration
  |  \- Worker B2: JWT + refresh tokens
  \- Lead Frontend (solo - 2 files)

  All workers complete. Review agent checking...
  Done.
```

**That's Pashupatastra.** Planning. Challenging. Executing. Reviewing. All with approval gates so you stay in control.

---

## How It Works

### Phase 1: Triage (~50 tokens)

Every task gets classified instantly. The skill reads your message and assigns a tier:

| Tier | When | What Happens | Agents Spawned |
|------|------|-------------|----------------|
| **S** | "Fix this typo" | Skip everything. Just do it. | 0 |
| **M** | "Add dark mode toggle" | Plan -> Approve -> 1 Worker | 1 |
| **L** | "Build auth with JWT" | Plan -> Challenge -> Approve -> Manager + Workers | 4-6 |
| **XL** | "Build a SaaS dashboard" | Plan -> Challenge -> Approve -> CEO -> Managers -> Workers | 8+ |

**Key insight:** S-tier tasks have zero overhead. The skill only activates the heavy machinery when it's actually needed.

### Phase 2: Planning

A dedicated Planner agent creates a structured implementation plan:
- **Goal** - One sentence
- **Approach** - Architecture decisions
- **File map** - Exact paths
- **Task breakdown** - With parallel opportunities marked

You approve, modify, or reject before anything gets built.

### Phase 3: Adversarial Challenge (L/XL only)

This is what makes Pashupatastra unique. Instead of Claude agreeing with everything, **3 specialized agents challenge your plan in parallel:**

| Agent | Role | Focus |
|-------|------|-------|
| **Skeptic** | "Why will this fail?" | Risks, edge cases, security holes |
| **Optimizer** | "How can this be simpler?" | Over-engineering, YAGNI violations |
| **User Advocate** | "Does this solve the actual problem?" | Requirement gaps, misunderstood intent |

**Rules for token efficiency:**
- All 3 run in **parallel** (not sequential)
- Each gets **ONE shot** - 50-100 words max
- **No back-and-forth** between challengers
- Results merged into one consolidated report

### Phase 4: Execution (Scaled by Tier)

The execution hierarchy adapts to task size:

```
XL Tier:

  CEO (Orchestrator)
    |
    |-- Manager: Backend        Manager: Frontend
    |      |                          |
    |   Worker: Auth Routes     Worker: Login UI
    |   Worker: JWT Logic       Worker: Signup UI
    |      |
    |   Sub-Worker: OAuth
    |   Sub-Worker: Refresh
```

**Lead Workers decide whether to spawn Sub-Workers based on:**
- 3+ independent sub-tasks? -> Spawn
- 3+ files to create? -> Spawn
- Otherwise? -> Do it solo

**No over-spawning.** Every agent earns its existence.

### Phase 5: Review

A final Review agent checks all work against the approved plan:
- Did workers complete every task?
- Any conflicts between parallel outputs?
- Missing imports, broken references?
- Pass/fail verdict with severity flags

---

## Token Efficiency

This is the core promise. Every design decision optimizes for fewer tokens:

| Strategy | How It Saves |
|----------|-------------|
| **Triage costs ~50 tokens** | Just classification, no deep analysis |
| **Challengers get plan only** | No codebase exploration during challenge phase |
| **Workers get ONLY their task** | Not the full plan, not other workers' tasks |
| **No agent-to-agent chat** | Agents report UP, never sideways |
| **Strict output limits** | 50-100 words per challenger, 100 words per manager |
| **Skip phases aggressively** | S-tier = 0 agents, M-tier = no challengers |
| **Parallel execution** | Faster wall-clock time = less context accumulation |

---

## Installation

### Step 1: Clone the skill

```bash
git clone https://github.com/isishaan178-ux/pashupatastra.git
```

### Step 2: Add to your Claude Code settings

Add to your `~/.claude/settings.json`:

```json
{
  "extraKnownMarketplaces": {
    "pashupatastra": {
      "source": {
        "source": "directory",
        "path": "/path/to/pashupatastra"
      }
    }
  },
  "enabledPlugins": {
    "pashupatastra@pashupatastra": true
  }
}
```

### Step 3: Test it

Open a new Claude Code session and type:

```
/pashupatastra build a REST API with authentication
```

You should see:
```
Pashupatastra Triage: L - Multi-file system with routes, models, middleware
```

---

## File Structure

```
pashupatastra/
|-- .claude-plugin/
|   |-- plugin.json              # Plugin manifest
|   \-- marketplace.json         # Marketplace manifest
|-- skills/
|   \-- pashupatastra/
|       |-- SKILL.md             # Main orchestrator
|       \-- agents/
|           |-- triage.md        # Complexity classifier
|           |-- planner.md       # Plan generation
|           |-- skeptic.md       # Challenger: risks
|           |-- optimizer.md     # Challenger: simplicity
|           |-- user-advocate.md # Challenger: user intent
|           |-- ceo.md           # XL orchestrator
|           |-- manager.md       # Task splitter
|           |-- worker.md        # Execution agent
|           \-- reviewer.md     # Final quality gate
|-- visual/
|   \-- index.html               # Flow diagram visualization
|-- assets/                      # Screenshots and banners
\-- docs/
    \-- specs/                   # Design documents
```

---

## What Makes Pashupatastra Different

| Feature | Default Claude | Superpowers | Pashupatastra |
|---------|---------------|-------------|---------------|
| **Planning before code** | No | Yes (brainstorming) | Yes (with approval gates) |
| **Challenges your ideas** | Never | No | Yes (3 adversarial agents) |
| **Parallel execution** | No | Yes (dispatching) | Yes (hierarchical + adaptive) |
| **Scales by complexity** | No | No | Yes (S/M/L/XL tiers) |
| **Token-efficient by design** | No | Partial | Yes (core principle) |
| **Org chart hierarchy** | No | No | Yes (CEO/Manager/Worker) |
| **Zero overhead for small tasks** | N/A | Skills always load | S-tier = skip everything |

---

## Roadmap

Pashupatastra is in **Phase 1** - a pure skill with no external dependencies. Here's where we're going:

### Phase 2: Multi-Skill Plugin
- [ ] Split into modular skills (triage, challenger, executor)
- [ ] Add token usage tracking per phase
- [ ] Support for custom challenger personas
- [ ] Integration with code-review-graph for codebase-aware triage

### Phase 3: Full Platform
- [ ] MCP server for real-time agent status dashboard
- [ ] Visual org chart in terminal showing live agent activity
- [ ] Marketplace publication for one-click install
- [ ] Multi-IDE support (Cursor, Windsurf, Codex)

### Phase 4: Community
- [ ] Custom agent templates (bring your own challenger)
- [ ] Shared triage rules across teams
- [ ] Plugin ecosystem for domain-specific workers

---

## The Name

**Pashupatastra** (Sanskrit: पाशुपतास्त्र) is the most powerful celestial weapon in Hindu mythology, bestowed by Lord Shiva himself. It is said to be unstoppable, capable of destroying anything in creation.

We named this skill after it because that's what it aspires to be - **the most powerful weapon in your Claude Code arsenal.** One skill that makes everything else work better.

---

## Contributing

Pashupatastra is open source and we welcome contributions:

1. **Try it** - Install it, use it, break it
2. **Report issues** - What didn't work? What confused you?
3. **Suggest challengers** - Have an idea for a new challenger persona? Open an issue
4. **Improve agents** - The agent prompts in `agents/` can always be refined
5. **Share your results** - Show us your token savings. We'll feature them here

---

## Support

If Pashupatastra saved you tokens, time, or sanity:

- Star this repo - it helps others find it
- Share it with your team
- Open issues with ideas - we read every one

---

<p align="center">
  <strong>Built with purpose. Optimized for efficiency. Named after a god's weapon.</strong><br>
  <em>Because your AI assistant deserves a full org chart.</em>
</p>

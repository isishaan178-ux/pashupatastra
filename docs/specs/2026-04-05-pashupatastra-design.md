# Pashupatastra - Design Specification

> The most powerful weapon in your Claude Code arsenal. Named after Lord Shiva's unstoppable celestial weapon.

**Core Promise:** Install one skill → Claude becomes a token-efficient AI organization that plans, challenges, and builds better than default Claude.

---

## Problem Statement

1. **Token waste** — Claude processes everything in one giant context, burning tokens on irrelevant work
2. **Yes-man behavior** — Claude approves bad ideas instead of challenging them
3. **No planning discipline** — Claude jumps to code without thinking through the approach
4. **Serial execution** — Claude does everything sequentially when tasks could be parallelized

## Solution: Adaptive AI Organization

Pashupatastra creates a dynamic multi-agent hierarchy that scales based on task complexity. Small tasks run lean. Large tasks deploy a full organizational structure with planning, adversarial challenge, and parallel execution.

---

## Architecture

### Phase 1: Triage (Near-Zero Token Cost)

A lightweight classifier that reads the user's message and scores complexity:

**Signals checked:**
- Message length and detail level
- Number of systems/components mentioned
- Keywords indicating scope (e.g., "build", "platform", "integrate" = large; "fix", "rename", "add" = small)
- Number of files likely affected
- Whether task is decomposable into independent sub-tasks

**Output — 4 Tiers:**

| Tier | Label | Criteria | Pipeline |
|------|-------|----------|----------|
| S | Quick Fix | Single file, single concept, < 2 min work | No agents. Just do it. |
| M | Feature | 1-3 files, single component, clear scope | 1 planner → approval → 1 worker |
| L | System | 3+ files, multiple components, needs design | Plan → 2 challengers → approval → manager + 2-3 parallel workers |
| XL | Platform | Multiple subsystems, cross-cutting concerns | Plan → 3 challengers cross-question → approval → CEO → manager → workers (can spawn sub-workers) |

### Phase 2: Planning (M/L/XL Only)

A single Planner agent creates a structured implementation plan:
- What to build and why
- File structure and component boundaries
- Task breakdown with dependencies marked
- Which tasks can run in parallel

**Output:** A concise plan document presented to the user.

### Phase 3: Adversarial Challenge (L/XL Only)

Parallel challenger agents, each with a distinct lens:

| Agent | Role | Focus |
|-------|------|-------|
| **Skeptic** | "Why will this fail?" | Risks, edge cases, failure modes, scalability issues |
| **Optimizer** | "How can this be simpler?" | Over-engineering, unnecessary complexity, YAGNI violations |
| **User Advocate** | "Does this solve the actual problem?" | Requirement gaps, UX issues, misunderstood intent |

**Rules for token efficiency:**
- All 3 run in parallel (not sequential)
- Each gets ONE shot — 50-100 word verdict max
- No back-and-forth between challengers
- Results merged into one consolidated report

**XL addition:** Challengers also cross-reference each other's concerns in their verdict (they receive each other's outputs in a single consolidation pass, NOT multi-round debate).

**Output:** Consolidated challenge report with actionable concerns → presented to user for approval.

### Phase 4: Execution (Scaled to Tier)

**Tier S:** No agents. Skill does the work directly.

**Tier M:**
- 1 worker agent executes the plan
- Reports back when done

**Tier L:**
- Manager agent reads approved plan
- Splits into 2-3 parallel task groups
- Spawns worker agents (isolated worktrees where possible)
- Workers execute independently
- Manager reviews all outputs

**Tier XL:**
- CEO agent orchestrates the full build
- Splits plan into major workstreams (e.g., "backend", "frontend", "infrastructure")
- Each workstream gets a Lead Worker
- Lead Workers assess their workstream:
  - If decomposable (3+ independent sub-tasks OR 3+ files to create): spawn Sub-Workers
  - If not: Lead does it themselves
- CEO collects all outputs, runs final coherence check

**Spawning heuristic (Lead Worker decision):**
- Number of functions/endpoints needed: 1-2 = solo, 3+ = spawn
- Number of files to create/modify: 1-2 = solo, 3+ = spawn
- Can sub-tasks run independently without shared state? Yes = spawn, No = solo

### Phase 5: Review

A single Review agent checks all work:
- Does it match the approved plan?
- Did workers introduce conflicts?
- Are there obvious bugs or missing pieces?

**Output:** Review report → presented to user.

---

## Approval Gates

```
[Triage] → auto (no approval needed)
[Plan ready] → PAUSE → User approves/requests changes
[Challenge report] → PAUSE → User acknowledges concerns, approves/modifies plan
[Each execution batch completes] → PAUSE → User reviews work
[Final review] → PAUSE → User accepts or requests fixes
```

---

## Token Efficiency Strategy

1. **Triage costs ~50 tokens** — just classification, no deep analysis
2. **Challengers get minimal context** — only the plan, not codebase exploration
3. **Workers get scoped context** — only their specific task + relevant files
4. **No agent-to-agent conversation** — agents report UP, never sideways
5. **Strict output limits** — challengers: 50-100 words; manager summaries: 100 words
6. **Skip phases when unnecessary** — S tier skips everything, M tier skips challengers
7. **Parallel execution** — faster wall-clock time = less total context accumulation

---

## User Experience

### What the user sees:

```
You: "Build an auth system with OAuth and JWT"

Pashupatastra:
  ⚡ Triage: L (System) — multi-component, needs design

  📋 Planning...
  [Plan presented]

  → Approve this plan? (y/modify/reject)

You: y

  🗡️ Deploying Challengers (3 parallel agents)...

  ⚔️ SKEPTIC: "JWT refresh token rotation is missing.
     Without it, stolen tokens have unlimited lifetime."

  ⚔️ OPTIMIZER: "OAuth + JWT + session management is 3 auth
     systems. Pick OAuth with JWT tokens — drop sessions."

  ⚔️ USER ADVOCATE: "The user said 'auth system' but didn't
     mention registration/signup. Clarify scope."

  → Address these concerns? (proceed/modify plan/ask me)

You: good points, add refresh tokens, drop sessions, and yes include signup

  📋 Plan updated.

  🏗️ Deploying Workers...
  ├── Lead Backend: auth routes + JWT logic (spawning 2 sub-workers)
  │   ├── Worker B1: OAuth integration
  │   └── Worker B2: JWT + refresh token logic
  └── Lead Frontend: login/signup UI (solo — 2 files)

  [Workers execute in parallel...]

  ✅ All workers complete.

  🔍 Review Agent checking coherence...

  → Review complete. 2 files created, 1 modified. Approve?
```

---

## File Structure

```
pashupatastra/
├── SKILL.md                    # Main skill — orchestration logic
├── agents/
│   ├── triage.md               # Complexity classifier prompt
│   ├── planner.md              # Plan generation prompt
│   ├── skeptic.md              # Challenger: risk focus
│   ├── optimizer.md            # Challenger: simplicity focus
│   ├── user-advocate.md        # Challenger: requirement focus
│   ├── ceo.md                  # XL orchestrator prompt
│   ├── manager.md              # L/XL task splitter prompt
│   ├── worker.md               # Execution agent prompt
│   └── reviewer.md             # Final review prompt
└── docs/
    ├── specs/                  # Design documents
    └── plans/                  # Implementation plans
```

---

## Constraints

- Pure skill — no MCP server, no external dependencies
- Single SKILL.md + agent prompt files
- Works with Claude Code's native Agent tool and worktrees
- Install in 10 seconds: copy to `~/.claude/skills/`
- Must not degrade experience for S-tier tasks (near-zero overhead)

---

## Future (Phase 2+)

- Token usage dashboard via MCP server
- Visual agent hierarchy in terminal
- Plugin packaging for marketplace distribution
- Integration with code-review-graph for codebase-aware triage

# 🧠 agent-brain

> Persistent memory infrastructure for AI coding agents, powered by Obsidian.
> Set up once with a single prompt. Never lose context again.

---

## The Problem

You're working with an AI coding agent — Antigravity 2.0, Claude Code, or Gemini CLI — on a large codebase. Every new session, the agent starts completely blind. It scans hundreds of files from scratch, burns through your token budget just to understand the project structure, and still makes mistakes because it doesn't know the decisions you made last week.

**Every session feels like onboarding a new developer who forgets everything overnight.**

---

## The Solution

`agent-brain` turns your Obsidian vault into a persistent, structured memory layer that lives right inside your project. A single prompt analyzes your entire codebase and builds an atomic knowledge graph — architecture, components, decisions, gotchas, patterns — all interlinked and ready for any agent to consume instantly.

The agent no longer scans your project. It reads the map.

```
Without agent-brain          With agent-brain
─────────────────────        ──────────────────────────
Session starts               Session starts
Agent scans 150 files   →    Agent reads INDEX.md
Wastes 40k tokens            Reads activeContext.md
Still misses context         Goes straight to work
Makes avoidable mistakes     Full context in 2 files
```

---

## How It Works

### 1. Run the setup prompt
Paste the prompt into your agent. It detects your project type automatically:

- **Project exists** → scans and builds the full brain
- **No project yet** → creates the skeleton, ready to fill later
- **Monorepo** → groups by workspace with intermediate nodes
- **Single app** → groups by feature with intermediate nodes
- **Small project** → flat structure, no intermediate nodes needed

### 2. Obsidian opens the vault
Point Obsidian at your project folder. The `brain/` directory becomes a visual, navigable knowledge graph — fully linked, browsable, editable.

### 3. Every session starts with context
The agent reads `INDEX.md` and `activeContext.md` — two lightweight files — and immediately knows:
- What the project does
- What's currently being worked on
- What the critical gotchas are
- Where every component lives

### 4. The brain grows with your project
At the end of every session, the agent updates the brain. New decisions, new gotchas, new patterns — all documented and linked automatically.

---

## Brain Structure

```
brain/
├── INDEX.md              ← Agent reads this first, every session
├── activeContext.md      ← What's being worked on right now
├── architecture.md       ← High-level map of the codebase
├── data-models.md        ← Database schema & relationships (if applicable)
│
├── components/
│   └── [group-name]/
│       └── [component-name].md
│
├── decisions/
│   ├── [package-name].md     ← Intermediate nodes (prevent supernodes)
│   └── [decision-name].md
│
├── gotchas/
│   └── [gotcha-name].md
│
├── patterns/
│   └── [pattern-name].md
│
└── logs/
    └── YYYY-MM-DD.md         ← Session logs, auto-generated
```

---

## Supernode Prevention

One of the biggest mistakes in AI knowledge graphs is the **supernode problem** — every node linking to one central file, making that file a black hole that pulls the agent's entire context into a single read.

`agent-brain` enforces a strict hierarchy:

```
❌ Bad — Supernode                    ✅ Good — Hierarchical

login-page ──┐                        INDEX
event-card ──┤                          └── apps-web
api ─────────┼──→ index.md                   └── login-page
forms ───────┤                                └── event-card
[60 more] ───┘                          └── packages-api
                                              └── auth
                                              └── api
```

**The 10-link rule**: If any single file receives more than 10 incoming links, the prompt automatically creates an intermediate node to distribute the connections.

---

## File Format

Every brain file follows a consistent schema:

```markdown
---
date: YYYY-MM-DD
type: component | decision | gotcha | pattern | architecture
status: active
---

[Content — max 30-40 lines]

Related: [[other-file]] [[another-file]]
```

Dates are always `YYYY-MM-DD` — no exceptions. This ensures consistent sorting across logs, decisions, and session history.

---

## Session Lifecycle

### Session Start
The agent reads only what it needs:

```
Always:
  1. brain/INDEX.md
  2. brain/activeContext.md

On demand:
  Component work    → brain/decisions/[group] → brain/components/[group]/
  New feature       → brain/patterns/ + brain/decisions/
  Debugging         → brain/gotchas/ (only when needed)
  Architecture      → brain/architecture.md
  Database work     → brain/data-models.md
```

### Session End
The agent updates the brain before closing:

```
1. Update changed components in brain/components/
2. Log new decisions in brain/decisions/
3. Document new gotchas in brain/gotchas/
4. Create brain/logs/YYYY-MM-DD.md
5. Update brain/activeContext.md
```

---

## Compatibility

| Agent | Status |
|-------|--------|
| Antigravity 2.0 | ✅ Full support |
| Claude Code | ✅ Full support |
| Gemini CLI | ✅ Full support |
| Codex CLI | ✅ Full support |
| Any markdown-aware agent | ✅ Works |

---

## Token Efficiency

The tiered loading strategy keeps token costs low:

- **Session start**: ~2 files (INDEX + activeContext) — lightweight
- **Task-specific**: only the relevant component group
- **Debugging only**: gotchas folder opened on demand
- **No full scans**: the agent navigates, not searches

---

## Getting Started

### Prerequisites
- [Obsidian](https://obsidian.md) (free)
- Any AI coding agent (Antigravity 2.0, Claude Code, Gemini CLI, etc.)

### Setup

**Step 1** — Copy the [setup prompt](./PROMPT.md) into your agent

**Step 2** — Wait for the agent to finish analyzing your project

**Step 3** — Open your project folder as an Obsidian vault

```
File → Open Folder as Vault → [your project folder]
```

**Step 4** — Open Graph View in Obsidian to see your knowledge graph

**Step 5** — Exclude non-markdown folders from the graph:

```
Settings → Files & Links → Excluded Files
Add: node_modules, dist, build, .git
```

That's it. Your agent now has persistent memory.

---

## Example Output

After running the setup prompt on a 150-file, 40k-line monorepo:

```
✓ 100 component files created
✓  5 decision files created (including 3 intermediate nodes)
✓  4 gotcha files created
✓  4 pattern files created
✓  0 broken wikilinks
✓  0 orphan files
✓  0 supernodes detected
```

---

## Best Practices

**Do:**
- Run the setup prompt once at the start of a new project
- Let the agent update the brain at the end of every session
- Review `brain/` files occasionally — correct anything the agent missed
- Use `brain/logs/` to track what changed over time

**Don't:**
- Add `brain/` to `.gitignore` — it's valuable history
- Let the brain grow stale — outdated context is worse than no context
- Override the 10-link rule — supernodes will hurt performance

---

## Contributing

Found an edge case the prompt doesn't handle? Open an issue or submit a PR.

Areas where contributions are especially welcome:
- Support for additional project types (mobile, embedded, ML pipelines)
- Language-specific gotcha templates
- Alternative agent rule formats (CLAUDE.md, GEMINI.md, etc.)

---

*Built from real pain. Tested on a 150-file, 40k-line production codebase.*

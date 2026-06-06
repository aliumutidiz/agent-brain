━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
OBSIDIAN BRAIN SETUP — UNIVERSAL PROMPT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PHASE 0 — DETECT SITUATION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

First, answer these questions:

1. Is there a project in the current directory?
   - Look for a dependency file:
     package.json, requirements.txt, Cargo.toml,
     go.mod, pom.xml, composer.json
   - Found → SITUATION A
   - Not found → SITUATION B

2. Did the user specify a directory?
   - Yes → scan that directory → SITUATION A
   - No → check current directory → apply check above

SITUATION A — Project exists:
  → Continue from PHASE 1

SITUATION B — No project yet:
  → Build only the brain/ skeleton and AGENTS.md
  → Create all files with default content
  → Write in activeContext.md:
     Status: Waiting. No project added yet.
     Brain is ready. Once the project is added,
     run the command: 'update the brain'.
  → Skip to PHASE 8 (final check)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PHASE 1 — PROJECT ANALYSIS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Scan the project from top to bottom.
Skip these directories entirely:
node_modules, dist, build, .git, .cache,
coverage, __pycache__, .next, .nuxt, vendor

While scanning, identify the following:

→ Technology stack
→ Folder structure and the purpose of each folder
→ Is this a single app or a monorepo?
   - Monorepo: identify main packages and workspaces
   - Single app: identify feature groups
→ Single-sentence purpose of each file
→ Exported functions and component names
→ Repeating code patterns
→ Configuration decisions (why this library was chosen)
→ Lines containing TODO, FIXME, HACK, workaround
→ Complex or fragile code blocks
→ Data models and their relationships

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PHASE 2 — INTERMEDIATE NODE STRATEGY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Based on the analysis, choose a grouping strategy:

Monorepo:
  Intermediate node = each package/workspace
  Example: apps-web, apps-admin, packages-api

Single app:
  Intermediate node = each feature group
  Example: feature-auth, feature-dashboard,
           feature-settings

Small project (fewer than 20 files):
  No intermediate nodes needed
  Components can link directly to INDEX

Rule:
  If any single file receives more than
  10 incoming links → create an intermediate node

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PHASE 3 — FOLDER STRUCTURE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

brain/
├── INDEX.md
├── activeContext.md
├── architecture.md
├── data-models.md        ← only if data models exist
├── components/
│   └── [group-name]/
│       └── [component-name].md
├── decisions/
│   ├── [intermediate-node-name].md
│   └── [decision-name].md
├── gotchas/
│   └── [gotcha-name].md
├── patterns/
│   └── [pattern-name].md
└── logs/

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PHASE 4 — FILE FORMAT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Every file must start with this template:

---
date: YYYY-MM-DD
type: component | decision | gotcha | pattern | architecture
status: active
---

Rules:
- All dates must strictly follow YYYY-MM-DD format
- Every file maximum 30-40 lines
- If a file exceeds 40 lines, split it
- File names: lowercase, hyphen-separated,
  no special characters
- Link related files with [[wikilinks]]
- Do not add personal commentary,
  only reflect the project as-is

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PHASE 5 — FILE CONTENTS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

INDEX.md
→ Single-sentence purpose of the project
→ Technology stack summary
→ [[wikilinks]] to all intermediate nodes
→ [[wikilinks]] to the 3-5 most critical gotchas
→ [[activeContext]] and [[architecture]] links

architecture.md
→ Folder structure and purpose of each folder
→ Data flow
→ External dependencies and integrations
→ [[wikilinks]] to intermediate nodes

data-models.md (if applicable)
→ Each model: name, fields, relationships
→ Only extract from existing schema/model files
→ [[wikilinks]] to related components and patterns

components/[group]/[component].md
→ Single purpose of the component (1-2 sentences)
→ List of exported functions/props (names only)
→ [[wikilinks]] to dependent components
→ [[wikilinks]] to related patterns or decisions
→ [[wikilink]] to its own intermediate node

decisions/[intermediate-node].md
→ Single-sentence purpose of this group
→ [[wikilinks]] to components it contains
→ [[wikilinks]] to related decisions

decisions/[decision].md
→ What was decided (1 sentence)
→ Why this choice was made
→ Why alternatives were rejected
→ [[wikilinks]] to affected intermediate nodes

gotchas/[gotcha].md
→ What is the problem (1 sentence)
→ Why it occurs
→ How to prevent / fix it
→ [[wikilinks]] to related files

patterns/[pattern].md
→ When to use this pattern
→ Code example (maximum 10-15 lines)
→ [[wikilinks]] to components using this pattern

activeContext.md
→ Write this content:
   Status: Idle. No active task assigned.
   Last updated: YYYY-MM-DD

logs/
→ Create the folder, leave it empty

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PHASE 6 — WIKILINK HIERARCHY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Strictly follow this hierarchy
to prevent supernode formation:

INDEX
  └→ decisions/[intermediate-node]
       └→ components/[group]/[component]

Forbidden connections:
✗ Components must not link directly to INDEX
✗ Components must not link directly to
  a top-level decision
✗ No single file should receive
  more than 10 incoming links

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PHASE 7 — AGENTS.md
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Write the following into AGENTS.md:

# Session Start
At the beginning of every session, read in order:
1. brain/INDEX.md
2. brain/activeContext.md

Read on demand based on the task:
- Component change →
  open the relevant intermediate node,
  then the specific component file
- New feature →
  brain/patterns/ + brain/decisions/
- Debugging →
  brain/gotchas/ (only open when needed)
- Architecture question →
  brain/architecture.md
- Database work →
  brain/data-models.md

# Session End
Before closing every session:
1. Update changed components in brain/components/
2. Add new decisions as files in brain/decisions/
3. Add discovered gotchas in brain/gotchas/
4. Create brain/logs/YYYY-MM-DD.md with:
   - Files that were changed
   - Decisions that were made
   - Unfinished work
5. Update brain/activeContext.md:
   - Current feature being worked on
   - Next step
   - Last updated: YYYY-MM-DD

# General Rules
- All dates must strictly follow YYYY-MM-DD
- If you need details, open the source file —
  never copy source code into the brain
- Every file maximum 30-40 lines —
  split if it grows larger
- If a file receives more than 10 incoming
  links, create an intermediate node
- Maintain [[wikilink]] hierarchy at all times
- Do not add personal commentary,
  only reflect the project as-is

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PHASE 8 — FINAL CHECK
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

When finished, report the following:

Detected project type:
- Monorepo / Single app / No project

File counts:
- Component files: how many
- Decision files: how many
- Gotcha files: how many
- Pattern files: how many
- Intermediate nodes: how many

Link check:
- Any file with more than 10 incoming links?
- Any broken [[wikilinks]]?
- Any orphan (unlinked) files?
- Any referenced but uncreated files?

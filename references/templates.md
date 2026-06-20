# ICM Templates and Examples

## Table of contents
1. `CLAUDE.md` template (the Map)
2. `CONTEXT.md` template (a Room)
3. `REFERENCES.md` template (optional companion)
4. Layer 3 config stub (stable-reference variant)
5. `_dependencies.md` template (all projects)
6. Full project tree examples (adapt to your context)
   - Content Creator
   - Freelancer / Agency
   - Developer

---

## 1. `CLAUDE.md` template (the Map)

Keep under ~50 lines. Identity → workspaces → naming → routing.

```markdown
# [Project / Practice Name]
I am [role / what this agent does].

## Workspaces
- /[workspace-a] — [one-line purpose].
- /[workspace-b] — [one-line purpose].

## Naming conventions
- Drafts: `topic-name_draft.md`
- Published: `YYYY-MM-platform-topic.md`
- [any other domain-specific rule]

## Routing
| Task | Go to | Read |
|------|-------|------|
| [task description] | /[workspace] | CONTEXT.md |
| [task description] | /[workspace]/[stage] | CONTEXT.md |
```

If Layer 3 skills/tools are wired in, add a fourth column (reference each skill only where it's used):

```markdown
## Routing
| Task | Go to | Read | Skills |
|------|-------|------|--------|
| Write code | /src | CONTEXT.md | testing-skill |
| Write docs | /docs | CONTEXT.md | doc-authoring-skill |
| Spec a feature | /planning | CONTEXT.md | — |
```

---

## 2. `CONTEXT.md` template (a Room)

Explicit contract. ~80% of the file describes the actual work; ≤20% describes agent behavior.

```markdown
# Stage: [name, e.g. 02_script]
> Last updated: YYYY-MM-DD

## Inputs
- Layer 4 (working): ../01_research/output/
- Layer 3 (reference): ../../_config/voice.md   ← name every Layer 3 file explicitly by path;
                                                    this is how grep finds dependents when you update a config

## Process
1. [Concrete step — what to read.]
2. [Concrete step — what to do.]
3. [Concrete step — constraints/format to apply.]

## Outputs
- [filename.md] -> output/

## Notes / constraints
- [Audience, tone, length, anything stage-specific.]
```

> **Why explicit paths matter:** If you later update `voice.md`, running `grep -r "voice.md" . --include="*.md"` instantly shows every CONTEXT.md that depends on it. Vague references (`"read the config"`) break this. See "Keeping Layer 3 references fresh" in `SKILL.md`.

---

## 3. `REFERENCES.md` template (optional companion to `CONTEXT.md`)

Use inside a workspace when there's enough passive background material that putting it in `CONTEXT.md` would bloat the contract. Holds things the agent should *know* but not act on directly.

```markdown
# References — [workspace name]

> Background material. Read for context; do not treat as instructions.

## Examples
- [links or pasted examples of good output]

## Style / voice notes
- [notes the agent should be aware of]

## Source material
- [research, prior work, anything to reference but not act on]
```

---

## 4. Layer 3 config stub (stable-reference variant — `_config/voice.md` etc.)

```markdown
# [Voice / Style / Design System]

> Layer 3 reference. Stable across runs. Fill this in once.

## [Section, e.g. Tone]
- TODO: [user fills in]

## [Section, e.g. Do / Don't]
- TODO: [user fills in]
```

---

## 5. `_dependencies.md` template

Maintain this file in every ICM project — small or large. It gives you an explicit reverse map of which CONTEXT.md files depend on which Layer 3 files, so you don't rely on grep alone after a change. On small projects this file is short; that is fine. Place it at the project root alongside `CLAUDE.md`.

```markdown
# Layer 3 Dependency Map
> Last full audit: YYYY-MM-DD

When you update a Layer 3 file, run:
  grep -r "[filename]" . --include="*.md"
Then review every CONTEXT.md in the results and update its "Last updated" line.

---

## _config/voice.md
Last updated: YYYY-MM-DD — [one-line summary of what changed]

Depends on this file:
- script-lab/01_research/CONTEXT.md
- script-lab/02_script/CONTEXT.md
- distribution/CONTEXT.md

---

## _config/platforms.md
Last updated: YYYY-MM-DD — [one-line summary of what changed]

Depends on this file:
- distribution/CONTEXT.md
```

> **Keep it short.** One entry per Layer 3 file. No nested checklists — the review happens in the CONTEXT.md files themselves, not here. This file is a lookup, not a task tracker.

---

## Full project tree examples (adapt to your context)

The three examples below show how ICM structures adapt to different work types. **These are starting points, not prescriptions.** Adjust workspaces, naming, stages, and Layer 3 references to match your actual needs.

### 5. Full project tree: Content Creator

```
content/
├── CLAUDE.md
├── _config/
│   ├── voice.md            # Layer 3: brand voice
│   └── platforms.md        # Layer 3: per-platform formatting (LinkedIn vs YouTube)
├── script-lab/             # Workspace: idea dev + drafts
│   ├── CONTEXT.md
│   ├── 01_research/
│   │   ├── CONTEXT.md
│   │   └── output/
│   └── 02_script/
│       ├── CONTEXT.md
│       └── output/
├── production/             # Workspace: specs, briefs, thumbnails
│   ├── CONTEXT.md
│   └── output/
└── distribution/           # Workspace: platform formatting
    ├── CONTEXT.md
    └── output/
```

---

### 6. Full project tree: Freelancer / Agency

Each client gets a folder with its own `CONTEXT.md` — a strict boundary so the agent never leaks pricing or context between clients.

```
agency/
├── CLAUDE.md
├── _config/
│   └── house-style.md
├── client-alpha/
│   ├── CONTEXT.md
│   └── output/
├── client-beta/
│   ├── CONTEXT.md
│   └── output/
├── templates/              # Reusable proposals & reports
│   ├── CONTEXT.md
│   └── output/
└── business-dev/           # Pipeline + outreach
    ├── CONTEXT.md
    └── output/
```

---

### 7. Full project tree: Developer

```
project/
├── CLAUDE.md
├── _config/
│   └── coding-standards.md
├── planning/               # Architecture decisions, specs
│   ├── CONTEXT.md
│   └── output/
├── src/                    # Codebase + testing rules
│   └── CONTEXT.md
└── docs/                   # API guides, changelogs
    ├── CONTEXT.md
    └── output/
```

---

## Tooling notes (mention to user where relevant)

- **Claude Code / Cursor** read the folder tree natively; Claude Code reads `CLAUDE.md` on entering a directory and knows its job immediately.
- **Obsidian** visualizes the markdown files as an interconnected graph, letting the user watch the glass box fill in as stages produce outputs.

# ICM Builder

**Interpretable Context Methodology** — a filesystem-based way to structure AI work using numbered folders and markdown files instead of heavy agent frameworks.

ICM replaces code-based orchestration (LangChain, CrewAI, etc.) with **plain filesystem structure**: numbered folders and markdown files tell a single orchestrating AI agent exactly what to read and do at each step. The result is a **glass box** — every intermediate output is an editable plain-text file, context loads per-stage instead of all at once, and the system is observable and deterministic.

## 🎯 What is ICM?

ICM is structured as **three layers**:

1. **Layer 1 — The Map** (`CLAUDE.md`)  
   Project identity, folder structure, naming conventions, and a Routing Table that answers "Where do I go?"

2. **Layer 2 — The Rooms** (`CONTEXT.md` per workspace)  
   Explicit Inputs / Process / Outputs contract that answers "What do I do?"

3. **Layer 3 — The Tools**  
   Pluggable resources: skills, MCP servers, or stable reference files (brand voice, style guides) wired per workspace.

## 🚀 Quick Start

### For Claude Code users

1. **Download the skill** from [Releases](../../releases) or via Claude Code directly
2. Open Claude Code in your project
3. Import the skill — it's ready to use
4. When setting up a new ICM project, the skill scaffolds the structure for you

### For everyone

1. **Read** `SKILL.md` to understand the three-layer model
2. **Use templates** in `references/templates.md` to start your first project:
   - Copy the `CLAUDE.md` template and adapt it to your workspaces
   - Create numbered stage folders with `CONTEXT.md` files
   - Fill in the Routing Table
3. **Test with one workspace first** — don't build the whole system before producing output

## 📂 Project Contents

- **`SKILL.md`** — Full skill documentation: core mental model, build sequence, conventions, common mistakes
- **`references/templates.md`** — Ready-to-use templates for `CLAUDE.md`, `CONTEXT.md`, `REFERENCES.md`, plus three full project-tree examples (content creator, freelancer/agency, developer)

## 💡 When to Use ICM

✅ **Great for:**
- Projects with multiple stages or "mental modes" (research → write → edit)
- Teams or solo practitioners managing complex workflows
- Work where intermediate outputs need to be edited by humans
- Any AI-driven process that needs to be observable and maintainable

❌ **Probably not the right fit for:**
- Single-step tasks (write one email, generate one image)
- Highly technical code-generation where agent frameworks shine

## 🛠️ Key Concepts

### Routing Table (mandatory)

Every `CLAUDE.md` includes a 3-column (or 4-column with skills) table that routes tasks to workspaces:

```markdown
| Task | Go to | Read |
|------|-------|------|
| Plan the campaign | /planning | CONTEXT.md |
| Write first draft | /writing/01_script | CONTEXT.md |
| Edit and polish | /writing/02_edit | CONTEXT.md |
```

This replaces guessing — the agent knows exactly where to look.

### Naming Replaces a Database

Define naming conventions in `CLAUDE.md`. Example:
- Drafts: `topic-name_draft.md`
- Published: `YYYY-MM-platform-topic.md`

With clear conventions, "pull my Q3 product updates and build a spec" tells the agent exactly where to find what it needs.

### Outputs Are Edit Surfaces

Every stage writes plain-text files to its `output/` folder. This is what makes the system a glass box — humans can edit heavily at the first stage (to set direction) and last stage (to polish), while middle stages run constrained by reference files.

## 📖 Example: Content Creator

```
content/
├── CLAUDE.md                 # Project map
├── _config/
│   ├── voice.md              # Brand voice (Layer 3 reference)
│   └── platforms.md          # Per-platform formatting rules
├── script-lab/               # Workspace: idea dev + drafts
│   ├── CONTEXT.md
│   ├── 01_research/
│   │   ├── CONTEXT.md
│   │   └── output/
│   └── 02_script/
│       ├── CONTEXT.md
│       └── output/
├── production/               # Workspace: specs, briefs
│   ├── CONTEXT.md
│   └── output/
└── distribution/             # Workspace: platform formatting
    ├── CONTEXT.md
    └── output/
```

See `references/templates.md` for complete examples.

## 🏗️ Build Sequence

1. **Identify mental modes** — what 2–4 distinct "modes" does your work require?
2. **Write root `CLAUDE.md`** — identity + workspaces + naming + routing (keep it under 50 lines)
3. **Create `_config/`** for stable reference files (voice, style, design)
4. **Scaffold numbered stages** — each stage gets `CONTEXT.md` + `output/`
5. **Write each `CONTEXT.md`** — explicit Inputs / Process / Outputs contract
6. **Add `REFERENCES.md`** if a workspace needs lots of passive background material
7. **Wire Layer 3** — reference skills/tools only where they're needed
8. **Test, fail, update** — trace errors back to source files (`CONTEXT.md`, reference rules) and fix there

**Pro tip:** Start small. Build one workspace, get it working, then iterate.

## 🔗 Common Mistakes to Design Against

- `CLAUDE.md` too long — it's a **router**, not a project brief
- Missing Routing Table — agent guesses and wastes tokens
- Too many workspaces — only split on a genuine mental-mode shift
- Over-describing AI persona instead of describing the work
- Stale context files — update them when requirements change
- Building everything before testing — ship one workspace first

## 🙋 How to Use This Skill

**In Claude Code or an AI editor:**

> "Set up an ICM workspace for [my project type]"

The skill will:
1. Ask you a few clarifying questions about your workspaces and workflow
2. Scaffold the full folder structure
3. Create your `CLAUDE.md` and initial `CONTEXT.md` files
4. Point out what you need to fill in next

**Manually:**

Copy the templates from `references/templates.md`, adapt them to your project, and follow the build sequence above.

## 📚 Further Reading

- **Layer 1 (Map):** `SKILL.md` → The Map section
- **Layer 2 (Rooms):** `SKILL.md` → The Rooms section  
- **Layer 3 (Tools):** `SKILL.md` → The Tools section
- **Templates & Examples:** `references/templates.md`

## 🤝 Contributing

Found a bug? Have an idea for a template or example? Open an issue or PR!

## 📄 License

MIT License — see LICENSE file for details.

---

**Questions?** Check the FAQ in `SKILL.md` or open an issue on GitHub.

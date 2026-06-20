---
name: icm-builder
description: Build and reason about projects using Interpretable Context Methodology (ICM) — a filesystem-based way to structure AI work using numbered folders and markdown files (CLAUDE.md, CONTEXT.md) instead of code-heavy agent frameworks. Use this skill whenever the user wants to scaffold a new ICM workspace, set up a CLAUDE.md routing file, design CONTEXT.md stage contracts, organize an AI project into numbered stages with review gates, or convert a messy single-prompt workflow into a structured "glass-box" folder system. Also trigger when the user mentions ICM, "context engineering," "context methodology," staged markdown pipelines, or asks how to organize folders so an AI agent loads only the context it needs per step — even if they don't say "ICM" explicitly.
---
 
# ICM Builder
 
Interpretable Context Methodology (ICM) replaces code-based agent orchestration (LangChain, CrewAI, etc.) with the plain structure of a filesystem: numbered folders and markdown files tell a single orchestrating agent exactly what to read and do at each step. The goal is a **glass box** — every intermediate output is an editable plain-text file, context loads per-stage instead of in one bloated prompt, and the system is observable and deterministic.
 
This skill helps you (1) understand ICM well enough to reason about it, and (2) scaffold a working ICM project on the filesystem.
 
## Core mental model
 
ICM is taught as **three layers** (the Map, the Rooms, the Tools). A fuller research treatment expands these into **five formal layers**; lead with the three, and reach for the five only when precision demands it.
 
- **Layer 1 — The Map** = `CLAUDE.md` at the project root. Answers "Where am I?" and "Where do I go?". Defines identity, folder structure, naming conventions, and a **Routing Table**. It is a *router*, not a brief — keep it under ~50 lines, fitting on one screen.
- **Layer 2 — The Rooms** = a `CONTEXT.md` inside each workspace/stage folder. Answers "What do I do?" via an explicit **Inputs / Process / Outputs** contract. Optionally paired with a `REFERENCES.md` (see below).
- **Layer 3 — The Tools** = the pluggable resources a workspace draws on. **There are two valid flavors of Layer 3; a project can use either or both:**
  - **Skills / tools wired per workspace.** Skills, MCP servers, and tools referenced only in the workspaces that need them (e.g. a testing skill in `/src`, a humanizer in the writing workspace). Surfaced via an optional **Skills column** in the routing table. This is the plug-and-play model: reference many skills project-wide, but each workspace loads only its own.
  - **Stable reference files ("the factory").** Configured-once rules — brand voice, style guides, design systems — kept in a shared config folder (e.g. `_config/`) and pointed to from `CONTEXT.md` inputs.
**The formal 5-layer view** (use only when needed): Layer 0 Global Identity + Layer 1 Task Routing both live in `CLAUDE.md`; Layer 2 Stage Contracts = `CONTEXT.md`; Layer 3 Reference Material = the stable "factory" rules; Layer 4 Working Artifacts = run-specific files (transcripts, notes, prior stage outputs) that change every run and live in each stage's `output/`.

 
### `REFERENCES.md` (optional companion to `CONTEXT.md`)
 
A workspace may include a `REFERENCES.md` alongside its `CONTEXT.md`. It holds background material the agent should *know about but not act on directly* — examples, links, style notes, prior work. Keep the acting instructions in `CONTEXT.md` and the passive background in `REFERENCES.md`. Use it when a workspace has enough reference material that stuffing it into `CONTEXT.md` would bloat the contract.
 

## When to build a workspace vs. a stage
 
A **workspace** is a top-level folder for a distinct *mental mode* of work (e.g. writing content vs. writing code). Aim for 2–4 workspaces total. Don't create a new workspace for tasks that share a mental mode — drafting and editing belong together.
 
A **stage** is a numbered subfolder inside a workspace representing one step of a pipeline (`01_research`, `02_script`, `03_edit`). Each stage is isolated and governed by its own `CONTEXT.md` contract.
 

## Build sequence
 
Follow this order. **Start small** — build one workspace, test it, then iterate. Never scaffold six workspaces before producing a single output.
 
1. **Identify mental modes.** Ask the user what 2–4 areas of work require different "modes." Each becomes a workspace. If they only need one to start, build only one.
2. **Write the root `CLAUDE.md` (the Map).** Identity + workspaces + naming conventions + Routing Table. Keep it under 50 lines. Use `references/templates.md`.
3. **Create `_config/` for Layer 3 reference files** if there are stable rules (voice, style, design). Stub them; the user fills them in.
4. **Scaffold workspaces and numbered stages.** Each stage folder gets a `CONTEXT.md` and an empty `output/`.
5. **Write each `CONTEXT.md` (the Rooms).** Explicit Inputs / Process / Outputs. Spend ~80% of the file describing the actual work and constraints; ≤20% on agent persona/behavior. Add a `REFERENCES.md` alongside it if the workspace has substantial passive background material. Consider a "Last updated: YYYY-MM-DD" line at the top so staleness is visible at a glance.
6. **Wire Layer 3 where needed.** If using skills/tools, add a fourth **Skills** column to the routing table and reference each skill only in the workspaces that use it. If using stable reference files, point to them from `CONTEXT.md` inputs. A project can do both.
7. **Test, fail, and update the factory.** When the agent errs, don't just patch the final output — trace the error to the source file (a `CONTEXT.md` or a reference rule) and fix it there. Context files are living documents.

## File templates
 
Read `references/templates.md` for ready-to-use `CLAUDE.md` and `CONTEXT.md` skeletons and three full project-tree examples (content creator, agency/freelancer, developer). Use them verbatim as starting points and adapt to the user's domain.
 

## Key conventions
 
- **Naming replaces a database.** No RAG/SQL needed; naming rules route information. Define them in `CLAUDE.md`. Examples: drafts as `topic-name_draft.md`, published as `YYYY-MM-platform-topic.md`. With clear conventions, "pull my demo v2 and build a spec" tells the agent exactly where to look.
- **Routing Table is mandatory.** Without it the agent guesses what to read and burns tokens. Use a 3-column table: `Task | Go to | Read`. Add a 4th `Skills` column only if Layer 3 skills/tools are wired in.
- **Outputs are edit surfaces.** Every stage writes plain-text files to its `output/`. This is what makes the system a glass box and enables the U-shaped intervention pattern (humans edit heavily at the first stage to set direction and the last stage to polish; middle stages run constrained by reference files).

## Common mistakes to design against
 
- `CLAUDE.md` too long — it's a router, not a project brief (<50 lines).
- Missing Routing Table — agent guesses and wastes tokens.
- Too many workspaces — only split on a genuine mental-mode shift.
- Over-describing the AI persona instead of the work — describe the project, constraints, and audience.
- Stale context files — update them when requirements change.
- One flat folder of many files — once a folder holds more than ~8–10 files at one level, use subfolders/stages so the agent doesn't read everything.
- Building the whole system before testing — ship one workspace first.

## Output format
 
When scaffolding, create the actual files and folders on disk, then show the user the resulting tree and a short explanation of what to fill in next. Don't just describe the structure in prose — build it. After building, point out the Layer 3 stubs in `_config/` that need the user's input.

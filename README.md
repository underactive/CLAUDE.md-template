# CLAUDE.md Template

A structured template for creating `CLAUDE.md` files — project context documents that give [Claude Code](https://claude.ai/code) (and other AI assistants) the knowledge they need to work effectively in your codebase.

## Why

AI coding assistants work better when they understand your project's architecture, conventions, and gotchas upfront. A `CLAUDE.md` file provides this context in a format that's easy to maintain alongside your code. Without it, the assistant has to rediscover your project structure, build system, and design decisions every session.

## What's Included

| File | Description |
|------|-------------|
| `CLAUDE_TEMPLATE.md` | The main template — copy this into your project as `CLAUDE.md` and fill in the bracketed placeholders |
| `docs/CLAUDE.md/version-history.md` | Changelog template |
| `docs/CLAUDE.md/future-improvements.md` | Ideas backlog template |
| `docs/CLAUDE.md/testing-checklist.md` | Example QA checklist (from an embedded BLE device project) |
| `docs/CLAUDE.md/plans/` | Directory for plan, implementation, and audit records (epoch-prefixed for chronological ordering) |

## Usage (confirmed with Claude 2.1.62 / Opus 4.6)

1. Copy `CLAUDE_TEMPLATE.md` into your project root
2. Optionally copy the `docs/CLAUDE.md/` templates for supplementary documentation

### A. Existing codebase

3. In Claude CLI, run `/init`
4. Prompt: `redo @CLAUDE.md using @CLAUDE_TEMPLATE.md as the template. do not remove any headings from the template. fill in the gaps from analyzing the project`

### B. New project (planning from scratch with Claude)

3. Before initiating your plan, include the following directive in your prompt to Claude: `follow the Plan Pre-Implementation, Plan Post-Implementation, Post-Implementation Audit, Audit Post-Implementation directives in @CLAUDE_TEMPLATE.md`
4. Claude will execute the plan and generate the corresponding documentation (`plan.md`, `implementation.md`, `audit.md`) alongside the project code
5. Once the project has been implemented, prompt: `redo @CLAUDE.md using @CLAUDE_TEMPLATE.md as the template. ensure any info from the original is not lost in the redo (either in the main doc or subdocs in docs/CLAUDE.md/*.md). do not remove any headings from the template. fill in the gaps from analyzing the project`

## Template Sections

- **Project Overview** — Name, description, version, status
- **Hardware** — MCU, components, pin assignments (removable for software projects)
- **Architecture** — Core files, dependencies, key subsystems, protocols
- **Build Configuration** — Build tool rationale, environment variables
- **Code Style** — Linter, formatter, key conventions
- **External Integrations** — Third-party services and SDKs
- **Known Issues / Limitations** — Current bugs and constraints
- **Development Rules** — Opinionated rules that prevent real bug classes (input validation, bounds checking, bounded formatting, etc.)
- **Plan Pre-Implementation** — Write the finalized plan before implementing; scan prior plans for context
- **Plan Post-Implementation** — Record what was implemented, files changed, and verification steps
- **Post-Implementation Audit** — Run parallel audit subagents across 7 categories (QA, security, state, etc.)
- **Audit Post-Implementation** — Fix audit findings, flag resolved items, append verification checklist
- **Common Modifications** — Step-by-step recipes for routine changes
- **File Inventory** — Quick-reference file/directory table
- **Build Instructions** — Prerequisites, quick start, troubleshooting
- **Testing** — Links to QA checklist
- **Future Improvements** — Links to ideas backlog
- **Maintaining This File** — Guidelines for keeping the document current

## Tips

- **Start small.** You don't need to fill every section on day one. Start with Architecture, Build Instructions, and Development Rules — the sections that save the most rediscovery time.
- **Keep it honest.** Remove sections that don't apply rather than leaving placeholders. An incomplete but accurate document is more useful than a comprehensive but stale one.
- **Evolve it.** Add Development Rules when you find new bug classes during QA. Update Architecture when subsystems change. The Maintaining This File section has guidance on when to update what.
- **Offload detail.** Move large sections (testing checklists, changelogs) to `docs/` and link from the main file to keep `CLAUDE.md` scannable.

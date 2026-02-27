# CLAUDE.md - [Project Name] Project Context

## Project Overview

**[Project Name]** is [one-sentence description of what this project does, its platform, and primary function].

**Current Version:** X.Y.Z
**Status:** [In development | Alpha | Beta | Production-ready]

---

## Hardware

<!-- Remove this section entirely if this is a software-only project -->

### Microcontroller
- **[MCU name and variant]**
- [Architecture and clock speed]
- [Connectivity (BLE, WiFi, etc.)]
- [Flash and RAM]
- [Power features]

### Components
| Ref | Component | Purpose |
|-----|-----------|---------|
| U1 | [MCU] | [Purpose] |
| ... | ... | ... |

### Pin Assignments
| Pin | Function | Notes |
|-----|----------|-------|
| ... | ... | ... |

---

## Architecture

### Core Files
[Brief description of the architecture style — modular, monolithic, microservices, etc.]

- `[entry_point]` - Entry point: [brief description]
- `[config_file]` - Configuration constants, enums, structs
- `[state_file]` - Mutable state / global state management
- `[settings_file]` - Persistent storage + accessors
<!-- Add one line per core module -->

### Dependencies
- [Dependency 1]
- [Dependency 2]
- [Dependency 3]

### Key Subsystems

<!-- Document each major subsystem. Include enough detail that an AI assistant
     can understand the design intent, constraints, and gotchas without reading
     every source file. Focus on: what it does, how it works, and what NOT to do. -->

#### 1. [Subsystem Name]
- [How it works at a high level]
- [Key design decisions and why]
- [Important constraints or limitations]

#### 2. [Subsystem Name]
- ...

#### 3. Settings / Configuration Storage
<!-- If your project has persistent settings, document the struct and storage mechanism -->
```
[Settings struct or schema here]
```
- Saved to [storage location] via [storage mechanism]
- Default values: [list key defaults]

#### 4. Communication Protocol
<!-- If your project has a config/command protocol, document it here -->
```
[Protocol format examples]
```
- [Transport details]
- [Protocol conventions: prefixes, delimiters, response format]

---

## Build Configuration

<!-- Document non-obvious build tool decisions, loader/plugin configs, and the reasoning
     behind them. "Build Instructions" covers how to run builds; this section covers
     why the build is configured the way it is. -->

### [Build tool / bundler name] Configuration
- **[Config option]** — [what it does and why it's needed]
- **[Config option]** — [what it does and why it's needed]
<!-- Example: runtimeCompiler: true — required for library X which uses string templates -->
<!-- Example: linker script uses custom FLASH origin — board has non-standard memory map -->

### Environment Variables

<!-- Document the env var layering strategy and key variables that control build behavior.
     For embedded: this may be build defines, board variant flags, or preprocessor macros. -->

| Variable | Purpose | Values |
|----------|---------|--------|
| `[VAR_NAME]` | [What it controls] | [Possible values or default] |
| ... | ... | ... |

Environment files / define sources:
- `[file1]` — [which build mode / board variant]
- `[file2]` — [which build mode / board variant]

---

## Code Style

<!-- Document linting, formatting, and style conventions that a contributor needs to match.
     Include tool configs, key rule overrides, and any non-obvious style decisions. -->

- **Linter:** [tool and config] (e.g., ESLint + Airbnb, clang-tidy, clippy)
- **Formatter:** [tool] (e.g., Prettier, clang-format, rustfmt)
- **Key rule overrides:** [list any rules turned off or customized, and why]
- **Indentation:** [spaces/tabs and size]
- **Line length:** [max chars]
- **Line endings:** [LF/CRLF]

---

## External Integrations

<!-- Document third-party services, SDKs, or external dependencies where the integration
     involves configuration, lifecycle management, or hostname/environment gating.
     Distinct from Key Subsystems (code you wrote) — these are services you consume. -->
<!-- For embedded: HAL/SDK versions, cloud services, OTA providers, etc. -->

### [Service Name]
- **What:** [brief description of the service]
- **Loaded via:** [script URL, SDK import, env var, etc.]
- **Lifecycle:** [how it starts, shuts down, restarts — if applicable]
- **Environment/hostname gating:** [which environments or hostnames activate it]
- **Key env vars:** [relevant environment variables]
- **Gotchas:** [non-obvious behavior, ordering dependencies, etc.]

---

## Known Issues / Limitations

1. **[Issue]** - [Brief explanation]
2. **[Issue]** - [Brief explanation]
<!-- Keep this list current. Remove items when fixed, add new ones as discovered. -->

---

## Development Rules

<!-- These rules exist to prevent classes of bugs found during QA.
     Follow them for all new code and modifications.
     Add new rules as new bug classes are discovered. -->

### 1. Validate all external input at the boundary
Every value arriving from an external source (API, serial, BLE, user input) must be validated and clamped to valid bounds before being stored or used. Never assign an externally-supplied value without bounds checking.

### 2. Guard all array-indexed lookups
Any value used as an index into an array must have a bounds check before access: `(val < COUNT) ? ARRAY[val] : fallback`. This is defense-in-depth against corrupt or unvalidated values.

### 3. Reset connection-scoped state on disconnect
Buffers, flags, and session variables that accumulate state during a connection must be reset on disconnect to prevent cross-session corruption.

### 4. Avoid memory-fragmenting patterns in long-running code
<!-- Adapt to your platform: heap fragmentation on embedded, memory leaks in Node, etc. -->
In hot paths or long-lived processes, prefer stack-allocated buffers and fixed-size arrays over dynamic allocation. Reserve dynamic allocation for short-lived, one-shot operations.

### 5. Use symbolic constants, not magic numbers
Never hardcode index values or numeric constants — use named defines or enums. When data structures are reordered, update both the data and all symbolic references together.

### 6. Throttle event-driven output
Any function that sends data in response to frequent events must implement rate limiting or throttling to prevent saturation of output channels.

### 7. Use bounded string formatting
Always use `snprintf(buf, sizeof(buf), ...)` (or language equivalent) instead of unbounded formatting. This prevents silent overflow if format arguments change in the future.

### 8. Report errors, don't silently fail
When input exceeds limits or operations fail, provide actionable error feedback to the caller. Never silently truncate, drop, or ignore errors.

<!-- Add project-specific rules below as bugs are discovered during QA.
     Each rule should reference the class of bug it prevents. -->

---

## Common Modifications

<!-- Document step-by-step recipes for common changes.
     These save time and prevent mistakes when making routine modifications.
     Keep each recipe as a numbered checklist. -->

### Version bumps
Version string appears in N files:
<!-- List every file that contains the version string -->
1. `[file1]` - [where in the file]
2. `[file2]` - [where in the file]
3. ...

**Keep all version references in sync.** Always bump all files together during any version bump.

### [Common Modification 1: e.g., "Add a new API endpoint"]
1. [Step 1]
2. [Step 2]
3. ...

### [Common Modification 2: e.g., "Add a new config setting"]
1. [Step 1 — define the setting]
2. [Step 2 — set the default]
3. [Step 3 — add validation (use bounded checks)]
4. [Step 4 — add to UI / protocol if applicable]
5. [Step 5 — update any auto-adapting mechanisms like checksums]
<!-- Reference the Development Rules where applicable -->

---

## File Inventory

| File / Directory | Purpose |
|------------------|---------|
| `[entry_point]` | [Purpose] |
| `[config]` | [Purpose] |
| ... | ... |
| `CLAUDE.md` | This file |
| `docs/` | [What's in docs] |

---

## Build Instructions

### Prerequisites
- [Tool 1 and version]
- [Tool 2 and version]

### Quick Start
```bash
[setup command]    # Install dependencies
[build command]    # Build the project
[run command]      # Run / flash / deploy
```

### Troubleshooting Build
- **"[common error message]"** - [fix]
- **"[common error message]"** - [fix]

---

## Testing

<!-- Link to or describe the testing approach.
     For large checklists, put them in a separate file and link here. -->

See `[path/to/testing-checklist]` for the full QA testing checklist.

---

## Future Improvements

<!-- Track ideas separately from active work.
     For long lists, put them in a separate file and link here. -->

See `[path/to/future-improvements]` for the ideas backlog.

---

## Maintaining This File

<!-- Instructions for keeping CLAUDE.md accurate over time -->

### When to update CLAUDE.md
- **Adding a new subsystem or module** — add it to Architecture and File Inventory
- **Adding a new setting or config field** — update the Settings section and Common Modifications
- **Discovering a new bug class** — add a Development Rule to prevent recurrence
- **Changing the build process** — update Build Instructions and/or Build Configuration
- **Adding/changing env vars or build defines** — update Build Configuration > Environment Variables
- **Changing linting or style rules** — update Code Style
- **Integrating a new third-party service or SDK** — add to External Integrations
- **Bumping the version** — update the version in Project Overview
- **Adding/removing files** — update File Inventory
- **Finding a new limitation** — add to Known Issues

### Supplementary docs
For sections that grow large (display layouts, testing checklists, changelogs), move them to separate files under `docs/` and link from here. This keeps the main CLAUDE.md scannable while preserving detail.

### Future improvements tracking
When a new feature is added and related enhancements or follow-up ideas are suggested but declined, add them as `- [ ]` items to `docs/future-improvements.md`. This preserves good ideas for later without cluttering the current task.

### Testing checklist maintenance
When adding or modifying user-facing behavior (new settings, UI modes, protocol commands, or display changes), add corresponding `- [ ]` test items to `docs/testing-checklist.md`. Each item should describe the expected observable behavior, not the implementation detail.

### What belongs here vs. in code comments
- **Here:** Architecture decisions, cross-cutting concerns, "how things fit together," gotchas, recipes
- **In code:** Implementation details, function-level docs, inline explanations of tricky logic

---

## Origin

Created with Claude (Anthropic)

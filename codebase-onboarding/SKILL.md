---
name: codebase-onboarding
description: Analyze an unfamiliar repository and produce an evidence-backed onboarding guide, architecture map, key entry points, conventions, and portable agent instructions. Use when joining a project, learning how a codebase works, or preparing repository guidance for coding agents.
metadata:
  origin: ECC
---

# Codebase onboarding

Build a concise, evidence-backed map of an unfamiliar repository. Write for developers and coding agents without assuming a particular agent product or runtime.

Match the deliverables to the request:

- For "onboard me" or "help me understand this repository," return an onboarding guide.
- For repository agent instructions, create or update the file the user names.
- For a general setup request, return the guide and create portable repository instructions as described below.

## 1. Reconnaissance

Gather broad signals before reading implementation details. Run independent checks in parallel when the environment supports it.

Inspect:

- repository instruction files such as `AGENTS.md`, `CLAUDE.md`, `GEMINI.md`, and `.github/copilot-instructions.md`
- package and language manifests
- framework, build, deployment, and CI configuration
- executable entry points and public interfaces
- the top two useful directory levels, excluding generated and dependency directories
- test layout and test-runner configuration
- the README and architecture or decision records
- recent version-control history when available

Use fast file listing and text search tools available in the current environment. Read representative files selectively. Reconnaissance is complete when each relevant category above is either identified or recorded as unavailable.

## 2. Map the architecture

Use code and configuration as evidence. Identify:

- languages, version constraints, frameworks, major libraries, data stores, build tools, and CI
- repository shape, such as a single application, monorepo, service collection, package, or infrastructure project
- runtime boundaries and how the main components connect
- key directories and entry points
- one representative execution path from an external entry point through validation and business logic to persistence or output

Adapt the traced path to the project. A request lifecycle suits a web service; a command invocation, job, event, build, or library call may be the right path elsewhere. Cite concrete repository paths for every architectural claim.

## 3. Detect conventions

Infer conventions only from repeated evidence. Check:

- file and symbol naming
- module boundaries and dependency direction
- error handling, validation, state management, and asynchronous work
- test naming, placement, fixtures, and commands
- formatting, linting, type-checking, build, and development commands
- commit and branch patterns when version-control history is deep enough

Treat scripts and configuration as the source of truth for commands. If evidence conflicts, state the conflict. If a convention cannot be established, mark it unknown rather than guessing.

## 4. Produce the requested artifacts

### Onboarding guide

Keep the guide scannable and grounded in paths. Use only sections that help with this repository:

```markdown
# Onboarding guide: [project]

## Overview
[What the project does, who or what uses it, and its repository shape]

## Technology
| Concern | Choice | Evidence |
| --- | --- | --- |
| [language/runtime/etc.] | [detected value] | `[path]` |

## Architecture
[Small diagram or compact description of component relationships]

## Key entry points
- `[path]`: [role]

## Directory map
| Path | Purpose |
| --- | --- |
| `[path]` | [non-obvious responsibility] |

## Representative flow
[One traced request, command, event, job, build, or library call]

## Conventions
- [Observed pattern and evidence]

## Common tasks
- [Task]: `[command from project configuration]`

## Where to make changes
| Goal | Start here |
| --- | --- |
| [common change] | `[path]` |

## Unknowns
- [Question the repository did not answer]
```

Put the guide in the conversation unless the user asks for a file or the surrounding workflow establishes a documentation location.

### Repository agent instructions

Use the filename requested by the user. Otherwise:

1. Discover instruction files already used by the repository and current runtime.
2. Update the existing root-level shared file when one clearly serves as the canonical source.
3. Preserve product-specific files and their syntax when the repository intentionally uses them.
4. If no convention exists and the request calls for a new shared file, create `AGENTS.md` as the portable default.

Read an existing file before editing it. Preserve project-specific guidance, remove only claims proven stale, and report material changes. Do not copy the onboarding guide into the instruction file.

Keep agent instructions focused on facts that change how work should be done:

```markdown
# Repository instructions

## Project shape
[Short map of the runtime boundaries and important directories]

## Build and verification
- Development: `[detected command]`
- Tests: `[detected command]`
- Lint or format: `[detected command]`
- Build or type-check: `[detected command]`

## Code conventions
- [Observed local rule]

## Change guidance
- [Where common changes belong and any non-obvious constraint]
```

Omit commands or rules that are easy to discover from one obvious manifest unless agents routinely need them or the lookup is ambiguous. Keep each instruction in one authoritative file. When multiple product-specific instruction files must coexist, put shared guidance in one file and use short pointers where the runtimes support them.

## Completion check

Before finishing, verify that:

- architectural claims and commands point to repository evidence
- the representative flow reaches a real outcome
- detected conventions come from repeated examples or explicit configuration
- generated instructions preserve existing user guidance and contain no runtime-specific assumptions unless the repository or user requires them
- unknowns are labeled
- the result adds structural insight rather than restating the README or listing every dependency

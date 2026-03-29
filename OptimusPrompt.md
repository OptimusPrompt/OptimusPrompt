# Optimus Prompt

## Rules

### 1. Session Start

At the start of every session, read these things. They replace exploratory commands.

1. `SPEC.md` if it exists. It has project context, requirements, config, and dependencies.
2. All entries in `devlog/`. Read every entry to fully understand the project history, past decisions, and lessons learned.

If `SPEC.md` doesn't exist, read the codebase and create it using the template below. Create the `devlog/` folder at the same time.

### 2. Classify Before Acting

| Type | Behavior |
|------|----------|
| **Lookup** (find a file, check a value, list something) | Answer immediately. Keep it short. |
| **Implementation** (write code, fix a bug, refactor) | Read only the files you'll change. Return the targeted diff. |
| **Architecture** (design, debug something subtle, multi-file) | Think step by step. Consider tradeoffs. Propose before acting. |

### 3. Don't Explore, Locate

The project context in `SPEC.md` is authoritative. Don't run exploratory commands to learn things already documented there. If something is missing, ask.

Locate before loading. Find the right file and line number before opening anything. Read only the range you need. If a devlog entry already described a file's contents, use that instead of re-reading.

### 4. Return Less

Format all output as concise structured blocks:

```
Files: [paths + line numbers]
Result: [pass/fail, count, or status]
Detail: [only what's actionable: errors, failures, blockers]
```

Don't paste raw command output, full file contents, or complete diffs unless asked.

### 5. Hand Off with Context

When passing work to another agent, prompt, or session:

```
## Handoff
State: [what was done, what files were changed]
Decisions: [choices made and why]
Open: [what's unfinished or failing]
Next: [specific task for the recipient]
Files: [only the paths they'll need]
```

Without a handoff block, the next agent wastes its entire first pass re-exploring the project.

---

## Spec File

Maintain a `SPEC.md` at the project root. The canonical reference for what the project is and how it's configured. On first run, the agent creates this by reading the codebase.

```markdown
# SPEC.md

## Project
**Name**:
**Description**:
**Stack**:
**Entry**:
**Tests**:

## Structure
[Map the project directory here]

## Key Decisions
[Document architectural decisions here]

## Requirements
- [Functional requirement 1]
- [Functional requirement 2]

## Configuration
| Setting | Value | Location |
|---------|-------|----------|
| [e.g. API port] | [8080] | [config.json] |

## Dependencies
| Package | Version | Purpose |
|---------|---------|---------|
| [e.g. fastapi] | [0.104+] | [REST framework] |

## Environments
- **Dev**: [how to run locally]
- **Test**: [how tests are configured]
- **Prod**: [deployment method, hosting]

## API / Interface Contract
[Endpoints, CLI commands, or UI routes the project exposes]

## Constraints
- [e.g. Must support Python 3.12+]
- [e.g. All responses under 200ms]
```

**Rules:** After each change, check: did this affect any requirement, config, dependency, API contract, or constraint? If yes, update `SPEC.md` in the same commit. If no, skip. Don't duplicate what's in Optimus Prompt. That file holds behavioral rules, the spec holds project facts.

---

## Dev Log

Maintain a `devlog/` folder with numbered entries (`devlog-001.md`, `devlog-002.md`, ...). Before each commit, write a log entry. This is the project's persistent memory. It keeps future agents from duplicating effort or retrying failed approaches.

### Entry format

```markdown
# devlog-[NNN]

**Date**: [YYYY-MM-DD]
**Author**: [human or agent name/model]
**Commit**: [short hash, filled after commit]

## What Changed
- [File or feature]: [what was added/modified/removed]

## Why
[The reason for the change]

## How
[Brief technical approach, not a diff, just the strategy]

## Learned
- [e.g. "Tried approach X first, it failed because Y. Switched to Z"]

## Open Questions
- [Anything unresolved for the next session]
```

**Rules:** Write the entry before committing. Include it in the same commit. Number sequentially. The "Learned" section is mandatory. Keep entries 10-30 lines. Max 500 lines per file for fast reference.

**Skipping:** If the user says "skip devlog", skip the entry for that commit.

# Optimus Prompt

**One markdown file that makes any AI coding agent faster by killing re-discovery.**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](https://makeapullrequest.com)

---

## The Problem

Every time an AI agent starts a session, it explores your project from scratch. It reads files to learn the structure, runs commands to discover the tech stack, and re-derives decisions you already made. This burns tokens, wastes time, and produces worse results because the agent is guessing instead of knowing.

Multi-agent workflows are worse. Agent B re-reads everything Agent A already processed. No context carries over. No lessons persist. The same failed approaches get retried across sessions.

## The Solution

`OptimusPrompt.md` is a single markdown file you paste at the start of any chat. It tells the agent to:

1. **Read the spec and devlog first** instead of exploring from scratch.
2. **Follow behavioral rules**: classify tasks, locate before loading, return structured output, hand off with context.
3. **Maintain persistent memory**: keep `SPEC.md` current and write a `devlog/` entry before each commit.

The agent creates `SPEC.md` and `devlog/` on first run by reading your codebase. You review and correct. Works with any LLM. No server, no dependencies, no infrastructure.

---

## The Three-File System

| File | Purpose | Updated |
|------|---------|---------|
| `OptimusPrompt.md` | Behavioral rules and directions | Rarely, only when rules change |
| `SPEC.md` | Living spec: requirements, config, deps, API contracts | When project facts change |
| `devlog/devlog-NNN.md` | Numbered log entries: what changed, why, what was learned | Before each commit |

**Optimus Prompt** tells the agent how to behave. **SPEC** tells it what the project is. **Devlog** tells it what happened.

Together they eliminate re-discovery and give agents persistent memory across sessions.

---

## Quick Start

Paste the contents of `OptimusPrompt.md` at the start of any chat. The agent will read `SPEC.md` and all devlog entries before doing anything. If they don't exist yet, it reads your codebase and creates them. You review and correct. Done.

---

## What's Inside

### 5 Rules

| Rule | What It Does |
|------|-------------|
| **Session Start** | Read SPEC.md and all devlog entries before doing anything |
| **Classify Before Acting** | Match effort to task type. Lookups get short answers, architecture gets step-by-step |
| **Don't Explore, Locate** | Use documented context. Search for the right line before opening files |
| **Return Less** | Structured output blocks, not raw dumps |
| **Hand Off with Context** | Pass state, decisions, and next steps between agents. Never just "finish this" |

### SPEC.md Template

Project spec covering: project details, structure, key decisions, requirements, configuration, dependencies, environments, API contracts, and constraints. The agent creates it on first run and updates it when project facts change.

### Devlog Convention

Numbered entries (`devlog-001.md`, `devlog-002.md`, ...) written before each commit with five sections:

- **What Changed**: files and features touched
- **Why**: the reason for the change
- **How**: technical approach
- **Learned**: mandatory section for gotchas, failed attempts, new understanding
- **Open Questions**: unresolved items for the next session

Say "skip devlog" for trivial changes.

---

## What This Achieves

Estimated **20-40% token reduction** per session depending on project complexity and session length. These are not benchmarked numbers. They're estimates based on comparing the cost of reading pre-loaded context vs. the cost of an agent exploring a project from scratch through repeated file reads, command execution, and output parsing.

| Feature | What It Does | Estimated Savings |
|---------|-------------|-------------------|
| Pre-loaded project context | Agent reads structured context instead of running exploratory commands | ~40-60% of session start |
| SPEC.md | One file replaces exploring config, deps, and env files | ~30-50% of project discovery |
| Devlog | Agent reads all entries instead of re-exploring or retrying failed approaches | ~30-50% across sessions |
| Structured handoffs | Next agent gets a short block instead of starting from scratch | ~40-60% of agent-to-agent overhead |
| Return less rule | Structured summaries instead of raw output dumps | ~20-40% of output tokens |

Actual savings vary. The rules are advisory, not enforced. An agent can ignore them. Savings compound across multi-session workflows where the devlog prevents the most repeated work.

Works with any LLM that reads markdown. Zero vendor lock-in. Zero dependencies, zero infrastructure, zero cost.

## What This Can't Do

- Embedding-based semantic search (needs a vector store)
- Enforced model routing (advisory only, the agent can choose to ignore it)
- Inter-agent communication within a session (no message bus)
- Enforced capability restrictions (markdown can't prevent actions)

The instruction-only approach covers the highest-value efficiency gains. The features it can't replicate require a running service.

---

## Contributing

1. Fork the repository
2. Make your changes
3. Submit a PR

Improvements to the rules, templates, or new tool placement guides are all welcome.

## License

MIT

# RottenContext

A curated collection of **patterns and tools** for managing context rot in AI-assisted coding.

[rottencontext.com](https://rottencontext.com) — web version with better navigation

Context rot is the practical failure mode of long, mixed-purpose AI coding sessions: contradictions, forgotten constraints, vague plans, and repeated re-explanations as the working context becomes polluted.

## Table of Contents

- [Operating Model](#operating-model)
- [Patterns](#patterns)
- [Antipatterns](#antipatterns)
- [Tools](#tools)
- [Research](#research)
- [Contributing](#contributing)
- [License](#license)

## Operating Model

Most effective approaches map to four levers:

| Lever | What it does |
|-------|--------------|
| **Isolation** | Bound each session to a single task/phase/agent to prevent context pollution |
| **Snapshot & Rehydrate** | Record authoritative project state in files; resume from those instead of chat history |
| **Packing** | Provide only relevant repository slices, prepared for efficient AI consumption |
| **Compaction** | Summarize or prune context intentionally before quality degrades |

### Quantitative Guidance

Research-backed thresholds for context management:

| Metric | Recommendation | Rationale |
|--------|----------------|-----------|
| Effective context window | <256k tokens | Models degrade well before advertised limits (1M+) |
| Context utilization | Stop at ~75% | Sessions at 90% produce more code but lower quality |
| Compaction trigger | ~128k tokens or 50% utilization | Compact before degradation, not after |
| Memory/rules file size | <300 lines | Context tokens are precious; shorter is better |

## Patterns

### STATE.md as source of truth

Maintain a `STATE.md` file in your repository with:

- **Objective** — what "done" means
- **Constraints** — non-negotiables
- **Decisions** — selected approach and rationale
- **Current status** — where things stand
- **Next steps** — 3-7 concrete items

When outputs begin to drift, update `STATE.md` and start a fresh session using only that file plus relevant source code.

```md
# STATE

## Objective
- ...

## Constraints
- ...

## Decisions
- ...

## Current Status
- ...

## Next Steps
1. ...
2. ...
3. ...
```

### Task-scoped sessions

One task per session. When you finish a task or pivot to something different, end the session and start fresh. Mixing unrelated work in a single context accelerates rot.

### Phase separation

Split work into distinct phases (research, planning, implementation, review) and run each in its own session. Don't let implementation details pollute research context, or planning debates pollute implementation.

### Fresh-start rehydration

When context feels degraded, don't try to recover it. Instead:

1. Update your state file with current knowledge
2. Start a new session
3. Load only: state file + relevant source files (or a packed digest)

This is cheaper and more reliable than trying to "fix" a polluted context.

### Controlled repository slicing

Never dump an entire repository into context. Use packing tools to provide:

- Only files relevant to the current task
- Summaries instead of full content where appropriate
- Token-aware truncation

## Antipatterns

### Dumping entire repositories

Loading a full repo into context wastes tokens on irrelevant code and drowns the signal in noise. The model will hallucinate connections between unrelated files and lose track of what matters.

### Mixing planning and execution

Debating architecture while also writing code creates a context where half-formed plans mix with implementation details. Neither activity gets clean focus. Separate them into distinct sessions.

### Chat history as project memory

Relying on earlier messages to remember decisions, constraints, or status. Chat history degrades, gets summarized lossy, and eventually falls out of context entirely. Externalize to files.

### Pushing through drift

When outputs start missing constraints or repeating mistakes, continuing in the same session makes it worse. The context is polluted. Stop, snapshot, restart fresh.

### Over-trusting summaries

Automatic summaries lose detail. When you resume from a summary, verify critical constraints and decisions are actually present. Don't assume the summary captured everything that matters.

### Overloading a single agent

Asking one agent/session to handle research, planning, implementation, and review. Each activity pollutes context for the others. Use separate agents or sessions for separate concerns.

## Tools

Each tool is included because it exposes a clear mechanism for one of the four levers.

### Orchestration and Phase Separation

Tools that enforce phased execution and externalize state.

| Tool | Mechanism | Best for |
|------|-----------|----------|
| [Get Shit Done (GSD)](https://github.com/glittercowboy/get-shit-done) | Spec-driven phases with persistent state files | Multi-phase feature delivery |
| [Ralph](https://github.com/frankbria/ralph-claude-code) | Autonomous loop runner with explicit completion gates | Long-running iteration with guardrails |
| [Continuous Claude](https://github.com/parcadei/Continuous-Claude-v3) | Hooks + ledger/handoff patterns | Structured CLI workflows with state preservation |
| [Claude Code Workflow](https://github.com/catlog22/Claude-Code-Workflow) | JSON-driven multi-step structure with explicit phase boundaries | Repeatable, bounded agent execution |

### Parallel Work Without Context Bleeding

Tools for running multiple workstreams while keeping contexts isolated.

| Tool | Mechanism | Best for |
|------|-----------|----------|
| [workmux](https://github.com/raine/workmux) | Git worktrees + tmux to isolate at filesystem/session level | Parallel development without context mixing |
| [Claude Squad](https://github.com/smtg-ai/claude-squad) | Multiple agents in separate terminal workspaces | Running parallel agent workstreams |
| [Vibe Kanban](https://github.com/BloopAI/vibe-kanban) | Multi-agent orchestration with task tracking | Managing multiple concurrent tasks/agents |

### Repository Packing

Tools that produce controlled, prompt-friendly repository context.

| Tool | Mechanism | Best for |
|------|-----------|----------|
| [Repomix](https://github.com/yamadashy/repomix) | Repo to AI-friendly artifact with configurable packing | Sharing controlled repo context |
| [CTX](https://github.com/context-hub/generator) | Structured context docs based on explicit selection rules | Deterministic, high-signal context packets |
| [Gitingest](https://github.com/coderamp-labs/gitingest) | Prompt-ready repository digest | Fast repo rehydration |

### Memory Persistence

Tools that persist decisions and state across sessions.

| Tool | Mechanism | Best for |
|------|-----------|----------|
| [memory-bank-mcp](https://github.com/alioshr/memory-bank-mcp) | Remote memory bank service via MCP | Cross-session project memory |
| [Context Portal](https://github.com/GreatScottyMac/context-portal) | Structured, DB-backed project context via MCP | Durable source-of-truth memory |
| [Cursor Memory Bank](https://github.com/vanzan01/cursor-memory-bank) | Structured memory workflow with rules loading | Reducing repeated explanations in Cursor |
| [Cursor Rules](https://cursor.com/docs/context/rules) | Persistent, versioned instructions per repository/path | Project-level AI configuration |

### Compaction and Pruning

Tools and features for compacting long sessions before degradation.

| Tool | Mechanism | Best for |
|------|-----------|----------|
| [OpenCode](https://github.com/opencode-ai/opencode) | Built-in compaction/pruning workflows | Long terminal sessions |
| [Claude Code](https://docs.anthropic.com/en/docs/claude-code) | `/compact` command and session management | CLI-based context hygiene |

## Research

Foundational reading on context management in AI-assisted development.

- **Anthropic: Effective Context Engineering for Agents**
  https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents

- **LangChain: Context Engineering for Agents**
  https://blog.langchain.com/context-engineering-for-agents

- **Chroma: Context Rot** (research study)
  https://research.trychroma.com/context-rot
  https://github.com/chroma-core/context-rot

- **Addy Osmani: Conductors to Orchestrators**
  https://addyosmani.com/blog/future-agentic-coding/

- **arXiv: Context Engineering for Multi-Agent LLM Code Assistants**
  https://arxiv.org/html/2508.08322v1

- **Phil Schmid: Context Engineering for AI Agents (Part 2)**
  https://www.philschmid.de/context-engineering-part-2

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

[CC0](LICENSE) — public domain.

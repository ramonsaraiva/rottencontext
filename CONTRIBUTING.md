# Contributing

Contributions are welcome. This list is intentionally selective—every entry must address context rot through a concrete mechanism.

## Selection Criteria

A tool or pattern should be included only if it clearly demonstrates at least one of these mechanisms:

| Mechanism | Description |
|-----------|-------------|
| **Isolation** | Task/phase/workspace separation that prevents context pollution |
| **Snapshot & Rehydrate** | Durable state artifacts intended for clean restarts |
| **Packing** | Controlled, token-aware repository context preparation |
| **Compaction** | Explicit summarization/pruning to slow degradation |
| **Persistent Memory** | Cross-session state that replaces reliance on chat history |

## Adding a Tool

When submitting a tool, include:

1. **Name and link** — tool name linking to repository or documentation
2. **Mechanism** — one sentence describing *how* it reduces context rot
3. **Best for** — one sentence describing when to use it
4. **Category** — which section it belongs in (Orchestration, Parallel Work, Packing, Memory, Compaction)

Example:

```md
| [Tool Name](https://github.com/org/repo) | Mechanism description | Best use case |
```

## Adding a Pattern or Antipattern

Patterns and antipatterns should be:

- **Actionable** — clear enough that someone can follow or avoid them
- **Specific** — tied to a concrete context rot failure mode
- **Tool-agnostic** — works regardless of which AI assistant or IDE is used

Include a brief explanation of *why* the pattern helps or *why* the antipattern causes problems.

## What Not to Submit

- Generic "AI productivity" tools without explicit context management features
- Tools where the context-rot mechanism is speculative or unclear
- Duplicates of existing entries without meaningful differentiation

## Process

1. Open a pull request with your addition
2. Ensure the entry meets selection criteria
3. Place the entry in the appropriate section

Keep the list lean. Quality over quantity.

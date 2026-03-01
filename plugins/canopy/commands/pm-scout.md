---
description: Run a PM scout cycle on the current project — explore, propose improvements, learn
argument-hint: [lens]
allowed-tools: [Read, Glob, Grep, Bash, Agent, Write, Edit]
---

# PM Scout

Run a product management scout cycle on the current project.

## Arguments

- `lens` (optional): The exploration lens to use. One of: user-value, adoption-blockers, integration-depth, trust-reliability, tech-debt. If not specified, rotate from the last lens used (check `.claude/pm/runs/` for the most recent run).

## Process

1. Invoke the `pm-supervisor` skill
2. Execute Phase 1 (Scout) using the specified lens
3. Present findings in the Phase 2 (Propose) format
4. Wait for user disposition on each proposal
5. Execute Phase 6 (Learn) — write run log and update learnings
6. Evaluate for universal improvements to the skill itself

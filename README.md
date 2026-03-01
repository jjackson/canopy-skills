# Canopy Skills

Personal Claude Code plugin marketplace with self-improving AI workflow skills.

## What This Is

A collection of skills for Claude Code that get better over time. Skills accumulate project-level knowledge automatically and propose universal improvements back to this repo via PRs.

## Install

```bash
claude plugin marketplace add jjackson/canopy-skills
claude plugin install canopy@canopy-skills
```

## Skills

### pm-supervisor

Autonomous product development cycles: explore a codebase, propose improvements, implement approved changes, and learn from outcomes.

- **Skill**: `pm-supervisor` — automatically invoked for PM-style development
- **Command**: `/pm-scout [lens]` — run a scout cycle with a specific exploration lens
- **Agent**: `pm-supervisor` — for open claws and automated supervisors

#### Exploration Lenses

- `user-value` — features that make users love the product
- `adoption-blockers` — friction that makes people stop using it
- `integration-depth` — ecosystem connectivity
- `trust-reliability` — bugs, silent failures, missing error handling
- `tech-debt` — dead code, flaky tests, outdated deps

#### How It Learns

**Project-level** (automatic): Run logs and learnings accumulate in `.claude/pm/` within each project. Claude reads these before every run to avoid repeating mistakes.

**Skill-level** (via PR): When Claude identifies a universal improvement, it proposes a PR to this repo. You review and merge — all environments get the update.

## Adding More Skills

Add new skills under `plugins/canopy/skills/<skill-name>/SKILL.md`. Follow the pattern in `pm-supervisor` for state conventions and learning protocols.

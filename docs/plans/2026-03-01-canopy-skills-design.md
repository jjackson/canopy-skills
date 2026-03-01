# Canopy Skills — Design Document

**Date:** 2026-03-01
**Status:** Approved

## Problem

Skills need to be portable across laptops, open claw containers, and human collaborators — all running the same skill definitions. Some skills are self-improving: they accumulate project-level knowledge and surface universal improvements back to the skill definition. Today these skills live in project directories, tightly coupled to specific projects and machines.

## Solution

A personal Claude Code plugin marketplace (`jjackson/canopy-skills`) that:
1. Distributes skills to all environments via `claude plugin install`
2. Separates portable skill definitions from project-specific state
3. Uses two learning loops: automatic project-level learning + PR-based skill improvement

## Architecture

### Repository Structure

```
canopy-skills/
├── .claude-plugin/marketplace.json
├── plugins/
│   └── canopy/
│       ├── .claude-plugin/plugin.json
│       ├── skills/
│       │   └── pm-supervisor/
│       │       ├── SKILL.md
│       │       └── templates/
│       ├── commands/
│       │   └── pm-scout.md
│       └── agents/
│           └── pm-supervisor.md
└── docs/plans/
```

### State Convention

Per-project state lives in `.claude/pm/` within each project:

```
<project>/
└── .claude/
    └── pm/
        ├── context.md       ← project identity, bootstrapped on first run
        ├── learnings.md     ← "don't propose X again"
        └── runs/            ← cycle logs
```

This follows the existing `.claude/` convention: commit shareable knowledge, gitignore sensitive/local data.

### Two Learning Loops

1. **Project loop** (automatic): After each cycle, update `learnings.md` and write run log. This is project-scoped memory.
2. **Skill loop** (via PR): When a universal insight emerges, propose a PR to `canopy-skills` with the improvement. Reviewed before merging.

### Deployment

```bash
claude plugin marketplace add jjackson/canopy-skills
claude plugin install canopy@canopy-skills
```

Works on laptops, containers, and for any collaborator.

## Migration

Existing PM skill in `chrome-plugins/examples/` seeds this system:
- Process + templates + universal lessons → `canopy-skills` repo
- `codebases/*.md` → each project's `.claude/pm/context.md`
- `runs/` → each project's `.claude/pm/runs/`
- Hardcoded paths and multi-project references → removed, skill is project-agnostic

## Key Decisions

- **PRs for skill improvement, not direct self-editing**: prevents degradation, provides audit trail
- **`.claude/pm/` for project state**: follows existing convention, committable and shareable
- **One marketplace, one plugin, multiple skills**: simple to start, scales as more skills are added
- **Project-agnostic skills**: skill works on whatever project it's invoked in, no hardcoded references

---
name: doc-regeneration
description: Audit project documentation for staleness and coverage gaps, then regenerate CLAUDE.md and learnings. Use when docs may be out of sync with actual project state.
version: 0.1.0
---

# Doc Regeneration — Lean v1

## Purpose

Audit CLAUDE.md and docs/ against the actual project state (GitHub issues, PRs, learnings), then produce a corrected version. The goal is ensuring an AI agent starting a new wave has accurate, complete context — no stale status, no missing learnings, no confusing contradictions.

## Modes

**Dry-run (default):** Generate all output into `docs/.dry-run/`. Nothing is committed or branched. Review the output and decide what to apply.

**Apply:** Create a `docs/regen-YYYY-MM-DD` branch, write the regenerated files, and create a PR.

To select mode, the invoker specifies `--dry-run` or `--apply` when calling the skill. Default is `--dry-run`.

## Process

### Phase 1: Read Everything (no output yet)

Read these inputs. Do NOT produce any output until all inputs are gathered:

1. **CLAUDE.md** — read the full file
2. **docs/learnings/** — read every file, note each learning's key takeaway
3. **docs/plans/** — read headers and status sections only (skip large plan bodies like Phase 0's 2000+ lines — just read the first 50 lines for context)
4. **GitHub issues** — run `gh issue list --state all --limit 50` to get current state
5. **GitHub PRs** — run `gh pr list --state all --limit 50` to get merged/open PRs

### Phase 2: Analyze (two checks)

**Check 1 — Staleness:**
Compare CLAUDE.md's status table against GitHub issue states. For each wave:
- Is the status in CLAUDE.md correct? (Open vs Done, issue number, file count)
- Does the PR link match?
- Are any completed waves still marked as Open?

**Check 2 — Coverage:**
For each learning in `docs/learnings/`:
- Is the learning's key takeaway reflected somewhere in CLAUDE.md? (Checklist item, guideline, key doc reference, etc.)
- If not, what section of CLAUDE.md should it be added to?

Also check:
- Are there patterns from merged PRs or closed issues that should be learnings but aren't?
- Are any docs/plans referenced in CLAUDE.md that don't exist or are obsolete?

### Phase 3: Produce Output

Generate these files:

**`review-report.md`** — The core deliverable. Contains:
1. **Staleness findings** — table of what's wrong in the status table, with corrections
2. **Coverage findings** — table of each learning and whether it's reflected in CLAUDE.md
3. **Opinionated assessment** — "If I were an agent starting Wave N today, here's what would confuse me and what I'd need." Be specific and direct.
4. **Recommended changes** — bullet list of what the regenerated CLAUDE.md changes

**`CLAUDE.md`** — The regenerated version. Rules:
- Preserve the existing structure and section order exactly
- Update the status table to match GitHub reality
- Add missing learning references to Key Docs or relevant sections
- Do NOT add new sections unless a learning explicitly calls for one
- Do NOT remove or rewrite content that is already correct
- Mark completed/obsolete plans appropriately in Key Docs

**`learnings/<name>.md`** (only if gaps found) — New learning docs for patterns discovered in PRs/issues that aren't captured yet. Use the existing learning format:
```
# Learning: <Title>

**Date**: YYYY-MM-DD
**Context**: <where this came from>
**Status**: <Resolved/Active>

## Problem
<what went wrong or was discovered>

## Root Cause
<why>

## Fix / Key Takeaway
<what to do differently>
```

### Phase 4: Deliver

**If dry-run (default):**
1. Create `docs/.dry-run/` directory
2. Write `docs/.dry-run/review-report.md`
3. Write `docs/.dry-run/CLAUDE.md`
4. Write any new learnings to `docs/.dry-run/learnings/`
5. Present the review report to the user
6. Suggest: "Review `docs/.dry-run/` and compare against current docs. When ready, re-run with `--apply` to create a PR."

**If apply:**
1. Create branch `docs/regen-YYYY-MM-DD`
2. Write regenerated CLAUDE.md to repo root
3. Write any new learnings to `docs/learnings/`
4. Commit with message `docs: regenerate documentation (staleness + coverage fixes)`
5. Create PR targeting `main`

## Key Principles

- **Read before write.** Gather ALL inputs before producing ANY output.
- **Preserve structure.** CLAUDE.md's section order is intentional. Don't reorganize.
- **Be opinionated.** The assessment should say what would confuse a new agent, not just list facts.
- **Minimal changes.** Only change what's wrong or missing. Don't rewrite correct content.
- **Evidence-based.** Every finding must cite the source (issue number, learning filename, PR number).

# Implement Prompt Template

Use this template when prompting Claude Code for the implementation phase.

```
Implement: {title}

Branch: {prefix}/{slug}

What to do:
{spec}

Acceptance criteria:
{validation}

Constraints:
- Follow existing patterns (check CLAUDE.md)
- Include tests for new behavior
- Don't modify unrelated code
- Keep the PR small and reviewable

Steps:
1. Create branch: git checkout -b {prefix}/{slug}
2. Implement the change
3. Write/update tests
4. Run: {test_command}
5. Run /simplify to clean up code reuse, quality, and efficiency issues
6. If validation fails, fix and re-run. If you can't fix after 2 attempts, report what's failing and stop.
7. Commit with descriptive message
8. Push and create PR
9. Report: what changed, test results, anything surprising
```

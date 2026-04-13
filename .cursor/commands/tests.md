---
description: Write or update unit tests for a feature based on the implementation
argument-hint: <ticket-number>
allowed-tools: Read, Write, Edit, Bash(git diff *, yarn test*, npm test*)
model: inherit
---

You are helping write or update unit tests for a recently implemented feature.

## Specs directory

Read the project config at: @.claude/specforge/config.json

If the file exists, use the value of the `specsDir` field as the base directory.
If the file does not exist, fall back to `.claude/specforge/` inside the current project.

The full path for this ticket is: <specs-dir>/$ARGUMENTS/

Read the spec at: <specs-dir>/$ARGUMENTS/spec.md
Read the plan at: <specs-dir>/$ARGUMENTS/plan.md

## 1. Get the diff

Run both commands in the repo root:

```bash
git diff origin/main..HEAD
git diff --cached
```

The first shows all committed changes on the current branch vs `main` (use a different base branch if needed, e.g. `develop`). The second shows staged but not yet committed changes. Combine both to get the full picture of what needs to be tested.

## 2. Discover the project's test conventions

Before writing any tests, examine existing test files in the project to understand its conventions:

- Find existing test files (look for files matching `*.test.*`, `*.spec.*`, or similar patterns).
- Note where test files live relative to source files (e.g. co-located, in a `__tests__` folder, in a top-level `tests/` directory, etc.).
- Note the file naming pattern used (e.g. `foo.test.ts`, `foo.spec.tsx`).
- Note the testing framework and style in use (e.g. `describe`/`it`, assertion style, mocking approach).

For each modified or new source file in the diff, find or create the corresponding test file following the same conventions observed above.

## 3. Add or update tests

- **New code paths / exports**: Add tests that cover the new behaviour (props, hooks, handlers, edge cases).
- **Changed logic**: Update or add tests so they reflect the new behaviour; fix or remove tests that asserted old behaviour.
- **Deleted code**: Remove or adjust tests that targeted the removed code.
- Follow the conventions discovered in step 2 — framework, assertion style, mocking approach, and file structure.
- **Coverage requirement**: Every line that was changed or added must have a unit test. Only leave lines or files untested when they genuinely cannot be tested (e.g. platform-specific code, third-party integration boundaries, or code that is not meaningfully testable in isolation).

## 4. Run tests and check coverage

After adding or updating tests:

1. Run the full suite to confirm nothing is broken: `yarn test` (or `npm test` if yarn is not used).
2. Run coverage: `yarn test:coverage` (or `npm run test:coverage`).

**Coverage target**: Coverage for the changed or added code (and for the affected files) should be between **90–100%**. If coverage falls below 90%, add or adjust tests until it meets this range. Only leave coverage below 90% for lines or files that cannot reasonably be tested — document the reason if so.

## After tests are written

1. Report a summary of which files were tested, what was added or updated, and the final coverage percentage
2. Flag any code that was intentionally left untested and explain why
3. Suggest the next step: "Ready to review? Run `/specforge:review $ARGUMENTS`"

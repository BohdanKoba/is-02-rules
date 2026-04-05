# PR checks (`/pr-checks`)

Invoke in chat with **`/pr-checks`** (filename `pr-checks.md` in `.cursor/commands/`).

Run the same checks contributors expect before opening a pull request in this monorepo.

## Steps

1. From the repository root, run `yarn test:all` (typecheck, ESLint, Prettier list-different, Vitest non-watch).
2. If the change touches the production app bundle, run `yarn build` and confirm it completes successfully.
3. Summarize any failures with file paths and the minimal fix; do not silence lint or tests without justification.

## Output

Report pass/fail per step and list any commands the user should re-run locally if something was skipped (e.g. long-running tests).

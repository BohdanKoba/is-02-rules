# AGENTS.md — Excalidraw monorepo

Context for AI coding agents and contributors working in this repository.

## Prerequisites

- **Node.js** ≥ 18 (`package.json` `engines`).
- **Yarn** 1.x (Classic), matching `packageManager` in root `package.json`.
- Clone the repo, then from the root run **`yarn install`** before `yarn start`, `yarn test`, or `yarn build`.

## Project overview

This repository is the **Excalidraw** open-source virtual whiteboard: a collaborative, hand-drawn-style canvas editor. It ships as **npm packages** under `@excalidraw/*` and a **Vite-based web app** (`excalidraw-app`) similar to [excalidraw.com](https://excalidraw.com). The codebase is mostly **TypeScript** and **React 19**.

## Architecture

- **Monorepo:** Yarn 1 workspaces (`package.json` → `workspaces`: `excalidraw-app`, `packages/*`, `examples/*`).
- **Packages:**
  - `packages/common` — shared utilities and types.
  - `packages/math` — geometry and math primitives.
  - `packages/element` — element model and operations.
  - `packages/excalidraw` — main editor UI, canvas, scene logic, and the public embeddable API.
- **Application:** `excalidraw-app` — production-oriented shell (PWA, collaboration hooks, build pipeline).
- **Examples:** `examples/*` — sample integrations (e.g. script-in-browser).

## Commands (run from repository root)

| Goal | Command |
|------|---------|
| Dev server | `yarn start` |
| Production build | `yarn build` |
| Unit / integration tests | `yarn test` or `yarn test:app` |
| Full CI-like suite | `yarn test:all` |
| Typecheck only | `yarn test:typecheck` |
| ESLint | `yarn test:code` |
| Prettier check | `yarn test:other` |
| Auto-fix lint | `yarn fix:code` |
| Auto-fix format | `yarn fix:other` |
| Build all packages | `yarn build:packages` |

Node **≥ 18** is required (`package.json` `engines`).

## Conventions

- **Imports:** Use workspace aliases such as `@excalidraw/common`, `@excalidraw/element`, `@excalidraw/math`, `@excalidraw/excalidraw` as configured in tooling (see `vitest.config.mts` and package exports).
- **Tests:** Vitest (`vitest.config.mts`, `setupTests.ts`). Tests are colocated under `packages/*/tests/` or as `*.test.tsx` files.
- **Lint / format:** ESLint with `@excalidraw/eslint-config`; Prettier with `@excalidraw/prettier-config`.
- **Security:** Do not commit secrets; treat imported documents and URLs as untrusted when rendering or executing logic.

## Cursor project rules

Persistent agent guidance lives in **`.cursor/rules/*.mdc`**. Each rule file includes a **How to verify** section with concrete commands or checks.

| Rule file | When it applies |
|-----------|-----------------|
| `monorepo-core.mdc` | Always (`alwaysApply`) |
| `security.mdc` | Always |
| `typescript-conventions.mdc` | `**/*.{ts,tsx}` |
| `testing.mdc` | `**/*.test.{ts,tsx}` |
| `excalidraw-package.mdc` | `packages/excalidraw/**/*.{ts,tsx}` |
| `excalidraw-app.mdc` | `excalidraw-app/**/*.{ts,tsx}` |

## Cursor slash commands

Custom commands are Markdown files under **`.cursor/commands/`**. In Cursor chat, invoke them by typing **`/`** and choosing the command — the name matches the **filename without `.md`**:

| Slash command | File | Purpose |
|---------------|------|--------|
| **`/pr-checks`** | `.cursor/commands/pr-checks.md` | Run pre-PR checks (`yarn test:all`, `yarn build` when relevant). |
| **`/editor-package`** | `.cursor/commands/editor-package.md` | Workflow for `packages/excalidraw` changes (tests + package build + smoke test). |

## A/B rule validation

Rule comparisons (enabled vs disabled) for at least one rule are documented in **`AB_VALIDATION.md`** at the repository root, including scenario, **both** outcomes, and a conclusion.

## Where to change what

- **Editor behavior / UI:** `packages/excalidraw` (and its `tests/` tree).
- **App shell / deploy / PWA:** `excalidraw-app`.
- **Shared primitives:** `packages/element`, `packages/math`, `packages/common`.

When unsure, start with the nearest existing feature and mirror its patterns for state, history, and tests.

## Verification checklist (acceptance)

Use this to confirm the repo meets workshop acceptance criteria:

1. **Rules:** At least six `.cursor/rules/*.mdc` files; each includes a **How to verify** section (open each file and search for that heading).
2. **Slash commands:** At least two files in `.cursor/commands/*.md` invokable as **`/pr-checks`** and **`/editor-package`** (type `/` in Cursor chat).
3. **A/B validation:** `AB_VALIDATION.md` documents scenario, result A, result B, and a conclusion showing a **difference** between conditions.
4. **This file:** `AGENTS.md` includes the sections above (prerequisites, overview, architecture, yarn commands, conventions, Cursor rules, slash commands, A/B pointer, where to change what, verification checklist).
5. **Build:** From repo root after `yarn install`, **`yarn build`** completes with exit code 0 and no errors.

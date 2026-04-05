# AGENTS.md — Excalidraw monorepo

Context for AI coding agents and contributors working in this repository.

## Prerequisites

- **Node.js** ≥ 18 (`package.json` `engines`).
- **Yarn** 1.x (Classic), matching `packageManager` in root `package.json`.
- Clone the repo, then from the root run **`yarn install`** before `yarn start`, `yarn test`, or `yarn build`.

## Project overview

This repository is the **Excalidraw** open-source virtual whiteboard: a collaborative, hand-drawn-style canvas editor. It ships as **npm packages** under `@excalidraw/*` and a **Vite-based web app** (`excalidraw-app`) similar to [excalidraw.com](https://excalidraw.com). The codebase is mostly **TypeScript** and **React 19**.

## Tech Stack

- **Languages:** TypeScript, JavaScript (tooling/scripts), HTML/CSS/SCSS where used.
- **UI:** React **19**, hand-drawn canvas editor in `packages/excalidraw`.
- **Runtime:** **Node.js ≥ 18** (see root `package.json` `engines`).
- **Build / dev:** **Vite** (app and package builds), **Yarn** 1.x workspaces.
- **Test:** **Vitest**, **jsdom**, `vitest.config.mts`, `setupTests.ts`.
- **Quality:** ESLint (`@excalidraw/eslint-config`), Prettier (`@excalidraw/prettier-config`).
- **Packages:** Scoped **`@excalidraw/*`** — `common`, `element`, `math`, `utils`, `excalidraw` (see `packages/*/package.json` and `vitest.config.mts` aliases).

## Architecture

- **Monorepo:** Yarn 1 workspaces (`package.json` → `workspaces`: `excalidraw-app`, `packages/*`, `examples/*`).
- **Packages:**
  - `packages/common` — shared utilities and types.
  - `packages/math` — geometry and math primitives.
  - `packages/utils` — shared helpers (published as `@excalidraw/utils`).
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

- **Imports:** Use workspace aliases `@excalidraw/common`, `@excalidraw/element`, `@excalidraw/math`, `@excalidraw/utils`, `@excalidraw/excalidraw` as in `vitest.config.mts` and each package’s `package.json` `"exports"`.
- **Tests:** Vitest (`vitest.config.mts`, `setupTests.ts`). Tests are colocated under `packages/*/tests/` or as `*.test.tsx` files.
- **Lint / format:** ESLint with `@excalidraw/eslint-config`; Prettier with `@excalidraw/prettier-config`.
- **Security:** Do not commit secrets; treat imported documents and URLs as untrusted when rendering or executing logic.

## Do-Not-Touch/Constraints

- **Secrets:** No API keys, tokens, or credentials in the repository; use environment variables and safe defaults.
- **Workspace aliases:** Keep `@excalidraw/*` imports consistent with `vitest.config.mts` and published `exports`; do not introduce ad-hoc path aliases without aligning bundler and TS configs.
- **Toolchain:** Do not lower **Node ≥ 18** or weaken root/package **TypeScript** compiler settings in feature work unless explicitly agreed.
- **App boundaries:** **PWA**, service worker, and **collaboration / hosting** wiring live primarily in **`excalidraw-app`** — do not move that surface into library packages without a clear design.
- **Workshop / agent config:** Treat **`.cursor/rules`**, **`.cursor/commands`**, **`AGENTS.md`**, **`AB_VALIDATION.md`**, and **`.cursorrules`** as documentation and guardrails — edit only when improving assignments or team conventions, not as a place for application business logic.

## Cursor project rules

Persistent agent guidance lives in **`.cursor/rules/*.mdc`**. Each rule file includes a **How to verify** section with concrete commands or checks.

| Rule file | When it applies |
|-----------|-----------------|
| `monorepo-core.mdc` | Always (`alwaysApply`) |
| `security.mdc` | Always |
| `typescript-conventions.mdc` | `packages/**/*.{ts,tsx}`, `excalidraw-app/**/*.{ts,tsx}` |
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
- **Shared primitives:** `packages/element`, `packages/math`, `packages/common`, `packages/utils`.

When unsure, start with the nearest existing feature and mirror its patterns for state, history, and tests.

## Verification checklist (acceptance)

Use this to confirm the repo meets workshop acceptance criteria:

1. **Rules:** At least six `.cursor/rules/*.mdc` files; each includes a **How to verify** section (open each file and search for that heading).
2. **Slash commands:** At least two files in `.cursor/commands/*.md` invokable as **`/pr-checks`** and **`/editor-package`** (type `/` in Cursor chat).
3. **A/B validation:** `AB_VALIDATION.md` documents scenario, result A, result B, and a conclusion showing a **difference** between conditions.
4. **This file:** `AGENTS.md` includes the sections above (prerequisites, overview, **Tech Stack**, architecture, yarn commands, conventions, **Do-Not-Touch/Constraints**, Cursor rules, slash commands, A/B pointer, where to change what, verification checklist).
5. **Build:** From repo root after `yarn install`, **`yarn build`** completes with exit code 0 and no errors.

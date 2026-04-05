# CLAUDE.md

Short pointer for Claude (and other agents) working in this repo. Full detail: **`AGENTS.md`**.

## Quick facts

- **Monorepo:** Yarn workspaces — packages in `packages/*`, app in `excalidraw-app`.
- **Stack:** TypeScript, React 19, Vite, Vitest.
- **Main package:** `packages/excalidraw` (`@excalidraw/excalidraw`).

## Commands

- `yarn start` — dev server  
- `yarn build` — production build  
- `yarn test:all` — typecheck + lint + prettier + tests  

## Agent configuration

- **Cursor rules:** `.cursor/rules/*.mdc` (monorepo, TypeScript, testing, security, package-specific globs).
- **Custom commands (Cursor):** `/pr-checks`, `/editor-package` — see `.cursor/commands/*.md`.

Follow `AGENTS.md` for architecture, conventions, and verification steps.

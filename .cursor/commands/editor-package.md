# Editor package workflow (`/editor-package`)

Invoke in chat with **`/editor-package`** (filename `editor-package.md` in `.cursor/commands/`).

Use when editing `packages/excalidraw` or its tests so the editor package stays buildable and tested.

## Steps

1. Identify the touched area (components, `data/`, `scene/`, tests) and run focused tests first, e.g. `yarn test --run packages/excalidraw/tests/<area>`.
2. Run `yarn build:packages` or at least `yarn build:excalidraw` if public exports or types changed.
3. For interaction or rendering changes, suggest `yarn start` and a short manual smoke test (create shape, undo, export).

## Output

List exact commands run, failing test names if any, and whether a full `yarn test:all` is still recommended before merge.

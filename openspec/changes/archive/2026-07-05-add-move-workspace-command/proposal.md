## Why

Users can already reorder tabs with `move_tab:<offset>`, but workspaces currently have no equivalent command. Adding workspace reordering makes workspace organization keyboard-configurable and keeps the workspace command set aligned with existing tab management behavior.

## What Changes

- Add a bindable `move_workspace:<offset>` action.
- Reorder the active workspace by the signed relative offset within the current window's workspace list.
- Wrap cyclically when moving before the first workspace or after the last workspace, matching `move_tab:<offset>` behavior.
- Preserve the moved workspace as the active workspace after reordering.
- Do not add default keybindings in this change.

## Capabilities

### New Capabilities

- None.

### Modified Capabilities

- `workspace-management`: Add reordering behavior for the active workspace via `move_workspace:<offset>`.

## Impact

- Input binding parsing and command metadata for a new parameterized action.
- Runtime action dispatch from surfaces to the GTK window.
- GTK window workspace list ordering, active workspace index maintenance, and sidebar refresh behavior.
- Tests for parsing and workspace reordering behavior where practical.

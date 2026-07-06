## Why

Workspace UI behavior has a few polish issues that make the feature feel inconsistent: generated workspace titles should be predictable from `Workspace 1`, the sidebar should expose the numeric shortcuts users can press, and release/debug behavior differs when closing the final workspace tab.

## What Changes

- Ensure automatic workspace naming starts at `Workspace 1` and continues from there for generated default names.
- Add a right-aligned, menu-like shortcut hint to each workspace sidebar row: `1` for the first workspace, continuing by position for direct `goto_workspace` targets, and `0` for the last workspace when it does not already have a direct numeric position hint.
- Align release and debug behavior when closing the last tab of the last workspace by allowing the window/application to close instead of creating a replacement workspace.

## Capabilities

### New Capabilities

- None.

### Modified Capabilities

- `workspace-renaming`: Clarify generated workspace names start at `Workspace 1`.
- `workspace-sidebar-ui`: Add sidebar shortcut hint display requirements.
- `workspace-navigation`: Clarify sidebar shortcut hints mirror workspace positional navigation and last-workspace navigation.
- `workspace-management`: Change final-tab closing behavior so release and debug builds follow the same specified lifecycle outcome without auto-recreating a workspace.

## Impact

- Affected code is expected in GTK workspace/sidebar UI, workspace title generation, workspace navigation display mapping, and final workspace/tab lifecycle handling.
- No new external dependencies or public APIs are expected.

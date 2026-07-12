## Why

When multiple workspaces are open in one Ghostty window, the OS-visible window title only reflects the active tab title, making windows harder to identify from titlebars, task switchers, and window-manager UI. Including the active workspace name in the dynamic window title gives users immediate context without relying on the sidebar being visible.

## What Changes

- Compose GTK dynamic window titles from the active workspace title and active tab title using the format `<workspace> - <tab title>`.
- Update the window title whenever the active workspace changes, the active workspace is renamed, the selected tab changes, or the active tab title changes.
- Preserve literal static title behavior: if `title` is configured, the configured title remains the complete window title and is not prefixed with the workspace name.
- Keep the window title as plain text; do not add partial coloring or other styling for the workspace portion.

## Capabilities

### New Capabilities
- `workspace-window-title`: Defines how GTK window titles include active workspace context while preserving literal static titles.

### Modified Capabilities

## Impact

- Affected code is expected in GTK window title binding/computation, active workspace switching, workspace rename notification handling, and selected tab title propagation.
- No terminal protocol changes, config format changes, or new dependencies are expected.

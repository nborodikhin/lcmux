## Why

Workspaces are currently identified only by generated positional labels such as `Workspace 1`, which makes them hard to distinguish in real projects once multiple workspaces are open. Users need a keyboard-driven way to give the active workspace a meaningful name without leaving the terminal workflow.

## What Changes

- Add a `rename_workspace` keybinding action that targets the active workspace in the current window, with Ctrl+Shift+R bound by default.
- Provide a GTK prompt for renaming the active workspace, reusing the existing title-prompt pattern where practical.
- Store a generated default title and an optional user-provided title on each `Workspace`; generated titles remain stable for the workspace lifetime.
- Generate default workspace names from a monotonic per-window counter and skip names already used by open workspaces.
- Refuse workspace renames that would duplicate another open workspace title.
- Update the workspace sidebar to display custom workspace titles and refresh immediately after rename.
- Allow users to override or rebind the action in config, for example `keybind = ctrl+shift+r=rename_workspace`.
- Add a Workspace section to the GTK top-bar menu between Window and Tab actions, exposing `Rename Workspace` and `New Workspace`.
- Size the workspace sidebar to at least 60px while allowing longer workspace titles to expand it naturally with symmetric horizontal padding.

## Capabilities

### New Capabilities
- `workspace-renaming`: User-driven workspace naming, including the rename action, prompt behavior, title storage, stable default labels, duplicate-name rejection, and sidebar updates.

### Modified Capabilities
- `workspace-sidebar-ui`: Sidebar entries display each workspace's stable current title, including custom names after a rename.

## Impact

- Affects the input/action pipeline: `src/input/Binding.zig`, `src/apprt/action.zig`, `src/Surface.zig`, and `src/apprt/gtk/class/application.zig`.
- Affects GTK workspace/window state in `src/apprt/gtk/class/workspace.zig` and `src/apprt/gtk/class/window.zig`.
- May reuse or extend existing title prompt UI patterns from tab/surface renaming.
- Adds config-level support for `rename_workspace` with Ctrl+Shift+R as the default keybinding.

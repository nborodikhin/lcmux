## 1. Workspace Naming

- [x] 1.1 Verify the GTK window workspace name counter initializes to 1 for each window and generated names are `Workspace 1`, `Workspace 2`, and so on.
- [x] 1.2 Add or adjust tests to cover first generated workspace title and stable generated titles after closing earlier workspaces.

## 2. Sidebar Shortcut Hints

- [x] 2.1 Update GTK workspace sidebar row construction to render each row as a title plus right-aligned shortcut hint.
- [x] 2.2 Compute shortcut hints from workspace position: `1` through `9` for direct `goto_workspace` positions and `0` for the last workspace when it does not already have a direct position hint.
- [x] 2.3 Apply subdued/menu-like styling to shortcut hint text while preserving active row indication, click behavior, and existing sidebar ordering.
- [x] 2.4 Verify sidebar sizing still accounts for workspace titles and row content without clipping long names or shortcut hints.
- [x] 2.5 Keep active-workspace updates from rebuilding sidebar rows during workspace switching, coalesce rapid workspace selection onto an idle callback, and use workspace-owned tab bars to avoid rebinding libadwaita tab bars across workspace tab views.

## 3. Final Workspace Close Behavior

- [x] 3.1 Remove the final-workspace auto-recreation path from the GTK empty-workspace handler.
- [x] 3.2 Ensure closing the last tab of the last workspace requests normal window close and does not create a replacement workspace or tab in release or debug builds.
- [x] 3.3 Ensure closing the last tab of a workspace still removes that workspace and focuses the adjacent workspace when other workspaces remain.

## 4. Verification

- [x] 4.1 Run targeted Zig tests for workspace naming/navigation/sidebar behavior where available.
- [x] 4.2 Run `zig build test -Dtest-filter=workspace` or the closest available targeted filter.
- [x] 4.3 Manually verify the GTK sidebar shows `1`, `2`, etc. and `0` for the last workspace beyond direct numeric positions.
- [x] 4.4 Manually verify closing the final tab exits/closes normally instead of recreating a workspace.

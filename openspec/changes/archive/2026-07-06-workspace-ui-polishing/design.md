## Context

Workspace support is implemented in the GTK window layer. Workspaces are kept in an ordered per-window list, generated names come from a per-window counter, the sidebar is rebuilt from the current workspace list, and workspace navigation already uses 1-indexed positions plus a separate last-workspace action.

The requested polish spans naming, visible sidebar affordances, and final-tab lifecycle behavior. The final-tab behavior currently conflicts with the desired outcome because an empty final workspace can be replaced with a new workspace and tab instead of allowing the normal close path to complete.

## Goals / Non-Goals

**Goals:**

- Keep generated workspace names stable and starting at `Workspace 1` for each window.
- Show right-aligned numeric shortcut hints in workspace sidebar rows, using the same positional model as workspace navigation.
- Make closing the last tab of the last workspace behave consistently in debug and release builds by not creating a replacement workspace.
- Preserve existing workspace ordering, click behavior, active indication, and rename behavior.

**Non-Goals:**

- Add new workspace actions or key bindings.
- Persist workspace names or sidebar visibility across launches.
- Redesign the sidebar beyond the requested shortcut hint polish.

## Decisions

- Use the existing per-window workspace name counter as the source of generated names. This keeps names independent from position and avoids renumbering after close or reorder operations.
- Render sidebar rows as a horizontal row with the title on the left and a shortcut hint on the right. The hint should use a dimmer/menu-like style so it reads as an accelerator rather than part of the workspace name.
- Derive sidebar hints from current workspace position. Positions 1 through 9 display their direct `goto_workspace` number; the last workspace displays `0` when it does not already have a direct numeric hint, matching the `last_workspace` shortcut.
- Remove final-workspace auto-recreation from the empty-workspace path. If removing the last workspace leaves no workspace in the window, the window should continue through the normal tab/window close lifecycle rather than creating a new workspace and tab.

## Risks / Trade-offs

- Shortcut hint layout may affect sidebar width calculation -> keep title sizing behavior intact and include hint width in row measurement naturally via GTK layout.
- Removing auto-recreation changes the existing zero-workspace guarantee -> update the spec contract and verify close behavior in release-like and debug builds.
- Hint display can become ambiguous with more than nine workspaces -> reserve `0` for the last workspace only when it is not already shown as positions 1-9; workspaces beyond nine that are not last do not get a direct-key hint.

## Context

Workspaces are currently window-local GTK objects with generated sidebar labels (`Workspace N`) derived from their position in `Window.syncWorkspaceSidebar`. This causes automatic renaming when earlier workspaces close. Tabs and surfaces already support user-editable titles through `TitleDialog`, action plumbing, and config-exposed keybinding actions. The workspace rename flow should reuse those patterns instead of introducing a separate prompt model.

The GTK top-bar menu currently groups Window actions before Tab actions. Workspace actions exist conceptually through `new_workspace`, `goto_workspace`, and `toggle_workspace_sidebar`, but the visible menu does not expose workspace creation or renaming. The workspace sidebar also uses a fixed `width-request: 180`; the requested behavior is for the left bar width to be the larger of 60px or the workspace name plus symmetric horizontal padding.

## Goals / Non-Goals

**Goals:**

- Add an action-driven rename flow for the active workspace, including keybinding/config parsing support, a Ctrl+Shift+R default binding, and GTK dispatch from the active surface/window target.
- Store a generated default title and optional custom title on each GTK `Workspace`, with empty rename input clearing the custom title and restoring the workspace's stable generated title.
- Generate default workspace titles from a monotonic per-window counter, skipping titles already used by open workspaces.
- Reject rename attempts that would duplicate another open workspace title.
- Refresh the workspace sidebar immediately after a workspace title changes.
- Reuse the existing title dialog UI, extending it only enough to present workspace-specific copy.
- Update the top-bar main menu to include a Workspace section between Window and Tab, with `Rename Workspace` and `New Workspace` items.
- Style the workspace sidebar so its width is at least 60px and otherwise follows the natural width of the workspace title plus equal left and right padding.

**Non-Goals:**

- Persist workspace names across application restarts or serialize them into config.
- Rename non-active workspaces from a context menu or command palette picker.
- Change workspace ordering, creation behavior, deletion behavior, or sidebar visibility defaults.
- Persist custom workspace titles across application restarts.

## Decisions

1. Store the custom workspace title on `Workspace`.

   The title belongs to the workspace, not the window sidebar row. `Workspace` should own a generated default title plus an optional duplicated custom title, expose a getter for the effective title, and expose a setter that treats an empty string as `null`. The generated default title is assigned once when the workspace is created and never recomputed from list position.

   Alternative considered: store a title map in `Window` keyed by workspace pointer. This avoids changing `Workspace`, but it splits workspace state from the object it describes and makes cleanup/reordering more fragile.

2. Use a workspace title-changed notification to refresh the sidebar.

   `Window.connectWorkspace` should connect to a workspace title notification/signal and call `syncWorkspaceSidebar`. This keeps rename updates immediate and avoids requiring every caller that might update a workspace title to remember sidebar refresh behavior.

   Alternative considered: call `syncWorkspaceSidebar` directly from the rename action. This is smaller initially, but it couples the action path to a specific UI refresh and is easier to miss if another workspace-title mutation path is added later.

3. Extend `TitleDialog.Target` with `workspace`.

   Existing terminal and tab rename flows already use `TitleDialog`; adding a workspace target provides the correct heading (`Rename Workspace`) while preserving dialog behavior and response handling. The workspace path can connect to the same `set` signal and apply the result to the active workspace.

   Alternative considered: create a dedicated workspace dialog. That duplicates template, signal, and lifecycle code without adding different behavior.

4. Add `rename_workspace` through the existing action pipeline.

   The new action should be added alongside other keybind actions in `input/Binding.zig`, converted through `apprt/action.zig`, dispatched in `Surface.zig`/application action handling, and implemented in GTK by resolving the active window/workspace. For non-GTK backends or unsupported targets, follow existing action patterns by returning false or logging an unsupported/unexpected target.

   Alternative considered: implement only a GTK window action without keybind/config support. That would satisfy the menu but not the proposal's keyboard-driven workflow.

5. Reject duplicate workspace names during rename.

   Before applying a confirmed prompt value, compare the requested effective title against all other open workspaces in the same window. If another workspace already has that title, leave the active workspace unchanged and notify the user. Empty input is treated as a request to restore the workspace's own generated default title and is subject to the same duplicate check.

   Alternative considered: allow duplicates and rely on positional order. This keeps renaming permissive, but makes the sidebar ambiguous and does not meet the requirement to refuse duplicate workspace names.

6. Generate default titles with a monotonic counter.

   `Window` should maintain a counter for generated names for the lifetime of that window, starting at 1. Creating a workspace tries `Workspace <counter>`, increments the counter, and skips any candidate currently used by an open workspace. This makes the first generated title `Workspace 1` and prevents later workspace removals from renaming existing workspaces.

   Alternative considered: continue deriving titles from workspace list position. This is simpler, but it automatically renames workspaces when earlier entries are removed.

7. Represent the top-bar workspace menu as its own section.

   Insert a section between the existing Window and Tab sections in `src/apprt/gtk/ui/1.5/window.blp`. The items should call window actions for `Rename Workspace` and `New Workspace` directly, keeping menu placement consistent with the user's requested ordering and avoiding disabled menu items when no terminal surface has focus.

   Alternative considered: add workspace items to the Window section. This keeps the menu shorter, but makes workspace actions harder to scan and does not match the requested section placement.

8. Let GTK layout compute sidebar width from content with a 60px minimum.

   Replace the fixed 180px sidebar request with a 60px minimum and ensure each row label uses equal start and end padding/margins. The label's natural width plus symmetric padding then determines widths above 60px. The sidebar styling should be expressed with CSS/classes where practical so row construction stays minimal and consistent.

   Alternative considered: compute text pixel widths manually and set a sidebar width after each sync. That is more brittle across font, DPI, theme, and localization changes than relying on GTK's natural-size layout.

## Risks / Trade-offs

- Long workspace names may make the sidebar very wide → Prefer GTK natural sizing initially and consider ellipsizing/max-width only if this becomes a usability issue.
- Duplicate rename attempts could silently fail if not surfaced → Show a user-visible toast and keep the current title unchanged.
- Adding a third `TitleDialog.Target` changes a shared dialog enum → Update all target switch statements and template/property registration paths so the dialog remains exhaustive.
- Workspace titles are GTK-local state → Non-GTK apprts may need stub handling for the new action until they support workspaces.
- Sidebar refresh depends on title notifications being emitted consistently → Centralize title mutation in one setter and have the window observe that signal/property.
- Menu actions can render disabled if they depend on active-surface binding dispatch → Use direct window handlers for menu actions while preserving keybinding dispatch through the action pipeline.

## Migration Plan

Implement the change behind the existing action/menu infrastructure with no data migration. Existing sessions continue to show generated workspace labels until a user renames a workspace. Rollback is removing the new action, workspace title state, dialog target, menu items, and sidebar sizing changes; no persisted data needs cleanup.

## Open Questions

- Should `Rename Workspace` use an ellipsis in visible menu text to match other prompt-opening menu items (`Change Tab Title...`), or exactly the requested label?
- None.

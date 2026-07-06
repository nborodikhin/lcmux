## Context

Workspace commands already exist for creation, renaming, navigation, direct selection, and sidebar visibility. Tabs also support `move_tab:<offset>`, which reorders the current tab cyclically within its workspace. Workspaces are stored as an ordered per-window list in the GTK window, with an active workspace index and a sidebar that reflects that order.

## Goals / Non-Goals

**Goals:**

- Add a parameterized `move_workspace:<offset>` binding action.
- Reorder the active workspace within the current window using the same signed, cyclic relative-offset semantics as `move_tab:<offset>`.
- Keep the moved workspace active after reordering.
- Refresh workspace UI state so positional navigation and the sidebar use the new order.

**Non-Goals:**

- Add default keybindings.
- Move workspaces between windows.
- Move tabs between workspaces.
- Add drag-and-drop workspace reordering in the sidebar.
- Rename, close, or otherwise expand workspace lifecycle commands.

## Decisions

- Mirror the existing `move_tab:<offset>` action shape with `move_workspace:<offset>`.
  - Rationale: Users already have a mental model and configuration syntax for signed relative movement.
  - Alternative considered: separate `move_workspace_left` and `move_workspace_right` actions. This would be less flexible and inconsistent with `move_tab`.

- Scope movement to the active workspace in the current window.
  - Rationale: Workspaces are currently window-local data structures, and existing workspace navigation targets the window containing the invoking surface.
  - Alternative considered: global workspace reordering. The current model has no global workspace list, so this would add unrelated architecture.

- Preserve cyclic wrapping semantics.
  - Rationale: This matches `move_tab:<offset>` and avoids no-op edge behavior at the first and last positions.
  - Alternative considered: clamp at boundaries. Clamping would diverge from tab reordering and make repeated shortcuts less predictable for users familiar with `move_tab`.

- Keep the active workspace active after reordering.
  - Rationale: The command reorganizes the workspace list; it should not switch the user's working context.
  - Alternative considered: make the workspace at the destination index active after array reordering. For a move operation this is equivalent only if the active index is explicitly updated to the moved workspace's new index; relying on the old index would accidentally activate a different workspace.

## Risks / Trade-offs

- Incorrect active index update could switch the active workspace unintentionally -> update the active index to the moved workspace's destination and verify with tests or focused manual checks.
- Sidebar order may not refresh automatically after mutating the backing array -> explicitly reuse existing workspace list synchronization after reordering.
- Large offsets need modulo-like wrapping rather than single-step wrap assumptions -> use logic that handles offsets larger than the number of workspaces, matching the command's parameterized nature.

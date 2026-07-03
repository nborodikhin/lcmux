## 1. Action Pipeline

- [x] 1.1 Add `rename_workspace` to the input binding action enum/parser and related config/keybind tests.
- [x] 1.2 Add the corresponding apprt action value and wire conversion from surface/keybind dispatch.
- [x] 1.3 Dispatch `rename_workspace` through `Surface.zig` and GTK application action handling, resolving surface targets to their containing window.
- [x] 1.4 Add or update unsupported-target handling for non-window/non-surface targets consistently with existing workspace actions.
- [x] 1.5 Bind Ctrl+Shift+R to `rename_workspace` by default.

## 2. Workspace Title Model

- [x] 2.1 Add optional custom title storage to `src/apprt/gtk/class/workspace.zig`, including allocation cleanup.
- [x] 2.2 Add workspace title getter/setter behavior where non-empty input stores a custom title and empty input clears it.
- [x] 2.3 Emit a title notification or signal whenever the custom workspace title changes.
- [x] 2.4 Update window workspace connection cleanup to connect and disconnect the title-change notification safely.
- [x] 2.5 Store a stable generated default title on each workspace.
- [x] 2.6 Generate default titles from a monotonic per-window counter starting at 1 and skip titles already used by open workspaces.

## 3. Rename Prompt

- [x] 3.1 Extend `TitleDialog.Target` with a workspace target and workspace-specific heading text.
- [x] 3.2 Add a window-level `promptWorkspaceTitle` flow that opens the title dialog for the active workspace.
- [x] 3.3 Initialize the prompt with the active workspace custom title or generated fallback title.
- [x] 3.4 Apply confirmed prompt values to the active workspace and leave titles unchanged on cancellation.
- [x] 3.5 Refuse rename attempts that duplicate another open workspace title.

## 4. Sidebar UI

- [x] 4.1 Update `syncWorkspaceSidebar` to display each workspace's effective title instead of always generating `Workspace N`.
- [x] 4.2 Refresh the sidebar immediately when a workspace title changes.
- [x] 4.3 Change the workspace sidebar layout from fixed 180px width to a 60px minimum with natural width expansion.
- [x] 4.4 Ensure workspace row labels have matching left and right horizontal padding or margins.

## 5. Top-Bar Menu

- [x] 5.1 Add a window action binding for prompting the active workspace rename.
- [x] 5.2 Insert a Workspace section in `src/apprt/gtk/ui/1.5/window.blp` between Window and Tab sections.
- [x] 5.3 Add `Rename Workspace` and `New Workspace` menu items wired to the correct window actions.
- [x] 5.4 Ensure Workspace menu items are enabled by handling them directly on the window.

## 6. Verification

- [x] 6.1 Add targeted Zig tests for parsing/configuring `rename_workspace` where action parser tests exist.
- [x] 6.2 Run targeted tests for action parsing/keybinding changes.
- [x] 6.3 Run `zig fmt` on touched Zig files.
- [x] 6.4 Run `openspec validate "rename-workspace" --changes`.

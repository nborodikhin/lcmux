## 1. Input and Action Plumbing

- [x] 1.1 Add `move_workspace: isize` to the input binding action model with documentation matching `move_tab` semantics.
- [x] 1.2 Add command metadata for `move_workspace` so command discovery exposes move-left and move-right workspace variants.
- [x] 1.3 Add an application runtime action value for `move_workspace` and keep action key ordering synchronized with public action definitions as required.
- [x] 1.4 Dispatch `move_workspace:<offset>` from surface binding execution to the runtime application action.

## 2. GTK Workspace Reordering

- [x] 2.1 Add GTK application action dispatch for `move_workspace` targeted at the window containing the invoking surface.
- [x] 2.2 Implement active workspace reordering on the GTK window's workspace list using signed cyclic offset semantics.
- [x] 2.3 Preserve the moved workspace as active after reordering and refresh sidebar/list UI state so ordering and active indication remain correct.
- [x] 2.4 Ensure single-workspace movement is a no-op.

## 3. Verification

- [x] 3.1 Add or update binding parser tests proving `move_workspace:<offset>` is accepted.
- [x] 3.2 Add focused tests where practical for cyclic movement, active workspace preservation, and single-workspace no-op behavior.
- [x] 3.3 Run targeted Zig tests for input binding and any affected GTK/workspace logic.
- [x] 3.4 Run `zig fmt` on touched Zig files.

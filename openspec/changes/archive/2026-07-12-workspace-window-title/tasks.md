## 1. Title Composition

- [x] 1.1 Add GTK window-level title composition that combines active workspace title and active tab title as `<workspace> - <tab title>` when no static `title` config is set.
- [x] 1.2 Preserve literal static title behavior so configured `title` remains the complete window title without workspace prefixing.
- [x] 1.3 Keep tab titles, surface titles, title reporting, and copy-title behavior unchanged.

## 2. Synchronization

- [x] 2.1 Recompute the window title when the active workspace changes.
- [x] 2.2 Recompute the window title when the active workspace is renamed.
- [x] 2.3 Recompute the window title when the selected tab or active tab title changes.

## 3. Verification

- [x] 3.1 Add or update targeted tests where practical for dynamic title formatting and static-title literal behavior.
- [x] 3.2 Run targeted Zig tests for GTK workspace/window title behavior, or the closest available test filter.
- [x] 3.3 Manually verify a workspace named `Job` with active tab title `htop` shows `Job - htop` in the GTK window title.
- [x] 3.4 Manually verify no color, markup, or partial styling is introduced into the OS-visible window title.

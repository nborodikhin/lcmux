## Context

GTK window titles currently follow the selected tab title. The selected tab title is already derived from terminal-set surface titles, surface overrides, tab overrides, config title fallback, bell state, and zoom state. Workspaces are managed at the GTK window layer, have stable generated or overridden titles, and the active workspace can change independently from the selected tab within each workspace, especially when the sidebar is closed.

This change should add active workspace context to dynamic OS-visible window titles without changing the meaning of terminal-set titles or user-configured static titles.

## Goals / Non-Goals

**Goals:**
- Show dynamic GTK window titles as `<workspace> - <tab title>` when no static `title` config is set.
- Keep the workspace portion synchronized with active workspace switches and workspace renames.
- Keep the tab title portion synchronized with selected tab and active tab title changes.
- Preserve configured `title` as a literal complete window title.

**Non-Goals:**
- Add color, markup, or partial styling to the workspace portion of the OS-visible title.
- Change tab titles, surface titles, title reporting, or clipboard title behavior.
- Add a new title-format configuration option.
- Change workspace naming or sidebar behavior.

## Decisions

1. Compose the decorated title at the window layer.

   The active workspace is only known by the GTK window, while active tab title binding already terminates at the window title. Keeping composition in `Window` avoids pushing workspace knowledge into `Tab`, `Surface`, or terminal title handling.

2. Preserve static config title as a full literal title.

   The existing `title` config intentionally forces the window title and ignores title changes. This change should not reinterpret that value as only a tab-title component, because users who configure a static title expect exact output.

3. Use plain text format `<workspace> - <tab title>`.

   Plain text works consistently across the headerbar, OS window title, task switchers, and window-manager UI. Partial color would only be possible in Ghostty-controlled widgets and would not transfer to OS-visible title consumers.

4. Update title composition from existing change points.

   The implementation should recompute the window title when active workspace changes, a workspace title changes, selected tab changes, or the selected tab title changes. Existing workspace and tab selection paths already centralize most of these events.

## Risks / Trade-offs

- Dynamic title recomputation can miss a change path -> connect recomputation to the same active workspace and selected tab binding paths that currently control window title updates.
- Title duplication may occur if users manually include the workspace name in tab titles -> accept this as user-controlled text; no parsing or de-duplication should be attempted.
- Longer window titles may truncate in titlebars or task switchers -> the chosen format puts workspace first so the highest-level context remains visible when truncated.

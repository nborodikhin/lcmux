## Purpose

Define how GTK window titles expose active workspace context while preserving existing terminal and static title behavior.

## Requirements

### Requirement: Dynamic window title includes active workspace
When no static title is configured, the GTK window title SHALL include the active workspace title followed by the active tab title using the format `<workspace> - <tab title>`.

#### Scenario: Active workspace prefixes tab title
- **WHEN** the active workspace is named `Job` and the active tab title is `htop`
- **THEN** the GTK window title is `Job - htop`

#### Scenario: Generated workspace title is used
- **WHEN** the active workspace has no custom title and its generated title is `Workspace 1`, and the active tab title is `htop`
- **THEN** the GTK window title is `Workspace 1 - htop`

### Requirement: Dynamic window title tracks workspace and tab changes
When no static title is configured, the GTK window title SHALL update whenever the active workspace title or active tab title changes.

#### Scenario: Switching active workspace updates title
- **WHEN** the active workspace changes from `Job` to `EasyNap`
- **THEN** the GTK window title uses `EasyNap` as the workspace portion

#### Scenario: Renaming active workspace updates title
- **WHEN** the active workspace title changes from `Workspace 1` to `Job`
- **THEN** the GTK window title uses `Job` as the workspace portion

#### Scenario: Selected tab title updates title
- **WHEN** the active tab title changes from `shell` to `htop`
- **THEN** the GTK window title uses `htop` as the tab title portion

### Requirement: Static title remains literal
When a static `title` is configured, the GTK window title SHALL remain exactly the configured title and SHALL NOT include the active workspace title.

#### Scenario: Configured title is not prefixed
- **WHEN** `title` is configured as `My Title` and the active workspace is named `Job`
- **THEN** the GTK window title is `My Title`

### Requirement: Window title is plain text
The workspace-aware GTK window title SHALL be represented as plain text without partial color, markup, or styling in the title string.

#### Scenario: Workspace portion has no markup
- **WHEN** the active workspace is named `Job` and the active tab title is `htop`
- **THEN** the GTK window title text is exactly `Job - htop`

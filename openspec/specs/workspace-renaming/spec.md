## Purpose

Define user-driven workspace naming, including rename actions, prompt behavior, stable generated titles, duplicate-name handling, and menu access.

## Requirements

### Requirement: Rename active workspace action
The system SHALL provide a bindable `rename_workspace` action that targets the active workspace in the current window and is bound by default to Ctrl+Shift+R.

#### Scenario: Rename action opens prompt
- **WHEN** the user invokes `rename_workspace` from a surface in a window with an active workspace
- **THEN** the system presents a workspace rename prompt for that active workspace

#### Scenario: Rename action is configurable
- **WHEN** the user configures a key binding such as `keybind = ctrl+shift+r=rename_workspace`
- **THEN** the configuration parser accepts the binding and dispatches the action when the trigger is pressed

#### Scenario: Default rename action binding
- **WHEN** the user presses Ctrl+Shift+R with default keybindings enabled
- **THEN** the system dispatches `rename_workspace` for the active workspace

### Requirement: Workspace rename prompt
The system SHALL use a GTK prompt to collect the new active workspace name and SHALL label the prompt for workspace renaming.

#### Scenario: Prompt starts with current custom title
- **WHEN** the active workspace has a custom title and the user invokes `rename_workspace`
- **THEN** the prompt input is initialized with that custom title

#### Scenario: Prompt starts with fallback title
- **WHEN** the active workspace has no custom title and the user invokes `rename_workspace`
- **THEN** the prompt input is initialized with the generated fallback workspace title

#### Scenario: Prompt cancellation leaves title unchanged
- **WHEN** the workspace rename prompt is cancelled
- **THEN** the active workspace title remains unchanged

### Requirement: Workspace title storage
Each workspace SHALL store a generated default title and an optional custom title. The system SHALL use the custom title when set and SHALL use the generated default title when no custom title is set.

#### Scenario: Setting a custom workspace title
- **WHEN** the user confirms the workspace rename prompt with a non-empty value
- **THEN** the active workspace stores that value as its custom title

#### Scenario: Clearing a custom workspace title
- **WHEN** the user confirms the workspace rename prompt with an empty value
- **THEN** the active workspace clears its custom title and uses its own generated default title

### Requirement: Stable generated workspace titles
The system SHALL assign each new workspace a generated default title from a monotonic per-window counter starting at 1 and SHALL NOT recompute generated titles from workspace position after creation.

#### Scenario: First workspace starts at one
- **WHEN** the first workspace is created in a window
- **THEN** its generated default title is `Workspace 1`

#### Scenario: Closing an earlier workspace preserves later workspace title
- **WHEN** `Workspace 1` and `Workspace 2` exist and `Workspace 1` is closed
- **THEN** the remaining workspace keeps the title `Workspace 2`

#### Scenario: New workspace name skips open workspace titles
- **WHEN** the next generated candidate title is already used by an open workspace
- **THEN** the system increments the counter until it finds an unused generated title and assigns that title to the new workspace

### Requirement: Duplicate workspace names are rejected
The system SHALL refuse to rename a workspace to a title already used by another open workspace in the same window.

#### Scenario: Rename to duplicate custom title is rejected
- **WHEN** another open workspace is titled `aaa` and the user confirms a rename to `aaa`
- **THEN** the active workspace title remains unchanged

#### Scenario: Clearing to duplicate default title is rejected
- **WHEN** clearing the custom title would make the active workspace title match another open workspace title
- **THEN** the active workspace title remains unchanged

### Requirement: Top-bar workspace menu actions
The GTK top-bar main menu SHALL include an enabled Workspace section between the Window and Tab sections, containing actions to rename the active workspace and create a new workspace.

#### Scenario: Workspace section placement
- **WHEN** the user opens the GTK top-bar main menu
- **THEN** the menu displays the Workspace section after Window actions and before Tab actions

#### Scenario: Rename workspace menu item
- **WHEN** the user activates the `Rename Workspace` item from the Workspace section
- **THEN** the system invokes the active workspace rename prompt

#### Scenario: Rename workspace menu item is enabled
- **WHEN** the user opens the GTK top-bar main menu
- **THEN** the `Rename Workspace` item is enabled

#### Scenario: New workspace menu item
- **WHEN** the user activates the `New Workspace` item from the Workspace section
- **THEN** the system creates and activates a new workspace

#### Scenario: New workspace menu item is enabled
- **WHEN** the user opens the GTK top-bar main menu
- **THEN** the `New Workspace` item is enabled

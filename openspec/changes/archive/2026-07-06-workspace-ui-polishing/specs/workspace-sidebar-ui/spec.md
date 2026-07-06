## MODIFIED Requirements

### Requirement: Sidebar lists workspaces in order
The sidebar SHALL display all open workspaces in a window as a vertical list, ordered by workspace position (the same order used by positional navigation, i.e. `goto_workspace`). Each sidebar entry SHALL display the workspace's current title, using a custom workspace title when one is set and the workspace's stable generated default title when one is not set. Each sidebar entry SHALL also display a shortcut hint at the end of the row when a numeric workspace shortcut can target that workspace.

#### Scenario: New workspace appears in the sidebar
- **WHEN** a new workspace is created
- **THEN** it appears in the sidebar at the end of the list

#### Scenario: Destroyed workspace disappears from the sidebar
- **WHEN** a workspace is destroyed because its last tab closed
- **THEN** it is removed from the sidebar list

#### Scenario: Later workspace keeps title after earlier workspace closes
- **WHEN** an earlier workspace is destroyed
- **THEN** remaining sidebar entries keep their existing workspace titles

#### Scenario: Renamed workspace updates in the sidebar
- **WHEN** the active workspace title is changed
- **THEN** the sidebar entry for that workspace updates to display the new title without requiring a window restart

#### Scenario: Sidebar displays direct goto hint
- **WHEN** a workspace is in positions 1 through 9
- **THEN** its sidebar entry displays that 1-indexed position as a right-aligned shortcut hint

#### Scenario: Sidebar displays last-workspace hint
- **WHEN** the last workspace does not already display a direct position hint
- **THEN** its sidebar entry displays `0` as a right-aligned shortcut hint

### Requirement: Sidebar shortcut hints use subdued styling
Sidebar shortcut hints SHALL be visually distinct from workspace titles by using a slightly darker or otherwise subdued text style comparable to menu accelerator text.

#### Scenario: Shortcut hint is visually secondary
- **WHEN** the sidebar displays a workspace row with a shortcut hint
- **THEN** the shortcut hint appears less prominent than the workspace title

#### Scenario: Shortcut hint is aligned at row end
- **WHEN** the sidebar displays a workspace row with a shortcut hint
- **THEN** the shortcut hint appears at the end of the row opposite the workspace title

### Requirement: Sidebar indicates the active workspace
The sidebar SHALL visually distinguish whichever workspace is currently active in the window, and SHALL update this indication whenever the active workspace changes (via navigation shortcuts, sidebar clicks, or workspace creation/destruction). Rapid workspace switching SHALL NOT crash or repeatedly force libadwaita tab-view replacement while prior switch events are still being processed. Each workspace SHALL own a stable libadwaita tab bar bound to that workspace's tab view, rather than rebinding a single shared tab bar across workspace tab views.

#### Scenario: Active indicator follows navigation
- **WHEN** the user switches to a different workspace via any navigation action
- **THEN** the sidebar updates to indicate the newly active workspace and no longer indicates the previous one

#### Scenario: Rapid workspace switching remains stable
- **WHEN** the user rapidly switches between workspaces via navigation shortcuts or sidebar clicks
- **THEN** workspace selection is coalesced safely and the active indicator updates without crashing the application

#### Scenario: Adding tabs after switching remains stable
- **WHEN** the user switches between workspaces and then adds tabs to the active workspace
- **THEN** the tabs appear in that workspace's tab bar without crashing the application

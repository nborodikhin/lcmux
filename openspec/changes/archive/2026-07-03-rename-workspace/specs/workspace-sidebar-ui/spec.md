## MODIFIED Requirements

### Requirement: Sidebar lists workspaces in order
The sidebar SHALL display all open workspaces in a window as a vertical list, ordered by workspace position (the same order used by positional navigation, i.e. `goto_workspace`). Each sidebar entry SHALL display the workspace's current title, using a custom workspace title when one is set and the workspace's stable generated default title when one is not set.

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

### Requirement: Sidebar sizes to workspace titles
The sidebar SHALL be at least 60px wide and SHALL expand when necessary to fit the displayed workspace title plus symmetric left and right horizontal padding.

#### Scenario: Sidebar uses minimum width for short names
- **WHEN** every displayed workspace title fits within 60px including horizontal padding
- **THEN** the sidebar width is 60px

#### Scenario: Sidebar expands for longer names
- **WHEN** a displayed workspace title requires more than 60px including horizontal padding
- **THEN** the sidebar expands to fit the title and keeps matching left and right horizontal padding around the title

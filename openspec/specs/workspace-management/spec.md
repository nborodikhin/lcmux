## Purpose

Define workspace lifecycle behavior and how workspaces contain Ghostty tabs within a window.

## Requirements

### Requirement: Workspace as a tab container
A workspace SHALL be a container that owns zero or more Ghostty tabs (each tab may itself contain splits, unchanged from stock Ghostty behavior). Every open tab in a window SHALL belong to exactly one workspace, and a window SHALL show exactly one workspace's tabs in its content area at a time (the active workspace).

#### Scenario: Tab belongs to its creating workspace
- **WHEN** a new tab is opened while a given workspace is active
- **THEN** the new tab is added to that active workspace

### Requirement: Create a new workspace
The system SHALL provide a `new_workspace` action, bound by default to Ctrl+Shift+N, that creates a new workspace containing exactly one new tab, appends it after the current last workspace, and switches the window's active workspace to it.

#### Scenario: User creates a workspace
- **WHEN** the user invokes `new_workspace` (Ctrl+Shift+N)
- **THEN** a new workspace is created containing one new tab, the new workspace becomes the active workspace, and the new tab is focused

### Requirement: Destroy workspace when empty
A workspace SHALL be automatically destroyed the moment its last tab is closed when at least one other workspace remains. If other workspaces remain, the active workspace SHALL move to an adjacent workspace. If no other workspaces remain, the window/application SHALL continue through the normal tab/window close lifecycle without creating a replacement workspace.

#### Scenario: Closing the only tab in a workspace
- **WHEN** the user closes the last remaining tab in a workspace
- **THEN** that workspace is destroyed and removed from the workspace list

#### Scenario: Closing one of several tabs in a workspace
- **WHEN** the user closes a tab in a workspace that has other tabs remaining
- **THEN** the workspace is not destroyed and continues to hold its remaining tabs

#### Scenario: Destroying the active workspace focuses a neighbor
- **WHEN** the active workspace is destroyed because its last tab closed, and at least one other workspace remains
- **THEN** the window's active workspace becomes the workspace that was adjacent to the destroyed one (preferring the next workspace, falling back to the previous one if the destroyed workspace was last)

#### Scenario: Destroying the final workspace does not recreate it
- **WHEN** the active workspace is destroyed because its last tab closed, and no other workspace remains
- **THEN** no replacement workspace or tab is created automatically

#### Scenario: Final workspace close uses window lifecycle
- **WHEN** the last tab in the only remaining workspace is closed
- **THEN** the system requests normal window close without first leaving a live window with zero workspaces

### Requirement: Reorder the active workspace
The system SHALL provide a bindable `move_workspace` action taking a signed relative offset. Invoking `move_workspace:<offset>` SHALL move the active workspace by that offset within the current window's ordered workspace list. Movement SHALL wrap cyclically when the destination is before the first workspace or after the last workspace, and the moved workspace SHALL remain active after the reorder.

#### Scenario: Move active workspace forward
- **WHEN** three workspaces exist, the first workspace is active, and the user invokes `move_workspace:1`
- **THEN** the active workspace moves to the second position and remains active

#### Scenario: Move active workspace backward
- **WHEN** three workspaces exist, the second workspace is active, and the user invokes `move_workspace:-1`
- **THEN** the active workspace moves to the first position and remains active

#### Scenario: Move wraps past the end
- **WHEN** three workspaces exist, the last workspace is active, and the user invokes `move_workspace:1`
- **THEN** the active workspace moves to the first position and remains active

#### Scenario: Move wraps before the beginning
- **WHEN** three workspaces exist, the first workspace is active, and the user invokes `move_workspace:-1`
- **THEN** the active workspace moves to the last position and remains active

#### Scenario: Moving the only workspace is a no-op
- **WHEN** one workspace exists and the user invokes `move_workspace:1` or `move_workspace:-1`
- **THEN** the workspace order and active workspace remain unchanged

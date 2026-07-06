## ADDED Requirements

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

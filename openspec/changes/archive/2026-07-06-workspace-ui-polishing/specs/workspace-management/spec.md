## REMOVED Requirements

### Requirement: Zero-workspace guarantee
**Reason**: Closing the last tab of the last workspace should follow normal window/application close behavior. Automatically creating a replacement workspace in release builds conflicts with debug behavior and surprises users who intended to close the window.

**Migration**: Remove the final-workspace auto-recreation path. When the final workspace becomes empty, request normal window close instead of destroying the workspace in place and leaving a live zero-workspace window. Existing workspace creation remains available through startup/new-window behavior and the `new_workspace` action.

#### Scenario: Closing the last tab of the last workspace
- **WHEN** the user closes the last tab in the only remaining workspace in a window
- **THEN** the system does not create a replacement workspace automatically

## MODIFIED Requirements

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

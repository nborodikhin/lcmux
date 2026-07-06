## ADDED Requirements

### Requirement: Sidebar shortcut hints mirror navigation actions
Numeric shortcut hints shown in the workspace sidebar SHALL correspond to workspace navigation actions: hints `1` through `9` SHALL indicate the `goto_workspace` position for that row, and hint `0` SHALL indicate the `last_workspace` target.

#### Scenario: Hint matches goto workspace position
- **WHEN** the sidebar displays hint `1` for the first workspace
- **THEN** invoking `goto_workspace` for position 1 targets that same workspace

#### Scenario: Zero hint matches last workspace action
- **WHEN** the sidebar displays hint `0` for a workspace
- **THEN** invoking `last_workspace` targets that same workspace

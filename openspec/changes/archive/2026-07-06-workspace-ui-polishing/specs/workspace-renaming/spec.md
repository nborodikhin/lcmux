## MODIFIED Requirements

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

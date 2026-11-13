# Known gaps

Living list of disagreements. Update a row when a person decides or a document is corrected. Do not close a row by guessing.

The client wrote the current code before these documents. Dark mode is the exception the team added. When a row is unresolved, ask, and change neither side until the team chooses.

## Documents that disagree with each other

### OBD-II

| Source | What it says |
|---|---|
| [traceability.md](traceability.md) | `UC-OBD-pair-device` and `UC-OBD-stream-vehicle-data` are Blocked. The old design/code cell said feasibility work was required, and verification was hardware verification. |
| [requirements/use-cases.md](../requirements/use-cases.md) | Pairing exists (Bluetooth LE, USB serial, simulator). Streaming exists for speed, RPM, and throttle. Turn signal is not available. Connection status exists. |
| [requirements/vision-and-scope.md](../requirements/vision-and-scope.md) | The demo benchmark is one live brake signal. Turn-signal state is not on standard OBD-II. `AS-adapter-ble` says the test adapter does not expose turn signal, brake, or gear. |
| [requirements/OPEN-ISSUES.md](../requirements/OPEN-ISSUES.md) | `OI-1` still asks whether the team is using the scanners at all. |

Unresolved. No source in this table is the winner.

### Admin roles

| Source | What it says |
|---|---|
| [requirements/software-requirements-specification.md](../requirements/software-requirements-specification.md) | An organization administrator may manage organizations, drive plans, maneuvers, score criteria, session times, and integration settings. Exact permissions are TBD. A system administrator handles system-level account and access management. Those permissions are TBD. |
| [requirements/use-cases.md](../requirements/use-cases.md), [requirements/business-rules.md](../requirements/business-rules.md) (`BR-platform-admin-settings`), [requirements/vision-and-scope.md](../requirements/vision-and-scope.md) | Platform administrator, from a platform-admin flag on the user profile. |
| Code | Roles are `examiner`, `instructor`, `parent`, and `student`, plus `isPlatformAdmin`. There is no organization-administrator or system-administrator role. |

Unresolved. Do not add a role to close this row.

### Traceability and the use-case inventory

[traceability.md](traceability.md) still has Status `Specified` and no code link for almost every row. Only `UC-SETT-toggle-dark-mode` is `Implemented`. [requirements/use-cases.md](../requirements/use-cases.md) marks many of those same use cases Exists or Partial. Those inventory notes were not copied into the matrix.

### Open issues

[requirements/OPEN-ISSUES.md](../requirements/OPEN-ISSUES.md) lists `OI-1` and a sample `OI-0` row. [requirements/vision-and-scope.md](../requirements/vision-and-scope.md) cites other `OI-*` identifiers that are not filed there. Do not invent the missing answers.

## Documents and code

| Area | Documents | Code | Status |
|---|---|---|---|
| Hours tracking | Texas PTDE hours and configurable category progress (`UC-HOUR-*`, `FR-HOURS-*`). Vision and scope lists `FEAT-training-hours-log` as the first feature to cut. | No hours module | Documents ahead of code. Open product work. |
| Roster / start drive | Spec and rules require roster students | Roster required; placeholder name-only creation blocked | Aligned |
| Integrations | Reservation and school-integration language. Vision and scope leaves further reservation work out of the MVP. The architecture says the proof of concept already reads a reservation API. | Generic REST for appointments, students, and results | Partially aligned |

## Quick Dev spec versus git

This section is not a class-document gap. The class documents do not name the migration below.

| Area | Quick Dev spec | Git | Status |
|---|---|---|---|
| Schema history | Drive Tracker's `_bmad-output/implementation-artifacts/spec-managed-student-rosters.md` names `api/db/migrations/20260828000001_roster_students.js` | `api/db/migrations/` has no files | Planning spec ahead of git. Do not invent the migration. |

## Rule

Read the code and the documents. Cite identifiers as intent. Do not resolve a row in this file by guessing. If approval is unclear, ask the team.

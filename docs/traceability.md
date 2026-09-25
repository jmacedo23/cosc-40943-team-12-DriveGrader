# Requirements traceability

> **Purpose (one line):** Team 12 can keep each Drive Grader use case honest end to end, tracing it forward to determine whether it is designed, built, and tested, and backward to determine why each piece of implementation exists.

!!! note "Living document"
    This traceability file must be updated as Drive Grader moves from requirements into design, implementation, and testing. A trace should only claim links that currently exist. Missing implementation or tests should be marked as such rather than invented.

## 2. Where it fits

- **Prerequisites:** the Drive Grader vision and scope, use cases, business rules, software requirements specification, implementation, and testing.
- **Leads into:** implementation, testing, maintenance, change-impact analysis, and final project handoff.
- **How Team 12 will use it:** traceability will be updated as use cases move from specification into design, code, and tests. The primary traceability spine will be maintained throughout the project instead of being reconstructed at the end.
- **Primary traceability spine:** `Use Case → Functional Requirement → Design/Code → Test`.
- **Project artifacts involved:** `requirements/use-cases.md`, `requirements/business-rules.md`, `requirements/software-requirements-specification.md`, `requirements/OPEN-ISSUES.md`, application source code, and project tests.
- **Course outcome it supports:** maintaining living traceability from specification to design to code to test and keeping those links honest with tooling and human review.

## 3. Motivation

- **The problem on Drive Grader:** Drive Grader contains several connected areas including drive-session tracking, real-time infraction grading, DL-40 grading, training-hour tracking, administrative configuration, and OBD-II integration. As implementation grows, the team cannot rely on memory to know which code satisfies which requirement or why a particular component exists.

- **Why it matters more with AI:** AI can generate implementation much faster than a human developer can manually remember and document every decision. A human developer may remember why a particular function, component, or validation rule was written, but an AI agent does not reliably carry that reasoning between sessions. Traceability externalizes that reasoning into the repository. Rather than relying on memory, the team can follow a durable chain from a requirement to its implementation and its verifying test.

- **Why we can afford it:** maintaining traceability manually has traditionally been tedious. AI can assist with the mechanical work of updating rows, finding identifiers, locating likely implementation files, and checking whether linked files still exist. That makes maintaining the matrix practical throughout the semester instead of treating it as a document created only for final submission.

- **The honesty rule:** AI may maintain or propose traceability links, but AI should not decide by itself whether those links are correct. Mechanical checks can verify that an identifier, file, or test exists. Human review determines whether that artifact actually satisfies the requirement. A plausible-looking trace is not enough.

## 4. Core concepts

### The single-axis traceability spine

Team 12 will maintain the following primary traceability chain:

```text
Use Case → Functional Requirement → Design/Code → Test
```

For example:

```text
UC-GRAD-log-infraction
        ↓
FR-INFRACTION-record
        ↓
Infraction logging implementation
        ↓
Infraction logging test
```

Each edge answers a different question.

- `Use Case → Functional Requirement`: what system behavior is required to accomplish the user's goal?
- `Functional Requirement → Design/Code`: where is that behavior implemented?
- `Design/Code → Test`: what verifies that the implementation behaves as required?

A use case should not be considered completely traced until the chain reaches verification.

### Forward traceability

Forward traceability begins with a requirement and asks:

> Is this actually built and tested?

For example:

```text
UC-DL40-conduct-graded-test
        ↓
FR-DL40-maneuvers
        ↓
DL-40 grading implementation
        ↓
DL-40 grading tests
```

If a requirement has no implementation, the implementation is incomplete.

If it has implementation but no verifying test, the trace is also incomplete.

Forward traceability is primarily a coverage question.

### Backward traceability

Backward traceability begins with implementation and asks:

> Why does this code exist?

For example:

```text
OBD-II vehicle-data parser
        ↑
FR-OBD2-data
        ↑
UC-OBD-stream-vehicle-data
```

If code cannot be traced to a requirement or approved design decision, Team 12 must determine whether:

- the code is unnecessary,
- the code is experimental,
- the specification is missing a requirement, or
- an implementation decision was made without approval.

Backward traceability is primarily a justification question.

### Stable identifiers

Traceability depends on stable identifiers.

Drive Grader currently uses identifiers including:

- `UC-*` — use cases
- `FR-*` — functional requirements
- `BR-*` — business rules
- `FEAT-*` — features
- `UI-*` — user-interface requirements
- `SI-*` — software/system interfaces
- `DI-*` — data requirements
- quality identifiers such as `USE-*`, `PER-*`, `SEC-*`, `SAF-*`, and others

Examples include:

```text
UC-SESS-start-drive-session
UC-GRAD-log-infraction
UC-DL40-conduct-graded-test

FR-DRIVE-start
FR-INFRACTION-record
FR-DL40-digital

BR-dl40-maneuver-deductions
```

These identifiers act as permanent addresses. Once other artifacts cite an identifier, it should not be silently renamed or reused for a different concept.

### Cite, do not restate

Traceability entries should cite requirement identifiers rather than copy the requirement text.

For example:

```text
FR-DL40-maneuvers
```

should be referenced rather than duplicating the entire functional requirement inside this file.

This keeps each requirement in one authoritative location and prevents copies from becoming inconsistent.

### The verification edge

A requirement is not fully traced merely because code exists.

The expected chain is:

```text
Requirement → Implementation → Verification
```

The verification may be:

- an automated unit test,
- an integration test,
- an end-to-end test,
- a hardware verification procedure,
- or another documented acceptance procedure where automation is not appropriate.

The exact test or procedure should be linked in the matrix.

### Current Drive Grader traceability matrix

The initial matrix is based on the current specification. Implementation and tests should be added as they are created.

| Use Case | Functional Requirement(s) | Business Rule(s) | Design / Code | Test / Verification | Status |
|---|---|---|---|---|---|
| `UC-SESS-start-drive-session` | `FR-DRIVE-start`, `FR-DRIVE-route`, `FR-METRICS-gps` | None currently identified | Not yet linked | Not yet linked | Specified |
| `UC-SESS-end-drive-session` | `FR-DRIVE-save`, `FR-DRIVE-summary` | None currently identified | Not yet linked | Not yet linked | Specified |
| `UC-SESS-view-session-history` | `FR-DRIVE-summary` | None currently identified | Not yet linked | Not yet linked | Specified |
| `UC-GRAD-log-infraction` | `FR-INFRACTION-record` | None currently identified | Not yet linked | Not yet linked | Specified |
| `UC-GRAD-review-infraction-log` | `FR-DRIVE-summary`, `FR-INFRACTION-record` | None currently identified | Not yet linked | Not yet linked | Specified |
| `UC-DL40-configure-checklist` | `FR-DL40-reorder`, `FR-DL40-maneuvers`, `FR-DL40-route-order` | `BR-dl40-maneuver-deductions` | Not yet linked | Not yet linked | Specified |
| `UC-DL40-conduct-graded-test` | `FR-DL40-digital`, `FR-DL40-driver`, `FR-DL40-guardian`, `FR-DL40-maneuvers`, `FR-DL40-route-order` | `BR-dl40-maneuver-deductions` | Not yet linked | Not yet linked | Specified |
| `UC-DL40-capture-signatures-and-print` | `FR-DL40-signatures`, `FR-DL40-output` | None currently identified | Not yet linked | Not yet linked | Specified |
| `UC-HOUR-track-progress` | `FR-HOURS-record`, `FR-HOURS-categories`, `FR-HOURS-progress` | Applicable hour rules must be linked when finalized | Not yet linked | Not yet linked | Specified |
| `UC-HOUR-configure-requirements` | Requirement mapping needs clarification | Applicable hour rules must be linked when finalized | Not yet linked | Not yet linked | TBD |
| `UC-OBD-pair-device` | `FR-OBD2-connect`, `FR-OBD2-bluetooth` | None currently identified | Feasibility work required | Hardware verification required | Blocked |
| `UC-OBD-stream-vehicle-data` | `FR-OBD2-data`, `FR-OBD2-correlate` | None currently identified | Feasibility work required | Hardware verification required | Blocked |
| `UC-ADMIN-manage-drive-plans` | `FR-ADMIN-drive-plans` | None currently identified | Not yet linked | Not yet linked | Specified |
| `UC-ADMIN-manage-maneuvers-and-criteria` | `FR-ADMIN-maneuvers`, `FR-ADMIN-score-criteria` | `BR-dl40-maneuver-deductions` where applicable | Not yet linked | Not yet linked | Specified |

The following status values are used:

| Status | Meaning |
|---|---|
| **Specified** | Requirement exists, but implementation or verification has not yet been linked. |
| **In Progress** | Implementation work has begun. |
| **Implemented** | Code satisfying the requirement has been identified. |
| **Verified** | Implementation exists and a test or verification procedure confirms the required behavior. |
| **Blocked** | A dependency or unresolved issue prevents reliable implementation. |
| **TBD** | The requirement needs clarification before a trustworthy trace can be established. |

### Three ways the specification and the code drift apart

The backward edge asks which requirement authorized a given piece of code. On a real codebase the answer can be "none", and that answer generally appears in three forms.

| Shape | What you see | Repair |
|---|---|---|
| Specification right, code behind | A requirement is written down and the code does not enforce it | Conform the code |
| Code right, specification stale | The document describes behavior the approved system no longer has | Amend the document |
| Neither says anything | The code made a decision no document records | Decide it, then write it down |

**Specification right, code behind.** Suppose `FR-DL40-signatures` requires Drive Grader to capture the required signatures before producing the completed DL-40, but the implemented workflow allows the examiner to finish without them. Trace forward from the requirement and the implementation fails to satisfy the contract. The repair is to change the implementation and add verification showing that the signature requirement is enforced.

**Code right, specification stale.** Suppose a later client decision changes how a DL-40 grading item should behave. The team implements the approved change, but `software-requirements-specification.md` still describes the old behavior. In this case the code may be correct and the specification is stale. The requirement should be amended and the reason for that amendment recorded rather than changing documentation silently just to match the code.

**Neither says anything.** Suppose the OBD-II implementation receives no value for a turn signal and silently interprets the missing value as "turn signal not used." If no requirement states what missing telemetry means, that behavior was never authorized. The team must decide whether missing data means "not used," "unknown," "ignore the event," or something else, then record that decision in the appropriate requirement.

The third case becomes especially dangerous when AI generates implementation.

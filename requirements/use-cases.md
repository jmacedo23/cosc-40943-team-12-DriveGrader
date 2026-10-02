# Use Cases

**Project:** Drive Grader
**Team:** 12
**Client:** Eric Brown
**Version:** 0.3

---

_**How to use this template.** Instructions appear in italic square brackets. Fill in underneath them and leave them in place until the document is stable._

_**What a use case is.** One goal a user can accomplish with your system, written as the dialogue between the actor and the system, including what happens when it goes wrong. It is the unit of work in this course: one use case becomes one issue, one branch, one pull request, and one set of tests._

_**Why the use case and not the user story.** You will meet user stories in industry, and they are a good planning tool: "As a student, I want to submit my report so that I get credit." A story is deliberately under-specified, because it is a **placeholder for a conversation** that happens later, between people. That is exactly the wrong property when the thing building your code is an agent that will implement precisely what the specification says and never ask what you meant. Use stories to plan and prioritize. Build against use cases._

_The difference that matters is the parts a story does not have: preconditions, the step-by-step flow, and above all the **extensions**, which is where the failure paths live. Most defects your team ships this semester will be in a path nobody wrote down._

## Identifiers

_Use cases are identified as `UC-<AREA>-<slug>`, where the area code groups related functionality and the slug is coined from the goal: `UC-RUB-create-rubric`, `UC-WAR-manage-activities`, `UC-STU-invite-students`._

_Pick your own area codes from your project's feature areas, three or four letters each, and list them at the top of the Use Case List. Areas correspond to the `FEAT-*` entries in your [vision and scope](vision-and-scope.md), which is where use cases come from._

_**Never renumber, rename, or repoint an identifier.** Moving a use case between areas would change its identifier, so put it in the right area the first time, and if you get it wrong, leave it. An identifier is an address, not a description._

_Within one use case, `PRE-1`, `POST-1`, and the step numbers are local and may be renumbered freely, because nothing outside the use case cites them._

## Revision History

| Date | Version | Description | Author |
|---|---|---|---|
| 2026-09-11 | 0.1 | Initial use cases derived from the vision and scope feature list | Kaylynn Slaughter |
| 2026-09-25 | 0.2 | Rewrote Purpose/Scope and Use Case List against Client Meeting 1 (Eric Brown); replaced the template's placeholder example with real Drive Grader use cases (`UC-SESS-start-drive-session`, `UC-DL40-conduct-graded-test`, `UC-HOUR-track-progress`); dropped ANLZ/CONT/MON areas pending confirmation | Kaylynn Slaughter |
| 2026-09-30 | 0.3 | Added `ACCT`, `SYNC`, and `SETT` areas and re-added `MON` (now optional/low priority) against the team's 28-Oct backlog; fully specified `UC-ACCT-reset-password`, `UC-SYNC-sync-offline-data`, and `UC-ADMIN-manage-student-limit`; flagged backlog items that are not use cases (staging indicator, generic "UI/UX", Tailscale) and redirected them out of this document | Kaylynn Slaughter |

---

## 1. Introduction

### 1.1 Purpose

Drive Grader gives parents doing Texas Parent-Taught Driver Education a structured way to grade their teen's driving instead of "go that way, don't hit that cone" with no real criteria. It does this three ways, confirmed directly by the client: (1) real-time infraction logging against ~10 standard grading categories during a practice drive, (2) a digital version of the state's official DL-40 road-test grade sheet — reordered to match the actual test route instead of the sheet's fixed printed order — that ends in a signed, printed grade sheet, and (3) tracking progress toward the 44 hours (30 general + 7 instruction + 7 observation) Texas requires before testing. This document specifies those goals in enough detail that a developer knows what to build and a tester knows what to check.

### 1.2 Scope

Covers what the client confirmed as core: session tracking, real-time grading, the DL-40 digital grading mode, hour/requirement tracking, OBD-II vehicle data integration, and the existing admin panel (organizations, drive plans, maneuvers/score criteria). As of v0.3, also covers account/password management, offline/sync behavior, student-count limits, and dark mode, all pulled from the team's own 28-Oct backlog rather than the client meeting — treat these as team-identified needs, not client-confirmed requirements, until Eric signs off on them.

Still explicitly out of scope or unconfirmed: AI evaluation/comparison of sessions (client: "optional, not required — no specific use case identified"); instructional/how-to videos (from the original brief, never revisited). Live parent observation of an in-progress drive (`MON`) was dropped in v0.2 for the same reason but reappears in v0.3 because the team's own backlog lists it and Eric raised it at the 2026-09-29 meeting ("like tracking an Uber") — it remains explicitly **optional** and low priority, not promoted to a confirmed requirement.

**Not use cases — redirect elsewhere:** a few backlog rows are not goals a user accomplishes and shouldn't be forced into this document:
- *Staging Indicator* — a developer/QA-facing indicator of which environment the app is pointed at. Belongs in architecture/ops notes, not here.
- *UI/UX* and *Mobile app* — too broad to be a use case; "Mobile app" (PWA vs. Capacitor) is already tracked as an open issue under `UC-SESS-start-drive-session` below.
- *Tailscale* — team VPN access to the staging environment; infra, not a user-facing feature.

**Open mapping issue:** these areas are written directly from the client meeting and the team backlog; they have not yet been reconciled against a formal `FEAT-*` list in vision-and-scope.md. Confirm that mapping before treating this section as final.

---

## 2. Use Case Template

_[The field definitions. Every use case below uses exactly these fields, in this order.]_

**UC ID and Name.** _The identifier plus a concise name stating the value this use case provides to a user. Begin with an action verb, followed by an object: "Create a rubric", not "Rubric creation" and not "Rubric management", which is a feature, not a goal._

**Created By** and **Date Created.** _Who wrote it, and when._

**Primary and Secondary Actors.** _An actor is a person or other entity outside the system that interacts with it. The primary actor initiates this use case; secondary actors participate in completing it. Actors usually correspond to the user classes you identified in the vision and scope._

**Trigger.** _The business event, system event, or user action that starts the use case. The trigger tells the system to begin testing the preconditions._

**Description.** _A brief statement of the reason for and the outcome of this use case._

**Preconditions.** _What must already be true before this use case can start. **The system must be able to test each precondition**, which is what separates a precondition from a hope. Label them `PRE-1`, `PRE-2`. Example: PRE-1. The user's identity has been authenticated._

**Postconditions.** _The state of the system at successful conclusion. Label them `POST-1`, `POST-2`. Example: POST-1. The price of the item in the database has been updated with the new value._

**Main Success Scenario.** _The actor's actions and the system's responses under normal, expected conditions, as a numbered list that alternates between the two and ends by accomplishing the goal in the name. Write "The system validates..." not "The system will validate..."; use cases are written in the present tense._

**Extensions.** _Where the real work is. Two kinds, both numbered relative to the step they branch from:_

- _**Alternative flows**, other ways the use case can still succeed. Number them `4a`, `4b` for branches from step 4, with their own sub-steps `4a1`, `4a2`. Say where the flow branches off and, if it does, where it rejoins._
- _**Exceptions**, anticipated error conditions and how the system responds. Numbered the same way._

_**A use case with no extensions is not finished.** For every step, ask: what if the input is invalid, the thing is not found, the user cancels, the user is not allowed, or the external system is down? An agent building from a flow with no failure paths will invent the error handling, and you will not find out until a demo._

**Priority.** _Relative priority of implementing this. Use the same scheme across all your use cases._

**Frequency of Use.** _Roughly how often this is performed, per an appropriate unit of time. An early indicator of load, concurrency, and transaction volume, and it is the field that tells your architecture which use cases matter._

**Business Rules.** _The `BR-*` identifiers that govern this use case. **Identifiers only, never the rule's text**, so the rule has one home in [business-rules.md](business-rules.md) and cannot go stale here._

**Associated Information.** _Everything a developer needs that is not a step: the data fields and their validation rules, quality attributes that apply, display and sort strategies, and what happens if execution fails for a systemic reason such as a network timeout. If the use case makes a durable change, say whether a failure rolls it back, completes it, or leaves it partially done._

_Data fields are specified as a table:_

| Property name | Data type | Validation rule | Security or access concerns | Glossary reference |
|---|---|---|---|---|
| _[field]_ | _[type]_ | _[required, format, range]_ | _[who may see or set it]_ | _[term]_ |

**Related Use Cases.** _Other use cases this one invokes or is invoked by, by identifier and name._

**Assumptions.** _Anything assumed about this use case or how it executes._

**Open Issues.** _What you do not know yet. Mirror it into [OPEN-ISSUES.md](OPEN-ISSUES.md) so it is visible in one place._

---

## 3. Use Case List

| Area code | Feature area | Use cases |
|---|---|---|
| ACCT | Account & Authentication | `UC-ACCT-reset-password`, `UC-ACCT-create-account`, `UC-ACCT-login` |
| SESS | Drive Session Management | `UC-SESS-start-drive-session`, `UC-SESS-end-drive-session`, `UC-SESS-view-session-history`, `UC-SESS-filter-session-history`, `UC-SESS-select-map-layer` |
| GRAD | Real-Time Infraction Grading | `UC-GRAD-log-infraction`, `UC-GRAD-review-infraction-log` |
| DL40 | Digital DL-40 Grade Sheet | `UC-DL40-configure-checklist`, `UC-DL40-conduct-graded-test`, `UC-DL40-capture-signatures-and-print`, `UC-DL40-suggest-route` |
| HOUR | Hour & Requirement Tracking | `UC-HOUR-track-progress`, `UC-HOUR-configure-requirements` |
| OBD | OBD-II / Vehicle Data Integration | `UC-OBD-pair-device`, `UC-OBD-stream-vehicle-data`, `UC-OBD-show-connection-status` |
| SYNC | Offline & Connectivity | `UC-SYNC-sync-offline-data` |
| ADMIN | Organization & Drive Plan Administration | `UC-ADMIN-manage-drive-plans`, `UC-ADMIN-manage-maneuvers-and-criteria`, `UC-ADMIN-manage-student-limit` |
| MON | Live Parent Observation (optional) | `UC-MON-observe-live-session` |
| SETT | App Settings | `UC-SETT-toggle-dark-mode` |

**Note on OBD:** four Bluetooth OBD-II units are already purchased, but as of the client meeting none had been tested. Whether CAN-bus data (turn signal, brake light) is exposed at all, and whether this works on EVs, are open questions, not settled requirements. Treat `UC-OBD-*` as spikes until that's resolved — don't build `UC-GRAD` or `UC-DL40` features that depend on OBD data being available.

**Note on MON:** the backlog item is explicitly "Optional: live track your kid's drive score," and Eric described it at the 2026-09-29 meeting as a product idea rather than a requirement. Keep it at low priority and do not let it pull scope from `SESS`, `GRAD`, or `DL40`, which are client-confirmed.

**Not yet specified in Section 4:** `UC-ACCT-create-account`, `UC-ACCT-login`, `UC-SESS-end-drive-session`, `UC-SESS-view-session-history`, `UC-SESS-filter-session-history`, `UC-SESS-select-map-layer`, both `UC-GRAD-*`, `UC-DL40-configure-checklist`, `UC-DL40-capture-signatures-and-print`, `UC-DL40-suggest-route`, `UC-HOUR-configure-requirements`, `UC-OBD-pair-device`, `UC-OBD-stream-vehicle-data`, `UC-OBD-show-connection-status`, both `UC-ADMIN-*` drive-plan/maneuver ones, `UC-MON-observe-live-session`, `UC-SETT-toggle-dark-mode`. This is a known gap, not an oversight — see "What's left before this document is stable" at the end for what to draft next.

---

## 4. Use Cases

## ACCT — Account & Authentication

### UC-ACCT-reset-password: The user resets a forgotten password

**UC ID and Name:** `UC-ACCT-reset-password`: Reset a forgotten password
**Created By:** Team 12
**Date Created:** 2026-09-30
**Primary Actor:** parent, instructor, or org admin (any account holder)
**Secondary Actors:** email service (sends the reset link)
**Trigger:** The user selects "Forgot Password?" on the login screen.
**Description:** A user who cannot remember their password wants to regain access to their account by resetting it through a verified channel, without needing an admin to intervene.

**Preconditions:**

- PRE-1. An account exists with the email address the user provides.

**Postconditions:**

- POST-1. The account's password is updated to a new value the user has chosen.
- POST-2. All existing sessions for that account are invalidated, requiring re-login.

**Main Success Scenario:**

1. The user selects "Forgot Password?" and enters the email address on file.
2. The system generates a single-use, time-limited reset token and emails a reset link to that address.
3. The user opens the link and enters a new password, confirmed by re-entry.
4. The system validates the new password against the "Password rules" in Associated Information and the token's validity.
5. The system updates the stored password, invalidates the token, and invalidates existing sessions.
6. The system confirms the reset and returns the user to login.
7. Use case ends.

**Extensions:**

- **1a. The entered email does not match any account:**
    - 1a1. The system shows a generic confirmation message ("If that email exists, a reset link has been sent") rather than revealing whether the account exists, to avoid leaking account existence.
- **3a. The reset link has expired or was already used:**
    - 3a1. The system rejects the attempt and offers to send a new reset link.
- **4a. The new password fails validation (too short, doesn't meet rules):**
    - 4a1. The system alerts the user to the specific rule violated and returns to step 3.
- **4b. The two password entries don't match:**
    - 4b1. The system alerts the user and returns to step 3.

**Priority:** High
**Frequency of Use:** Infrequent per user, but needed from day one since it's the only way back into a locked-out account.
**Business Rules:** `BR-password-complexity`, `BR-reset-token-expiry`

**Associated Information:**

Password rules:

| Property name | Data type | Validation rule | Security or access concerns | Glossary reference |
|---|---|---|---|---|
| new password | String | Per `BR-password-complexity`; must match confirmation field | Never logged or stored in plaintext | Password |
| reset token | String | Single-use, expires per `BR-reset-token-expiry` | Sent only to the verified email on file | Reset Token |

Failure handling: a failed or abandoned reset leaves the old password active and unchanged; nothing is updated until step 5 succeeds atomically.

**Related Use Cases:** `UC-ACCT-login`; `UC-ACCT-create-account`.
**Assumptions:** The system has a working transactional email channel; this is not yet confirmed as part of the current stack and should be checked against what the client's existing admin panel already uses.
**Open Issues:** Is there a fallback for a user with no access to their email (e.g., an org-admin-assisted reset)? Not raised by the client or the backlog item as written.

---

## SYNC — Offline & Connectivity

### UC-SYNC-sync-offline-data: The app syncs session data recorded without internet access

**UC ID and Name:** `UC-SYNC-sync-offline-data`: Sync offline session data
**Created By:** Team 12
**Date Created:** 2026-09-30
**Primary Actor:** system (triggered automatically on connectivity return)
**Secondary Actors:** parent (sees sync status)
**Trigger:** The device regains internet connectivity while one or more drive sessions have unsynced local data.
**Description:** Since a drive takes place in a moving vehicle, connectivity can drop or never exist for the whole session; the app needs to keep recording locally and reconcile with the backend once a connection is available, so no session data is lost and the parent isn't blocked from driving.

**Preconditions:**

- PRE-1. At least one drive session has data recorded locally that has not yet been confirmed as saved to the backend.
- PRE-2. The device has a usable internet connection.

**Postconditions:**

- POST-1. All previously unsynced session data (route, infractions, OBD readings) is persisted to the backend.
- POST-2. The locally queued copy is marked synced and the parent sees an up-to-date sync status.

**Main Success Scenario:**

1. The system detects a usable internet connection while unsynced local session data exists.
2. The system uploads the queued data to the backend in the order the sessions were recorded.
3. The backend confirms receipt of each session's data.
4. The system marks each confirmed session as synced and clears it from the local queue.
5. The system updates the sync status shown to the parent (e.g., "All drives synced").
6. Use case ends.

**Extensions:**

- **2a. The connection drops again mid-sync:**
    - 2a1. The system stops uploading, leaves already-confirmed sessions marked synced, and leaves the rest queued for the next connectivity event.
- **3a. The backend rejects a session's data (e.g., validation failure, conflicting record):**
    - 3a1. The system keeps that session queued and flagged as "sync failed" rather than silently dropping it, and surfaces this to the parent rather than failing silently.
- **3b. Two devices recorded overlapping or duplicate data for the same session (e.g., app reinstalled mid-drive):**
    - 3b1. The system treats sessions by their locally generated unique ID, so re-sync of an already-synced session is a no-op rather than a duplicate.

**Priority:** High
**Frequency of Use:** Every drive conducted with limited or no cellular connectivity — likely common, since practice drives happen in all kinds of areas.
**Business Rules:** none identified yet

**Associated Information:**

| Property name | Data type | Validation rule | Security or access concerns | Glossary reference |
|---|---|---|---|---|
| session local ID | UUID | Generated at session start, stable for the session's life | Used to deduplicate on sync; not sensitive | Drive Session |
| sync status | Enum | pending, syncing, synced, failed | Visible to the parent for that session only | Sync Status |

Failure handling: a session's data is never deleted locally until the backend has confirmed receipt; a failed sync leaves the local copy intact and retries on the next connectivity event rather than being treated as a dead letter.

**Related Use Cases:** `UC-SESS-start-drive-session` (the thing being synced); `UC-OBD-show-connection-status` (a related but distinct status indicator, for the OBD-II Bluetooth link rather than internet connectivity).
**Assumptions:** The device can reliably detect "has internet" vs. "has no internet" distinctly from "has GPS" — these are different capabilities and shouldn't be conflated in the implementation.
**Open Issues:** None from the client meeting — this entire area comes from the team's own backlog ("Internet Connection," due 28-Oct), not a client requirement. Confirm with Eric whether offline-first behavior is expected or whether a simpler "drive requires connectivity" constraint is acceptable for the MVP.

---

## ADMIN — Organization & Drive Plan Administration

### UC-ADMIN-manage-student-limit: The org admin manages the account's student limit

**UC ID and Name:** `UC-ADMIN-manage-student-limit`: Manage the account's student limit
**Created By:** Team 12
**Date Created:** 2026-09-30
**Primary Actor:** org admin
**Secondary Actors:** none
**Trigger:** The org admin opens account settings and views or changes the student limit.
**Description:** The org admin wants to see how many student/driver profiles their account is allowed and currently uses, and adjust that limit, so the org doesn't silently hit a cap mid-semester or get charged for a tier they don't need.

**Preconditions:**

- PRE-1. The org admin is logged in and authenticated with admin privileges for the org.

**Postconditions:**

- POST-1. The org's configured student limit reflects the admin's change, if any was made.

**Main Success Scenario:**

1. The org admin opens account settings.
2. The system displays the current student limit and the number of student/driver profiles currently in use.
3. The org admin requests a change to the limit.
4. The system validates the requested limit against the "Student limit rules" in Associated Information.
5. The system updates the org's student limit.
6. The system confirms the change.
7. Use case ends.

**Extensions:**

- **4a. The requested limit is below the number of currently active student profiles:**
    - 4a1. The system alerts the admin that existing profiles would exceed the new limit and does not apply the change until the admin either removes profiles or picks a higher limit.
- **4b. The requested limit exceeds what the org's plan/tier allows:**
    - 4b1. The system alerts the admin and, if an upgrade path exists, offers it; otherwise the limit is capped at the plan maximum.

**Priority:** Medium
**Frequency of Use:** Rare — set at onboarding, revisited occasionally as an org grows.
**Business Rules:** `BR-max-students-per-account`

**Associated Information:**

Student limit rules:

| Property name | Data type | Validation rule | Security or access concerns | Glossary reference |
|---|---|---|---|---|
| student limit | Integer | Must be ≥ current active student count; capped per `BR-max-students-per-account` | Admin-only; not visible to parents or students | Student Limit |

Failure handling: a rejected change leaves the existing limit in place; nothing is left in a partially-applied state.

**Related Use Cases:** `UC-ACCT-create-account` (profile creation should be blocked at the limit, not just reported after the fact).
**Assumptions:** The limit is per-organization, not per-parent — unconfirmed, since this entire item comes from the team backlog rather than the client meeting.
**Open Issues:** Is this a hard technical limit, a billing tier boundary, or both? The backlog item ("# of students limit") doesn't say, and it hasn't been raised with Eric yet.

---

## SESS — Drive Session Management

### UC-SESS-start-drive-session: The parent starts a drive session

**UC ID and Name:** `UC-SESS-start-drive-session`: Start a drive session
**Created By:** Team 12
**Date Created:** 2026-09-25
**Primary Actor:** parent (or instructor)
**Secondary Actors:** OBD-II device (optional, real or simulated); GPS provider (real or simulated)
**Trigger:** The parent creates a new drive and taps "Begin Drive."
**Description:** The parent wants to start a named drive session for a specific driver profile, with GPS and optional OBD-II tracking running, so the drive can be graded in real time and reviewed afterward.

**Preconditions:**

- PRE-1. The user is logged in and authenticated.
- PRE-2. A driver profile exists to associate with the session (e.g., "Test Driver").

**Postconditions:**

- POST-1. A drive session record exists in "in progress" state, associated with the driver profile and a drive plan (e.g., "Basic Skills").
- POST-2. GPS route data (real or simulated) and, if selected, OBD-II data begin logging against the session.

**Main Success Scenario:**

1. The parent creates a new drive, naming the drive plan and selecting the driver profile.
2. The system presents session settings (GPS source, OBD-II device) with sensible defaults.
3. The parent accepts the defaults or selects a simulated GPS and/or simulated OBD-II device for testing, or a real device if in-vehicle.
4. The parent taps "Begin Drive."
5. The system starts logging GPS location, speed, and route, plus accelerometer data (hard braking, hard turns, G-forces).
6. The system displays the live in-progress view so the parent can grade the drive (see `UC-GRAD-log-infraction`) while it runs.
7. Use case ends (session continues until `UC-SESS-end-drive-session`).

**Extensions:**

- **3a. A real OBD-II device is selected but fails to pair:**
    - 3a1. The system alerts the parent that the device is unavailable.
    - 3a2. The parent may retry pairing, switch to simulated/no OBD-II, or proceed without it.
- **4a. GPS lock cannot be acquired (real GPS selected, no simulated fallback):**
    - 4a1. The system alerts the parent and offers to retry or switch to simulated GPS.
- **5a. The app is backgrounded and the OS suspends location/Bluetooth tracking mid-session:**
    - 5a1. The system resumes tracking on foreground return and marks the gap as an interruption in the session log.
- **5b. The device has no internet connectivity when the session starts:**
    - 5b1. The system records locally and queues the session for `UC-SYNC-sync-offline-data` once connectivity returns; the parent is not blocked from starting or continuing the drive.

**Priority:** High
**Frequency of Use:** Every practice drive or road test, potentially several times per week per driver.
**Business Rules:** `BR-parent-own-students`, `BR-org-roster`, `BR-drive-active-student`

**Associated Information:**

| Property name | Data type | Validation rule | Security or access concerns | Glossary reference |
|---|---|---|---|---|
| driver profile | Reference | Required | Scoped to the parent's/org's account | Driver Profile |
| drive plan | Reference | Required, defaults to a standard plan | n/a | Drive Plan |
| gps source | Enum | real, simulated | n/a | GPS Source |
| obd device | Enum/Reference | none, simulated, or paired real device | Device data is vehicle-derived, not personal | OBD-II Device |

Failure handling: session creation is all-or-nothing; a mid-session interruption is logged, not discarded — partial data captured before the interruption is retained.

**Related Use Cases:** `UC-GRAD-log-infraction`; `UC-SESS-end-drive-session`; `UC-HOUR-track-progress` (a completed session should count toward hour requirements); `UC-SYNC-sync-offline-data` (see 5b).
**Assumptions:** The teen driver does not need their own login — the client confirmed the parent inputs all grading data themselves.
**Open Issues:** Whether a PWA gives sufficient Bluetooth/GPS access for real OBD-II devices, or whether the team must move to a Capacitor-wrapped native build — the client flagged this as something the team needs to determine, not something already decided. (This is also where the backlog's "Mobile app" item belongs, rather than as its own use case.)

---

## DL40 — Digital DL-40 Grade Sheet

### UC-DL40-conduct-graded-test: The instructor conducts a graded road test on the digital DL-40

**UC ID and Name:** `UC-DL40-conduct-graded-test`: Conduct a graded road test
**Created By:** Team 12
**Date Created:** 2026-09-25
**Primary Actor:** instructor/examiner (or parent, for a practice road test)
**Secondary Actors:** driver, parent/guardian (signs at end)
**Trigger:** The instructor selects a driver and taps to begin a DL-40 graded test.
**Description:** The instructor wants to grade a road test electronically, tapping each required maneuver as it happens along the actual route driven — rather than hunting for it on a fixed-order paper form — so that a completed, signed grade sheet can be printed at the end. This is the feature the client identified as the reason Drive Grader exists.

**Preconditions:**

- PRE-1. The instructor is logged in and authenticated.
- PRE-2. The DL-40 checklist has been reordered/configured to match the planned route (see `UC-DL40-configure-checklist`).
- PRE-3. A driver profile and parent/guardian record are available to attach to this test.

**Postconditions:**

- POST-1. Every maneuver on the DL-40 checklist has a recorded grade outcome (pass/fail/notes) tied to a timestamp.
- POST-2. A completed DL-40 grade sheet exists with driver and parent/guardian signatures, ready to print.

**Main Success Scenario:**

1. The instructor selects the driver and the pre-configured, route-ordered DL-40 checklist.
2. The instructor selects the parent/guardian who will sign alongside the driver.
3. The instructor begins the test; the system displays the reordered checklist item by item.
4. As each maneuver occurs (parking, merge, lane change, approach-to-corner, traffic signal, traffic sign, left turn, right turn, backing, etc.), the instructor taps the matching item and records the grade.
5. The system timestamps and records each graded item against the session.
6. The instructor completes the last maneuver on the checklist.
7. The system presents the completed grade sheet for driver and parent/guardian signature (see `UC-DL40-capture-signatures-and-print`).
8. Use case ends.

**Extensions:**

- **4a. A maneuver happens that isn't next on the reordered checklist (route deviated from plan):**
    - 4a1. The instructor searches or scrolls to the correct item out of sequence and grades it there.
    - 4a2. The system still records the correct timestamp and item; checklist order is a navigation aid, not a constraint on grading order.
- **4b. The instructor taps the wrong item by mistake:**
    - 4b1. The instructor may undo/correct the most recent grading action before moving on.
- **6a. The test is ended before every checklist item is graded (e.g., test aborted):**
    - 6a1. The system marks ungraded items as "not administered" rather than as a pass or fail.
    - 6a2. The grade sheet indicates the test was incomplete.

**Priority:** High
**Frequency of Use:** Once per official road test; lower volume than practice-drive grading but the feature the client most wants to see working.
**Business Rules:** `BR-dl40-official-form`, `BR-dl40-electronic-grading`, `BR-dl40-signatures`, `BR-road-test-route-maneuvers`, `BR-org-roster`

**Associated Information:**

| Property name | Data type | Validation rule | Security or access concerns | Glossary reference |
|---|---|---|---|---|
| checklist order | Ordered list of references | Must contain every required DL-40 item exactly once | Instructor-configurable per route | DL-40 Checklist |
| grade outcome | Enum | pass, fail, not administered | Part of an official test record | Grade Outcome |
| signatures | Image/binary | Required before print (see `UC-DL40-capture-signatures-and-print`) | Signature data is sensitive; access scoped to the org | Signature |

Failure handling: grade sheet is built incrementally as each item is tapped; a crash mid-test does not lose already-graded items, but the session must be resumed or explicitly marked incomplete rather than silently left open.

**Related Use Cases:** `UC-DL40-configure-checklist` (must happen first); `UC-DL40-capture-signatures-and-print` (happens last); `UC-DL40-suggest-route` (an upstream aid, not a dependency); `UC-SESS-start-drive-session` (a DL-40 test runs inside a session).
**Assumptions:** The required maneuver set (three left turns, three right turns, two stop signs, two lights, two approach-to-corners, parallel park, reverse, etc.) is fixed by the state form and not something the app invents per route.
**Open Issues:** None from the meeting — this is the most concretely specified feature the client described. Confirm the exact full DL-40 item list and required counts against the actual state form before building the checklist data model.

---

## HOUR — Hour & Requirement Tracking

### UC-HOUR-track-progress: The parent views progress toward required hours

**UC ID and Name:** `UC-HOUR-track-progress`: Track progress toward required hours
**Created By:** Team 12
**Date Created:** 2026-09-25
**Primary Actor:** parent
**Secondary Actors:** none
**Trigger:** The parent opens the driver's progress view.
**Description:** The parent wants to see how many of the required hours their teen has completed — general driving, behind-the-wheel instruction, and observation — so they know how close the teen is to meeting Texas's 44-hour requirement before testing.

**Preconditions:**

- PRE-1. The parent is logged in and authenticated.
- PRE-2. At least one completed drive session exists for the driver, or the driver has zero logged hours (still a valid, empty state).

**Postconditions:**

- POST-1. The parent sees hours completed and hours remaining in each of the three categories (general, instruction, observation), against the configured target for each.

**Main Success Scenario:**

1. The parent opens the driver's progress view.
2. The system totals logged session time by category (general/instruction/observation) for that driver.
3. The system displays completed hours, remaining hours, and percentage complete per category, plus the combined total against the 44-hour requirement.
4. Use case ends.

**Extensions:**

- **2a. A session's category was never set or is ambiguous:**
    - 2a1. The system counts it toward "general" by default and flags it so the parent can recategorize it.

**Priority:** High
**Frequency of Use:** Frequent — checked after most sessions.
**Business Rules:** `BR-ptde-required-hours`, `BR-ptde-total-hours`, `BR-parent-own-students`

**Associated Information:**

| Property name | Data type | Validation rule | Security or access concerns | Glossary reference |
|---|---|---|---|---|
| session category | Enum | general, instruction, observation | n/a | Session Category |
| target hours per category | Number | Configurable; defaults per `BR-ptde-required-hours` | Org-level, not driver-level, unless overridden | Hour Requirement |

Failure handling: this is a read-only aggregation; a failure to compute it should show a clear error rather than a wrong number, and never blocks the ability to start a new session.

**Related Use Cases:** `UC-SESS-end-drive-session` (writes the hours this view reads); `UC-HOUR-configure-requirements` (sets the targets referenced here). Note: the backlog's "Progress Meter" item is this use case's UI, not a separate use case.
**Assumptions:** None beyond `BR-ptde-required-hours` and `BR-ptde-total-hours` as stated by the client.
**Open Issues:** The client suggested modeling target hours as configurable variables per category rather than hardcoding 30/7/7 — confirm whether that configurability is org-level, state-level, or both, since Texas-specific rules may not generalize if the app is ever used outside Texas.

---

## Working these with your agent

_[Delegate: drafting the main success scenario once you have the trigger and the goal; proposing extensions you have not thought of, which it is genuinely good at; turning a filled-in use case into a first set of test cases; checking that every `BR-*` you cite exists in [business-rules.md](business-rules.md).]_

_Keep human: whether this is one use case or three, what the priority is, and whether an extension the agent proposed is a real path in your client's business or a generic one it has seen elsewhere. "The system handles concurrent edits" is a real requirement for some projects and invented complexity for others, and only you have met the client._

_The verification that catches the most: read the main success scenario aloud to someone who has not read the document, and stop wherever they ask a question. Every question is a missing step or a missing extension._

_**Checklist for each use case:** Does the name start with a verb? Can the system test every precondition? Does every step alternate actor and system? Is there at least one extension per step that can fail? Does every business rule appear as an identifier only? Could a tester write test cases from this without asking you anything?_

**What's left before this document is stable:**
- The use cases listed in Section 3 but not yet specified in Section 4 (see that list above) — use the six worked examples as the pattern, especially for extensions.
- `UC-DL40-suggest-route` and `UC-SESS-select-map-layer` come straight from the 28-Oct backlog and haven't been run past Eric yet — confirm before specifying them in full, so the team doesn't spend a write-up on something that gets cut. `UC-MON-observe-live-session` was raised by Eric on 2026-09-29 as an idea; confirm whether he wants it in the MVP before specifying it.
- `BR-password-complexity`, `BR-reset-token-expiry`, and `BR-max-students-per-account` are cited above but not yet defined in [business-rules.md](business-rules.md); add them there (the student cap is waiting on Eric to name N).
- Reconciling the Section 1.2 area list against a formal `FEAT-*` list in vision-and-scope.md.
- Confirming the exact DL-40 item list against the real state form.
- The three items flagged in Section 1.2 as not belonging in this document at all (staging indicator, generic UI/UX, Tailscale) should move to wherever the team tracks architecture/ops decisions, so they don't get lost just because they're off this list.

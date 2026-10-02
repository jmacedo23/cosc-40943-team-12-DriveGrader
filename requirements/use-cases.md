# Use Cases

**Project:** Drive Grader
**Team:** 12
**Client:** Eric Brown
**Version:** 0.4

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
| 2026-10-02 | 0.4 | Checked every specified use case against the client's Drive Tracker proof of concept and corrected flows that did not match it (DL-40 signatures are captured at drive start, DL-40 grading is per-aspect point deductions, GPS is required to start, simulation is platform-admin only, offline sync has no local ID); mapped every area and use case to the `FEAT-*` list in vision-and-scope; added `UC-ACCT-manage-student-roster` (fully specified) and listed `UC-SESS-edit-drive-grades`, `UC-SESS-delete-drive`, `UC-SESS-generate-drive-report`, `UC-GRAD-view-progress-history`, `UC-HOUR-export-hours-log`, `UC-SYNC-check-system-status`, `UC-SETT-install-app`, `UC-SETT-view-whats-new`; recorded FEATs with no use case yet | Kaylynn Slaughter |

---

## 1. Introduction

### 1.1 Purpose

Drive Grader gives parents doing Texas Parent-Taught Driver Education a structured way to grade their teen's driving instead of "go that way, don't hit that cone" with no real criteria. It does this three ways, confirmed directly by the client: (1) one-tap mistake logging against the maneuvers in a drive plan during a practice drive, (2) a digital version of the state's official DL-40 road-test grade sheet — ordered to match the actual test route instead of the sheet's fixed printed order — that ends in a signed, printed grade sheet, and (3) tracking progress toward the 44 hours Texas requires before testing. This document specifies those goals in enough detail that a developer knows what to build and a tester knows what to check.

### 1.2 Scope

Drive Grader extends the client's existing proof of concept, **Drive Tracker** ([odds-tcu-2026/drive-tracker](https://github.com/odds-tcu-2026/drive-tracker)). Where a use case describes something Drive Tracker already does, the flow below is written to match the code, and the Associated Information says what exists today and what has to change. Where a use case and the code disagree, the use case states the intended behavior and the gap is listed as an open issue.

Every area in Section 3 maps to one or more `FEAT-*` entries in [vision-and-scope.md](vision-and-scope.md). Use cases for the client's benchmark items (`FEAT-system-status-indicators`, `FEAT-obd-connection-management`, `FEAT-student-limits`, `FEAT-drive-reporting`, `FEAT-versioning-whats-new`, `FEAT-aerial-map-layer`, `FEAT-dark-mode`, `FEAT-pwa-install-prompts`) are client-requested, not team-invented. Two areas come from the team rather than the client: self-service password reset (`UC-ACCT-reset-password`) and the detailed offline-sync behavior (`UC-SYNC-sync-offline-data`, tracked as `OI-offline`).

Out of scope or deferred, per the release plan in vision-and-scope: `FEAT-ai-drive-analysis`, `FEAT-in-car-video`, `FEAT-fleet-tracking`, `FEAT-drive-scheduling`, `FEAT-reservation-integration`, and `FEAT-lesson-content` (content production is the client's work; Drive Tracker already lets a maneuver carry a video link). `FEAT-turn-signal-detection` is a feasibility study with a written finding, not a use case. `FEAT-live-drive-observation` (`MON`) stays listed as optional and low priority.

**Not use cases — tracked elsewhere:**
- *UI/UX* — too broad to be a use case; tracked as `OI-ui-improvements`.
- *Mobile app* — Drive Tracker already contains Capacitor Android and iOS projects, and its OBD screen states that iPhone Safari cannot open Bluetooth or USB adapters. A native build is therefore required for live OBD-II on iPhone; this is a platform constraint, recorded under `UC-SESS-start-drive-session`.
- *Tailscale* — team VPN access to the staging environment; infrastructure, not a user-facing feature.
- *Staging Indicator* — no longer a separate item: the client listed it as part of `FEAT-system-status-indicators`, so it is covered by `UC-SYNC-check-system-status`.

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

### 3.1 Areas

| Area code | Feature area | Features (vision-and-scope) |
|---|---|---|
| ACCT | Account & Authentication | `FEAT-accounts-and-organizations` |
| SESS | Drive Session Management | `FEAT-drive-tracking`, `FEAT-drive-review`, `FEAT-drive-reporting`, `FEAT-aerial-map-layer` |
| GRAD | Real-Time Infraction Grading | `FEAT-graded-practice-drive`, `FEAT-progress-history` |
| DL40 | Digital DL-40 Grade Sheet | `FEAT-dl40-road-test` |
| HOUR | Hour & Requirement Tracking | `FEAT-training-hours-log` |
| OBD | OBD-II / Vehicle Data Integration | `FEAT-vehicle-data`, `FEAT-obd-connection-management` |
| SYNC | Offline & Connectivity | `FEAT-system-status-indicators`; `OI-offline` |
| ADMIN | Organization & Drive Plan Administration | `FEAT-drive-plans`, `FEAT-student-limits` |
| MON | Live Parent Observation (optional) | `FEAT-live-drive-observation` |
| SETT | App Settings | `FEAT-dark-mode`, `FEAT-pwa-install-prompts`, `FEAT-versioning-whats-new` |

### 3.2 Use cases

"Drive Tracker today" says whether the client's proof of concept already does this: **Exists**, **Partial**, or **New**.

| Use case | Feature | Drive Tracker today | Spec status |
|---|---|---|---|
| `UC-ACCT-create-account` | `FEAT-accounts-and-organizations` | Exists (Register page) | Listed |
| `UC-ACCT-login` | `FEAT-accounts-and-organizations`, `FEAT-system-status-indicators` | Exists, with "API unavailable" banner | Listed |
| `UC-ACCT-reset-password` | `FEAT-accounts-and-organizations` | Partial (platform-admin reset only) | Specified |
| `UC-ACCT-manage-student-roster` | `FEAT-accounts-and-organizations` | Exists (Students page) | Specified |
| `UC-SESS-start-drive-session` | `FEAT-drive-tracking` | Exists | Specified |
| `UC-SESS-end-drive-session` | `FEAT-drive-tracking` | Exists | Listed |
| `UC-SESS-edit-drive-grades` | `FEAT-drive-review` | Exists ("Edit Grades" reopens a finished drive) | Listed |
| `UC-SESS-delete-drive` | `FEAT-drive-review` | Exists (soft delete) | Listed |
| `UC-SESS-view-session-history` | `FEAT-drive-review` | Exists (Dashboard, paged list) | Listed |
| `UC-SESS-filter-session-history` | `FEAT-drive-reporting` | New | Listed |
| `UC-SESS-generate-drive-report` | `FEAT-drive-reporting` | New | Listed |
| `UC-SESS-select-map-layer` | `FEAT-aerial-map-layer` | New (one OpenStreetMap street layer only) | Listed |
| `UC-GRAD-log-infraction` | `FEAT-graded-practice-drive` | Exists ("Parent Practice" flag mode) | Listed |
| `UC-GRAD-review-infraction-log` | `FEAT-drive-review` | Exists (Session Review page) | Listed |
| `UC-GRAD-view-progress-history` | `FEAT-progress-history` | Partial (API endpoint, no screen) | Listed |
| `UC-DL40-configure-checklist` | `FEAT-dl40-road-test`, `FEAT-drive-plans` | Exists (drive plan maneuver order) | Listed |
| `UC-DL40-conduct-graded-test` | `FEAT-dl40-road-test` | Exists | Specified |
| `UC-DL40-capture-signatures-and-print` | `FEAT-dl40-road-test` | Exists (signatures at start, PDF from review) | Listed |
| `UC-DL40-suggest-route` | none | New | Listed — no FEAT |
| `UC-HOUR-track-progress` | `FEAT-training-hours-log` | New | Specified |
| `UC-HOUR-configure-requirements` | `FEAT-training-hours-log` | New | Listed |
| `UC-HOUR-export-hours-log` | `FEAT-training-hours-log` | New | Listed |
| `UC-OBD-pair-device` | `FEAT-obd-connection-management` | Exists (Bluetooth LE, USB serial, simulator) | Listed |
| `UC-OBD-stream-vehicle-data` | `FEAT-vehicle-data` | Exists (speed, RPM, throttle; turn signal not available) | Listed |
| `UC-OBD-show-connection-status` | `FEAT-obd-connection-management` | Exists (Connected / Not connected badge) | Listed |
| `UC-SYNC-sync-offline-data` | `OI-offline` | Partial (device-storage queues) | Specified |
| `UC-SYNC-check-system-status` | `FEAT-system-status-indicators` | Partial (API-down banner on login only) | Listed |
| `UC-ADMIN-manage-drive-plans` | `FEAT-drive-plans` | Exists (Admin and SaaS Admin pages) | Listed |
| `UC-ADMIN-manage-maneuvers-and-criteria` | `FEAT-drive-plans` | Exists (Admin and SaaS Admin pages) | Listed |
| `UC-ADMIN-manage-student-limit` | `FEAT-student-limits` | New | Specified |
| `UC-MON-observe-live-session` | `FEAT-live-drive-observation` | New | Listed — optional |
| `UC-SETT-toggle-dark-mode` | `FEAT-dark-mode` | New | Listed |
| `UC-SETT-install-app` | `FEAT-pwa-install-prompts` | Partial (install gate component) | Listed |
| `UC-SETT-view-whats-new` | `FEAT-versioning-whats-new` | New | Listed |

### 3.3 Notes

**Note on OBD:** the team's adapter does not report turn-signal use (`RI-turn-signal-unavailable`), and the client said "track what you can." Drive Tracker already reads speed, engine RPM, and throttle over Bluetooth LE or USB serial from an ELM327 adapter. An OBD connection is never required to start or grade a drive. Do not build `GRAD` or `DL40` behavior that depends on OBD data being present.

**Note on MON:** the backlog item is explicitly "Optional: live track your kid's drive score," and Eric described it at the 2026-09-29 meeting as a product idea rather than a requirement. Vision-and-scope defers it (`OI-live-observation`) and flags the privacy exposure of a minor's live location (`RI-minor-location-data`). Keep it at low priority and do not let it pull scope from `SESS`, `GRAD`, or `DL40`.

**Note on access control:** in Drive Tracker today, the endpoints that read one drive's details, grades, route, motion events, and OBD samples (`GET /sessions/:id` and its sub-resources, `GET /grades/session/:id`) and the grade-delete endpoints check that the caller is logged in but not that the drive belongs to them. That conflicts with `BR-parent-own-students` and `BR-org-roster`. Every `SESS` and `GRAD` use case that reads or changes an existing drive must include an "out-of-scope drive" exception when it is specified.

**Note on registration:** Drive Tracker's Register page lets a new user choose any role, including examiner and instructor. Whether self-registration may grant staff roles must be settled when `UC-ACCT-create-account` is specified.

**Features with no use case yet** (deferred per vision-and-scope's release plan, so not drafted): `FEAT-turn-signal-detection`, `FEAT-ai-drive-analysis`, `FEAT-in-car-video`, `FEAT-fleet-tracking`, `FEAT-drive-scheduling`, `FEAT-reservation-integration`, `FEAT-lesson-content`. Drive Tracker already has a partial reservation integration (today's appointments preselect a roster student, and staff can sync the roster from an external system); it appears below as extensions of `UC-SESS-start-drive-session` and `UC-ACCT-manage-student-roster`, not as its own use case.

---

## 4. Use Cases

## ACCT — Account & Authentication

### UC-ACCT-reset-password: The user resets a forgotten password

**UC ID and Name:** `UC-ACCT-reset-password`: Reset a forgotten password
**Created By:** Team 12
**Date Created:** 2026-09-30
**Primary Actor:** any account holder (parent, instructor, examiner, or student with a login)
**Secondary Actors:** email service (sends the reset link); platform administrator (assisted reset, extension 1b)
**Trigger:** The user selects "Forgot Password?" on the login screen.
**Description:** A user who cannot remember their password wants to regain access to their account through a verified channel, without waiting for an administrator.

**Preconditions:**

- PRE-1. The login screen is reachable and the API responds to its health check.

**Postconditions:**

- POST-1. The account's stored password hash is replaced with a hash of the new password the user chose.
- POST-2. Every login token issued to that account before the reset is rejected on its next use.
- POST-3. The reset token is spent and cannot be used again.

**Main Success Scenario:**

1. The user selects "Forgot Password?" and enters their email address.
2. The system shows the same confirmation message whether or not an active account has that email ("If that email exists, a reset link has been sent").
3. If an active account exists, the system generates a single-use, time-limited reset token and emails a reset link to that address.
4. The user opens the link and enters a new password twice.
5. The system validates the token and the new password against the "Password rules" in Associated Information.
6. The system stores the new password hash, spends the token, and invalidates the account's earlier login tokens, in one transaction.
7. The system confirms the reset and returns the user to the login screen.
8. Use case ends.

**Extensions:**

- **1a. The entered text is not a valid email address:**
    - 1a1. The system shows a format error and stays on step 1.
- **1b. The user has no access to the email on file:**
    - 1b1. The user contacts the school. A platform administrator sets a temporary password from the SaaS admin screen (this already exists in Drive Tracker).
    - 1b2. Use case ends; the user logs in with the temporary password and changes it.
- **2a. The email belongs to no account, or to a deactivated or deleted account:**
    - 2a1. The system shows the same message as step 2 and sends nothing. A reset never reactivates a deactivated account.
- **2b. The same email has requested several resets in a short period:**
    - 2b1. The system shows the same message as step 2 but sends no further emails until the rate limit window passes.
- **3a. The email service is unavailable:**
    - 3a1. The system logs the failure without the user's email in the log, still shows the step 2 message, and changes nothing.
- **5a. The reset link has expired or was already used:**
    - 5a1. The system rejects it and offers to send a new link (returns to step 1).
- **5b. The new password breaks a password rule:**
    - 5b1. The system names the rule that failed and returns to step 4.
- **5c. The two password entries don't match:**
    - 5c1. The system alerts the user and returns to step 4.
- **6a. The update fails partway (database error):**
    - 6a1. The transaction rolls back; the old password and the token both stay valid, and the system asks the user to try again.

**Priority:** High
**Frequency of Use:** Infrequent per user, but it is the only self-service way back into a locked-out account.
**Business Rules:** `BR-password-complexity`, `BR-reset-token-expiry`

**Associated Information:**

Password rules:

| Property name | Data type | Validation rule | Security or access concerns | Glossary reference |
|---|---|---|---|---|
| email | String | Valid email format | Never echoed back in a way that reveals whether an account exists | Account |
| new password | String | Per `BR-password-complexity`; must match the confirmation field | Hashed with bcrypt, as Drive Tracker already does; never logged or stored in plaintext | Password |
| reset token | String | Single-use; expires per `BR-reset-token-expiry`; stored only as a hash | Sent only to the email on file | Reset Token |

Drive Tracker today: no self-service reset and no email library in the API. Registration and the admin reset both require at least 6 characters. A platform administrator can already set any user's password (`PUT /saas-admin/users/:id/password`). Login tokens are stateless JWTs with no revocation, so POST-2 needs a new mechanism (for example, a per-user token version checked on each request).

Failure handling: nothing changes until step 6 commits; an abandoned reset leaves the old password working.

**Related Use Cases:** `UC-ACCT-login`; `UC-ACCT-create-account`.
**Assumptions:** The team can add a transactional email provider to the client's stack.
**Open Issues:** Which email provider, and who pays for it? Should the platform-admin reset in extension 1b also invalidate existing logins? Today it does not.

---

### UC-ACCT-manage-student-roster: The user manages their student roster

**UC ID and Name:** `UC-ACCT-manage-student-roster`: Manage the student roster
**Created By:** Team 12
**Date Created:** 2026-10-02
**Primary Actor:** parent (personal roster), or examiner/instructor (their organization's roster)
**Secondary Actors:** the school's external student system (organization sync only)
**Trigger:** The user opens the Students page.
**Description:** A drive can only be started for a student on the grader's roster, so the user needs to add, correct, archive, and restore the students they supervise. Roster students are records, not logins: a teen does not need an account to be graded.

**Preconditions:**

- PRE-1. The user is logged in as a parent, examiner, or instructor.
- PRE-2. An examiner or instructor belongs to an organization.

**Postconditions:**

- POST-1. The roster entry is created, updated, archived, or restored as requested, within the user's own scope only.
- POST-2. Drives already recorded for an archived student remain in history unchanged.

**Main Success Scenario:**

1. The user opens Students.
2. The system lists the active students in the user's scope: a parent sees their personal roster; an examiner or instructor sees their organization's roster.
3. The user selects "Add student" and enters the student's details.
4. The system validates the details against the "Roster fields" in Associated Information and checks that the student is not already on the roster.
5. The system saves the student, who can be selected for a new drive immediately.
6. Use case ends.

**Extensions:**

- **2a. The roster is empty:**
    - 2a1. The system shows an empty state with an "Add student" action (continue at step 3).
- **2b. The user searches the roster:**
    - 2b1. The system filters the list by name or permit/license number within the user's scope only, never across organizations.
- **3a. The user edits an existing student instead:**
    - 3a1. The user changes fields on an existing entry; continue at step 4.
- **3b. The user archives a student:**
    - 3b1. The system asks for confirmation.
    - 3b2. On confirmation, the system hides the student from active lists and from drive selection without deleting it or its drives.
- **3c. The user restores an archived student:**
    - 3c1. The system returns the student to the active list, after the duplicate check in step 4.
- **3d. An examiner or instructor syncs the roster from the school's external system:**
    - 3d1. The system fetches students from the integration and adds or updates them by their stable external ID.
    - 3d2. The system skips rows with no external ID or no name and reports added, updated, and skipped counts.
    - 3d3. If the integration fails, the system keeps the current roster unchanged and reports the failure. Integration data never overwrites a parent's personal roster.
- **4a. A required field is missing or a value is invalid (e.g., bad email or date):**
    - 4a1. The system names the field and stays on the form.
- **4b. The student duplicates an existing roster entry:**
    - 4b1. The system rejects the save and points to the existing entry.
- **4c. The roster is at its student limit (see `UC-ADMIN-manage-student-limit`):**
    - 4c1. The system blocks the add or restore, explains the limit, and saves nothing.
- **5a. The user tries to open or change a student outside their scope (e.g., by editing a URL):**
    - 5a1. The server rejects the request; nothing is revealed about that student.

**Priority:** High — a precondition of every drive.
**Frequency of Use:** A few times per family at setup; regularly for school staff as students enroll.
**Business Rules:** `BR-parent-own-students`, `BR-parent-multiple-students`, `BR-org-roster`, `BR-platform-admin-settings`, `BR-roster-no-hard-delete`, `BR-max-students-per-account`

**Associated Information:**

Roster fields:

| Property name | Data type | Validation rule | Security or access concerns | Glossary reference |
|---|---|---|---|---|
| first name, last name | String | Required | Minor's personal data; visible only within scope | Roster Student |
| middle name, suffix | String | Optional | Same as above | Roster Student |
| date of birth | Date | Optional; valid date | Same as above; printed on the DL-40 | Roster Student |
| permit or license number | String | Optional; up to 40 characters; used for duplicate checks and appointment matching | Same as above; printed on the DL-40 | Permit Number |
| email | String | Optional; valid email format | Same as above | Roster Student |
| school name | String | Optional; up to 200 characters | Printed on the DL-40 | Roster Student |
| external student ID | String | Set only by organization sync; unique per organization | Not editable by users | External Student ID |

Drive Tracker today: implemented as specified in the client's approved "Managed Student Rosters" spec, with server-side scope checks and database-backed tests. No student limit exists yet (extension 4c is new).

Failure handling: each save is a single write; a sync is atomic, so a failed sync leaves the roster exactly as it was.

**Related Use Cases:** `UC-SESS-start-drive-session` (needs an active roster student); `UC-ADMIN-manage-student-limit` (sets the limit in 4c).
**Assumptions:** Archived students do not count toward the student limit; unconfirmed.
**Open Issues:** Should archived students count toward the limit? Does a parent need to link a student's login (Drive Tracker's join-code flow), or is that flow retired?

---

## SYNC — Offline & Connectivity

### UC-SYNC-sync-offline-data: The app uploads drive data recorded without internet access

**UC ID and Name:** `UC-SYNC-sync-offline-data`: Upload drive data recorded offline
**Created By:** Team 12
**Date Created:** 2026-09-30
**Primary Actor:** system (runs automatically)
**Secondary Actors:** the grader (sees the result)
**Trigger:** The device regains an internet connection, or the app is opened, while drive data is waiting in device storage.
**Description:** Coverage drops along real routes, so the app keeps recording while offline and uploads what it recorded once a connection returns. No route point, mistake, motion event, or OBD sample should be lost, and the grader should never be blocked from continuing a drive because of coverage.

**Preconditions:**

- PRE-1. Device storage holds at least one queued item (route points, grades, motion events, or OBD samples) for a drive that already exists on the server.
- PRE-2. The device has a connection that reaches the API.

**Postconditions:**

- POST-1. Every queued item the server accepts is stored against its drive.
- POST-2. Accepted items are removed from device storage; anything not yet accepted stays queued.
- POST-3. The grader can see whether any of their data is still waiting to upload.

**Main Success Scenario:**

1. The system detects that the device is back online, or that the app has opened with queued data.
2. The system uploads the queued items, grouped by drive and in the order they were recorded.
3. The server confirms each batch.
4. The system removes the confirmed batches from device storage.
5. The system clears the "waiting to upload" indicator once nothing is queued (see `UC-SYNC-check-system-status`).
6. Use case ends.

**Extensions:**

- **1a. The grader tries to finish or instant-fail a drive while offline:**
    - 1a1. The system keeps the drive open and the data queued, tells the grader the drive will be finalized when the connection returns, and finalizes it after the queue drains.
- **1b. Device storage was full when the app tried to queue data during the drive:**
    - 1b1. The system warned the grader at that moment that data was not being saved, rather than failing silently; whatever was queued before then still uploads.
- **2a. The connection drops again mid-upload:**
    - 2a1. The system stops, keeps already-confirmed batches removed, and leaves the rest queued for the next trigger.
- **3a. The server permanently rejects a batch (a 4xx response, e.g., its drive was deleted):**
    - 3a1. The system removes that batch so it is not retried forever, and tells the grader how many items were discarded and for which drive.
- **3b. The server fails temporarily (a 5xx response or timeout):**
    - 3b1. The system keeps the batch queued and retries on the next trigger.
- **3c. A batch was stored but the confirmation was lost, so it is sent again:**
    - 3c1. The server ignores items it has already stored for that drive, so the retry does not duplicate route points or grades.

**Priority:** High
**Frequency of Use:** Any drive through weak coverage — likely common.
**Business Rules:** none identified yet

**Associated Information:**

| Property name | Data type | Validation rule | Security or access concerns | Glossary reference |
|---|---|---|---|---|
| queued batch | Object | Belongs to exactly one existing drive ID | Contains a minor's location; kept only on the device that recorded it, and removed once accepted | Upload Queue |
| upload status | Enum | waiting, uploading, up to date, items discarded | Visible to the grader who recorded the drive | Upload Status |

Drive Tracker today (`frontend/src/stores/session.js`): route points, grades, motion events, and OBD samples are queued in the browser's `localStorage` and retried every 10–15 seconds and on the browser's `online` event, **but only while a drive is active**; anything left over is retried only when the next drive starts (step 1 on app open is new). The drive record itself is created by the server when the drive starts, so starting a drive needs a connection (see `UC-SESS-start-drive-session`, 7b), and so does finishing one (extension 1a is new). Telemetry rejected with a 4xx is dropped without telling the user (3a1's notice is new), while queued grades are retried even after a 4xx. A failed `localStorage` write is ignored silently (1b is new). The server has no duplicate protection (3c is new). Removing a practice-mode flag is not queued at all and is lost offline.

Failure handling: data is removed from the device only after the server confirms it.

**Related Use Cases:** `UC-SESS-start-drive-session`; `UC-SESS-end-drive-session`; `UC-SYNC-check-system-status` (shows the indicator); `UC-OBD-show-connection-status` (a different link: the car's adapter, not the internet).
**Assumptions:** "Online" means the API answers, not merely that the device reports a network; the browser's `online` event alone is not enough.
**Open Issues:** `OI-offline` — confirm with Eric whether a drive must be startable with no connection at all (which needs a device-generated drive ID), or whether "start online, keep recording offline" is enough for the MVP. `localStorage` holds only a few megabytes; is that enough for a long drive with OBD samples once per second?

---

## ADMIN — Organization & Drive Plan Administration

### UC-ADMIN-manage-student-limit: The platform administrator sets an account's student limit

**UC ID and Name:** `UC-ADMIN-manage-student-limit`: Set an account's student limit
**Created By:** Team 12
**Date Created:** 2026-09-30
**Primary Actor:** platform administrator
**Secondary Actors:** none
**Trigger:** The platform administrator opens an account in SaaS administration to view or change its student limit.
**Description:** The client's proposed business model is gating each account to a maximum number of students (`FEAT-student-limits`, client benchmark item 5). The platform administrator needs to see each account's limit and current use and change it, for example when a family pays for another student. The account holder can see their limit but cannot raise it themselves, or the gate would mean nothing.

**Preconditions:**

- PRE-1. The user is logged in with the platform administrator flag.
- PRE-2. The account being changed exists.

**Postconditions:**

- POST-1. The account's student limit equals the new value.
- POST-2. Adding or restoring students for that account is blocked once its active student count reaches the limit (`UC-ACCT-manage-student-roster`, 4c).

**Main Success Scenario:**

1. The platform administrator opens SaaS administration and selects an account.
2. The system shows the account's current student limit and its number of active students.
3. The platform administrator enters a new limit and saves.
4. The system validates the new limit against the "Student limit rules" in Associated Information.
5. The system saves the limit and records who changed it and when.
6. The system confirms the change.
7. Use case ends.

**Extensions:**

- **1a. The user is not a platform administrator:**
    - 1a1. The system does not show SaaS administration, and the server rejects the request.
- **4a. The new limit is lower than the account's active student count:**
    - 4a1. The system rejects it, shows the active count, and changes nothing. Students are never archived automatically to fit a limit.
- **4b. The new limit is not a whole number of at least 1:**
    - 4b1. The system names the problem and stays on step 3.
- **5a. The save fails:**
    - 5a1. The old limit stays in force and the system reports the error.

**Priority:** Medium — client benchmark item, but N has not been set.
**Frequency of Use:** Rare — at sign-up and whenever an account's plan changes.
**Business Rules:** `BR-max-students-per-account`, `BR-platform-admin-settings`

**Associated Information:**

Student limit rules:

| Property name | Data type | Validation rule | Security or access concerns | Glossary reference |
|---|---|---|---|---|
| student limit | Integer | ≥ 1 and ≥ the account's active student count; default per `BR-max-students-per-account` | Set only by a platform administrator; the account holder may view it | Student Limit |

Drive Tracker today: no student cap exists anywhere in the code. SaaS administration (users, organizations, plans, maneuvers, criteria, drive types) already exists behind the platform-admin flag, so this belongs there.

Failure handling: a single write; a rejected or failed change leaves the old limit.

**Related Use Cases:** `UC-ACCT-manage-student-roster` (enforces the limit); `UC-ACCT-create-account` (a new account gets the default limit).
**Assumptions:** None beyond the open issues.
**Open Issues:** The client has not named N, so `BR-max-students-per-account` is not yet a rule (business-rules.md). Is the limit per parent profile or per organization ("per account or profile" in vision-and-scope)? Does organization roster sync count toward it? `OI-business-model`.

---

## SESS — Drive Session Management

### UC-SESS-start-drive-session: The grader starts a drive

**UC ID and Name:** `UC-SESS-start-drive-session`: Start a drive
**Created By:** Team 12
**Date Created:** 2026-09-25
**Primary Actor:** grader (parent, instructor, or examiner)
**Secondary Actors:** GPS provider; OBD-II adapter (optional); the student and parent/guardian (sign for DL-40 drives); the school's reservation system (optional)
**Trigger:** The grader opens New Drive.
**Description:** The grader wants to start a drive for one of their students under a chosen drive plan, with the route recorded from GPS and vehicle data from an OBD-II adapter if one is connected, so the drive can be graded as it happens and reviewed afterward.

**Preconditions:**

- PRE-1. The user is logged in as a parent, instructor, or examiner.
- PRE-2. At least one active student is on the user's roster.
- PRE-3. At least one active drive plan is available to the user's role and organization.

**Postconditions:**

- POST-1. A drive record exists with its start time, the selected roster student, the drive plan, and the plan's drive type.
- POST-2. The drive stores a fixed copy of the student's name, permit number, date of birth, and school, so later roster edits don't change it.
- POST-3. For a DL-40 drive type, the student's signature, and the parent/guardian section if used, are stored with the drive.
- POST-4. Route points, motion events (hard braking, hard acceleration, sharp turns), and, if an adapter is connected, OBD samples are being recorded against the drive.

**Main Success Scenario:**

1. The grader opens New Drive.
2. The system lists the drive plans available to the grader and the active students on their roster.
3. The grader selects a plan and a student, and optionally records weather, traffic, and notes.
4. The system shows the plan's drive type. If it requires a DL-40, the system also shows the student signature pad and the optional parent/guardian section (relationship, driver license number, signature).
5. The grader optionally connects an OBD-II adapter (see `UC-OBD-pair-device`) and taps "Begin Drive."
6. The system checks location permission and gets a GPS fix.
7. The system creates the drive and starts recording the route, motion events, and OBD samples, and keeps the screen awake.
8. The system shows the live grading screen for the drive type: flag mode for practice (`UC-GRAD-log-infraction`) or the DL-40 checklist (`UC-DL40-conduct-graded-test`).
9. Use case ends; the drive continues until `UC-SESS-end-drive-session`.

**Extensions:**

- **2a. The grader has no active students:**
    - 2a1. The system shows an empty state linking to Students (`UC-ACCT-manage-student-roster`); "Begin Drive" stays unavailable.
- **2b. No drive plans are available to the grader:**
    - 2b1. The system explains that no plans are available and that the school must create one (`UC-ADMIN-manage-drive-plans`).
- **2c. The school's reservation integration is active and the grader has appointments today:**
    - 2c1. The system lists today's appointments; choosing one preselects the matching roster student by external ID, then by permit or license number.
    - 2c2. If no roster student matches, the system warns and the grader selects one manually. Continue at step 3.
- **3a. The selected student does not match the chosen appointment (different external ID, permit number, or date of birth):**
    - 3a1. The server rejects the drive and the system asks the grader to fix the selection.
- **4a. A DL-40 drive is started without the student's signature:**
    - 4a1. The system blocks the start and asks the student to sign.
- **4b. The parent/guardian section is partly filled in:**
    - 4b1. The system requires all three of relationship (son, daughter, or ward), driver license number, and signature, or none of them.
- **5a. The adapter fails to connect:**
    - 5a1. The system reports the failure and the grader may retry or begin without it; a connection is never required.
- **5b. The device is an iPhone running the web app:**
    - 5b1. The system explains that iPhone Safari cannot reach Bluetooth or USB adapters and that the native app is needed; the drive can still begin without OBD.
- **6a. Location is unavailable or permission is denied:**
    - 6a1. The system explains how to enable location and does not start the drive.
    - 6a2. Only a platform administrator may start with simulated GPS and a simulated adapter instead, for testing.
- **7a. Since the list was loaded, the student was archived or is no longer in the grader's scope:**
    - 7a1. The server refuses, and the system asks the grader to choose an active student from their roster.
- **7b. There is no internet connection when "Begin Drive" is tapped:**
    - 7b1. The drive cannot be created, and the system says the device is offline and asks the grader to retry (see `OI-offline` in `UC-SYNC-sync-offline-data`).
- **7c. The app is backgrounded or the screen locks mid-drive:**
    - 7c1. The phone may pause GPS; recording resumes when the app returns, and the review map shows the gap as a jump in the route.

**Priority:** High
**Frequency of Use:** Every practice drive or road test; several times a week per student.
**Business Rules:** `BR-drive-active-student`, `BR-parent-own-students`, `BR-org-roster`, `BR-dl40-signatures`, `BR-dl40-minor-statement`, `BR-platform-admin-settings`

**Associated Information:**

| Property name | Data type | Validation rule | Security or access concerns | Glossary reference |
|---|---|---|---|---|
| roster student | Reference | Required; active; in the grader's scope | Checked on the server, not just in the UI | Roster Student |
| drive plan | Reference | Required; active; visible to the grader's role and organization | n/a | Drive Plan |
| drive type | Reference | Taken from the plan, not chosen separately | n/a | Drive Type |
| weather, traffic | String | Optional; up to 50 and 30 characters | n/a | Drive Conditions |
| student signature | PNG image | Required when the drive type requires a DL-40 | Sensitive; scoped like the drive | Signature |
| parent/guardian relationship | Enum | son, daughter, or ward; all-or-nothing with the two below | Sensitive | Parent/Guardian Statement |
| parent/guardian license number | String | Up to 40 characters | Sensitive | Parent/Guardian Statement |
| parent/guardian signature | PNG image | Required if the section is used | Sensitive | Signature |

Drive Tracker today: implemented as described above, including the seeded drive types "Official DL-40" (graded, limited to examiners and instructors) and "Parent Practice" (flag mode).

Failure handling: creating the drive is a single insert; if it fails, nothing is recorded and GPS is not started. Recording failures after that point are queued (`UC-SYNC-sync-offline-data`).

**Related Use Cases:** `UC-ACCT-manage-student-roster`; `UC-OBD-pair-device`; `UC-GRAD-log-infraction`; `UC-DL40-conduct-graded-test`; `UC-SESS-end-drive-session`; `UC-HOUR-track-progress`; `UC-SYNC-sync-offline-data`.
**Assumptions:** The teen does not need a login; the parent or examiner enters everything.
**Open Issues:** `OI-offline` (start a drive with no connection). Should parents be able to run a practice DL-40, given the seeded "Official DL-40" type is limited to examiners and instructors? `OI-examiner-device` (tablet layout).

---

## DL40 — Digital DL-40 Grade Sheet

### UC-DL40-conduct-graded-test: The examiner grades a road test on the digital DL-40

**UC ID and Name:** `UC-DL40-conduct-graded-test`: Grade a road test on the DL-40
**Created By:** Team 12
**Date Created:** 2026-09-25
**Primary Actor:** examiner (or instructor)
**Secondary Actors:** student (drives); GPS provider
**Trigger:** The examiner begins a drive whose drive type requires a DL-40 (`UC-SESS-start-drive-session`).
**Description:** The examiner grades each maneuver of the road test as it happens, in the order of the route actually driven rather than the printed order of the paper form. The system totals the deductions, decides pass or fail, and produces a DL-40 ready to print. This is the feature the client identified as the reason Drive Grader exists.

**Preconditions:**

- PRE-1. The examiner is logged in as an examiner or instructor.
- PRE-2. A DL-40 drive is in progress, started under a plan whose maneuver order matches the route (`UC-DL40-configure-checklist`).
- PRE-3. The student's DL-40 signature was captured when the drive started.

**Postconditions:**

- POST-1. Every maneuver in the plan has a deduction recorded for each graded aspect, or zeros per `BR-dl40-ungraded-maneuver`.
- POST-2. The drive records its deduction total, its final score (100 minus deductions), pass or fail, and a result code (XFDD, XFVL, or XFDA) when failed.
- POST-3. A completed DL-40 can be generated as a PDF from the drive review (`UC-DL40-capture-signatures-and-print`).

**Main Success Scenario:**

1. The system shows the plan's maneuvers in route order. Each maneuver lists its graded aspects (control, observation, position, signal) with the point values that can be deducted.
2. When a maneuver happens, the examiner taps the point value for an aspect.
3. The system records the deduction with the time and GPS position, and clears any zero on other aspects of the same maneuver.
4. The examiner repeats steps 2–3 through the route.
5. The examiner taps "Finish" and confirms.
6. The system uploads any queued grades, records zeros for maneuvers with no deductions, removes zeros from maneuvers that have deductions, and totals the deductions.
7. The system computes the final score and marks the drive passed or failed (XFDD) per `BR-dl40-pass-threshold`.
8. The system shows the result and opens the drive review, where the DL-40 can be printed.
9. Use case ends.

**Extensions:**

- **2a. A maneuver happens out of the planned order (route deviated):**
    - 2a1. The examiner scrolls to that maneuver and grades it there; the order guides navigation but does not restrict grading.
- **2b. The examiner taps the wrong value:**
    - 2b1. Tapping the same value again clears it; tapping a different value replaces it.
- **2c. The examiner records 0 for an aspect while another aspect of the same maneuver has a deduction:**
    - 2c1. The system refuses and explains why (`BR-dl40-ungraded-maneuver`).
- **2d. The student commits a speed violation or a dangerous action:**
    - 2d1. The examiner taps "Instant Fail" for that reason and confirms; the dialog warns that this cannot be undone.
    - 2d2. The system uploads queued data, ends the drive, marks it failed with XFVL or XFDA (`BR-dl40-instant-fail`), and opens the review. Use case ends.
- **3a. The device is offline:**
    - 3a1. The deduction is kept on the device and uploaded later (`UC-SYNC-sync-offline-data`); grading continues.
- **5a. The examiner cancels the confirmation:**
    - 5a1. The system returns to grading (step 4).
- **5b. Some maneuvers were never graded because they did not happen (test cut short):**
    - 5b1. Before confirming, the system lists the maneuvers with no grades and asks the examiner to confirm they were performed without deductions, or to grade them first. (Drive Tracker today records them as zero deductions without asking, which could pass a student on maneuvers never driven.)
- **6a. Finishing fails because the device is offline or the server errors:**
    - 6a1. The system reports the failure; the drive stays in progress with all grades kept, and the examiner retries.
- **8a. The examiner later corrects a grade:**
    - 8a1. See `UC-SESS-edit-drive-grades`; the score is recalculated when it is saved again.

**Priority:** High
**Frequency of Use:** Once per road test; fewer than practice drives, but the feature the client most wants working.
**Business Rules:** `BR-dl40-official-form`, `BR-dl40-electronic-grading`, `BR-dl40-maneuver-deductions`, `BR-dl40-printed-points`, `BR-dl40-deduction-total`, `BR-dl40-pass-threshold`, `BR-dl40-instant-fail`, `BR-dl40-ungraded-maneuver`, `BR-road-test-route-maneuvers`, `BR-org-roster`

**Associated Information:**

| Property name | Data type | Validation rule | Security or access concerns | Glossary reference |
|---|---|---|---|---|
| maneuver order | Ordered list of maneuver references | Comes from the drive plan | Set by examiners/instructors when creating the plan | DL-40 Checklist |
| aspect deduction | Integer | Must be one of that maneuver and aspect's values per `BR-dl40-printed-points` | Part of an official test record | Deduction |
| grade time and position | Timestamp, latitude/longitude | Captured automatically | Minor's location | Grade |
| final score | Integer | 100 minus the deduction total | Official result | Final Score |
| result code | Enum | XFDD, XFVL, XFDA, or none when passed | Official result | Result Code |

Display: maneuvers appear in plan order and collapse once every aspect is graded; the bottom bar holds Instant Fail (speed violation, dangerous action) and Finish.

Drive Tracker today: implemented as described, except 5b. Its seeded deduction values (2/1/0, with one 3/2/0) disagree with the form; the values to build against are `BR-dl40-printed-points`.

Failure handling: each grade is saved as it is tapped, so a crash or reload does not lose earlier grades; the drive stays in progress until Finish or Instant Fail succeeds.

**Related Use Cases:** `UC-SESS-start-drive-session` (captures signatures and starts the drive); `UC-DL40-configure-checklist` (plan order); `UC-DL40-capture-signatures-and-print` (PDF); `UC-SESS-edit-drive-grades`; `UC-DL40-suggest-route`.
**Assumptions:** The set of graded maneuvers is fixed by the state form and the route requirements (`BR-road-test-route-maneuvers`), not invented per route.
**Open Issues:** `AS-dps-approval-stands` — confirm DPS approval covers the app's printed format. `OI-dps-log-format`. Confirm 5b with Eric: should finishing with ungraded maneuvers be allowed at all?

---

## HOUR — Hour & Requirement Tracking

### UC-HOUR-track-progress: The parent views progress toward required hours

**UC ID and Name:** `UC-HOUR-track-progress`: View progress toward required hours
**Created By:** Team 12
**Date Created:** 2026-09-25
**Primary Actor:** parent
**Secondary Actors:** none
**Trigger:** The parent opens a student's hours view.
**Description:** The parent wants to see how many of Texas's required hours their teen has completed in each category, and how many remain, so they know how close the teen is to being eligible for the road test.

**Preconditions:**

- PRE-1. The parent is logged in.
- PRE-2. The selected student is on the parent's roster.

**Postconditions:**

- POST-1. The parent sees, for each hour category, the hours completed, the hours remaining, and the percentage complete, plus the total against the required total.
- POST-2. Nothing is changed; this is read-only.

**Main Success Scenario:**

1. The parent opens the hours view and selects a student.
2. The system adds up the durations of that student's finished, non-deleted drives in each hour category.
3. The system shows completed hours, remaining hours, and percentage complete for each category, and the combined total against `BR-ptde-total-hours`.
4. Use case ends.

**Extensions:**

- **1a. The parent has more than one student:**
    - 1a1. The system asks which student to show and never combines students' hours (`BR-parent-multiple-students`).
- **1b. The parent tries to open a student who is not on their roster:**
    - 1b1. The server refuses; nothing about that student is shown.
- **2a. A drive has no hour category:**
    - 2a1. The system counts it as general driving and marks it so the parent can recategorize it.
- **2b. A drive is still in progress or was deleted:**
    - 2b1. The system leaves it out of the totals.
- **2c. A category's requirement is already met:**
    - 2c1. The system shows the category as complete, with 0 remaining, never a negative number.
- **3a. The student has no finished drives:**
    - 3a1. The system shows 0 hours and 0% for every category, which is a valid result, not an error.
- **3b. The totals cannot be computed (server error):**
    - 3b1. The system shows an error rather than a wrong number; starting a drive is not affected.

**Priority:** High — but the first `FEAT-training-hours-log` item to cut if time runs short, per vision-and-scope.
**Frequency of Use:** Frequent — after most drives.
**Business Rules:** `BR-ptde-required-hours`, `BR-ptde-total-hours`, `BR-parent-own-students`, `BR-parent-multiple-students`

**Associated Information:**

| Property name | Data type | Validation rule | Security or access concerns | Glossary reference |
|---|---|---|---|---|
| hour category | Enum | instruction, observation, general driving (per `BR-ptde-required-hours`) | Set by the grader | Hour Category |
| drive duration | Minutes | From the drive's start to its end; computed by the server | n/a | Drive Duration |
| required hours per category | Number | Defaults per `BR-ptde-required-hours`; configurable (`UC-HOUR-configure-requirements`) | Set by the school, not the parent | Hour Requirement |

Display: one progress bar per category, then the total. Show hours to one decimal place.

Drive Tracker today: no hours feature. Each finished drive already stores `durationMinutes`, but its only classification is its drive type ("Official DL-40" or "Parent Practice"), which does not map to the state's hour categories. The existing `GET /grades/progress/:studentID` endpoint reports grade averages by maneuver (`FEAT-progress-history`, `UC-GRAD-view-progress-history`), not hours, and looks students up by the legacy login ID rather than the roster ID.

Failure handling: read-only; failure shows an error and changes nothing.

**Related Use Cases:** `UC-SESS-end-drive-session` (writes the durations); `UC-HOUR-configure-requirements` (sets the targets); `UC-HOUR-export-hours-log` (produces the log the family presents). The backlog's "Progress Meter" is this use case's display, not a separate use case.
**Assumptions:** A drive that ended in a failed road test still counts toward hours. Confirm.
**Open Issues:** How is a drive's hour category chosen: by drive type, by the grader at start, or afterward? Observation hours are time the teen watches rather than drives — are they recorded as drives at all, or entered by hand? `FEAT-training-hours-log` also lists night practice, and vision-and-scope mentions 10 required night hours, but `BR-ptde-required-hours` does not include night hours until the state requirement is confirmed. Are required hours configured per school, per state, or both?

---

## Working these with your agent

_[Delegate: drafting the main success scenario once you have the trigger and the goal; proposing extensions you have not thought of, which it is genuinely good at; turning a filled-in use case into a first set of test cases; checking that every `BR-*` you cite exists in [business-rules.md](business-rules.md).]_

_Keep human: whether this is one use case or three, what the priority is, and whether an extension the agent proposed is a real path in your client's business or a generic one it has seen elsewhere. "The system handles concurrent edits" is a real requirement for some projects and invented complexity for others, and only you have met the client._

_The verification that catches the most: read the main success scenario aloud to someone who has not read the document, and stop wherever they ask a question. Every question is a missing step or a missing extension._

_**Checklist for each use case:** Does the name start with a verb? Can the system test every precondition? Does every step alternate actor and system? Is there at least one extension per step that can fail? Does every business rule appear as an identifier only? Could a tester write test cases from this without asking you anything?_

**What's left before this document is stable:**
- Specify the use cases marked "Listed" in Section 3.2. Start with the ones Drive Tracker already does (`UC-SESS-end-drive-session`, `UC-GRAD-log-infraction`, `UC-GRAD-review-infraction-log`, `UC-OBD-*`, `UC-DL40-configure-checklist`, `UC-DL40-capture-signatures-and-print`), since the code answers most questions, then the client benchmark items (`UC-SYNC-check-system-status`, `UC-SESS-filter-session-history`, `UC-SESS-generate-drive-report`, `UC-SESS-select-map-layer`, `UC-SETT-*`).
- `UC-DL40-suggest-route` has no `FEAT-*` entry. Either add one to vision-and-scope with the client's agreement, or drop the use case.
- Add `BR-password-complexity` and `BR-reset-token-expiry` to [business-rules.md](business-rules.md). `BR-max-students-per-account` waits on Eric naming N.
- Raise the access-control gap in Section 3.3 with the team; it affects every drive-reading use case and is a privacy risk for minors' location data (`RI-minor-location-data`).
- Confirm with Eric: `OI-offline` (start a drive with no connection); whether parents may run a practice DL-40; extension 5b of `UC-DL40-conduct-graded-test`; the hour-category questions in `UC-HOUR-track-progress`.
- Mirror the open issues above into [OPEN-ISSUES.md](OPEN-ISSUES.md).

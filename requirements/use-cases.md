# Use Cases

**Project:** Drive Grader
**Team:** 12
**Client:** Eric Brown
**Version:** 0.1

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
| _[2026-09-11]_ | 0.1 | Initial use cases derived from the vision and scope feature list | _[Kaylynn]_ |
| _[2026-09-25]_ | 0.2 |	Rewrote Purpose/Scope and Use Case List against Client Meeting 1 (Eric Brown); replaced the template's placeholder example with real Drive Grader use cases (UC-SESS-start-drive-session, UC-DL40-conduct-graded-test, UC-HOUR-track-progress); dropped ANLZ/CONT/MON areas pending confirmation | _[Kaylynn]_ |

---

## 1. Introduction

### 1.1 Purpose

Drive Grader gives parents doing Texas Parent-Taught Driver Education a structured way to grade their teen's driving instead of "go that way, don't hit that cone" with no real criteria. It does this three ways, confirmed directly by the client: (1) real-time infraction logging against ~10 standard grading categories during a practice drive, (2) a digital version of the state's official DL-40 road-test grade sheet — reordered to match the actual test route instead of the sheet's fixed printed order — that ends in a signed, printed grade sheet, and (3) tracking progress toward the 44 hours (30 general + 7 instruction + 7 observation) Texas requires before testing. This document specifies those goals in enough detail that a developer knows what to build and a tester knows what to check.

### 1.2 Scope

_[Which feature areas from the vision and scope are covered here. Name the `FEAT-*` entries. If a feature has no use cases yet, say so rather than leaving the reader to notice.]_

Covers what the client confirmed as core: session tracking, real-time grading, the DL-40 digital grading mode, hour/requirement tracking, OBD-II vehicle data integration, and the existing admin panel (organizations, drive plans, maneuvers/score criteria). Explicitly out of scope for this version, per the client: AI evaluation/comparison of sessions ("AI is optional, not required — no specific use case identified"); instructional/how-to videos and real-time instructor fleet observation were in the original brief but were not raised or confirmed in the client meeting, so they are dropped from this list until confirmed. If they resurface, they belong under new area codes, not folded into the areas below.

Open mapping issue: these areas are written directly from the client meeting; they have not yet been reconciled against a formal FEAT-* list in vision-and-scope.md. Confirm that mapping before treating this section list as final.

---

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

_[Your area codes, then a table of every use case by area. Write this list first, before specifying any single use case in detail. It is the cheapest thing to review with your client, and finding out you missed a whole area costs minutes here rather than a week later.]_

| Area code | Feature area | Use cases |
|---|---|---|
| _[SESS]_ | _[Drive Session Management]_ | _[`UC-SESS-start-drive-session, UC-SESS-end-drive-session, UC-SESS-view-session-history`]_ |
| _[GRAD]_ | _[Real-Time Infraction Grading]_ | _[`UC-GRAD-log-infraction, UC-GRAD-review-infraction-log`]_ |
| _[DL40]_ | _[Digital DL-40 Grade Sheet]_ | _[`UC-DL40-configure-checklist, UC-DL40-conduct-graded-test, UC-DL40-capture-signatures-and-print`]_ |
| _[HOUR]_ | _[Hour & Requirement Tracking]_ | _[`UC-HOUR-track-progress, UC-HOUR-configure-requirements`]_ |
| _[OBD]_ | _[OBD-II / Vehicle Data Integration]_ | _[`UC-OBD-pair-device, UC-OBD-stream-vehicle-data`]_ |
| _[ADMIN]_ | _[Organization & Drive Plan Administration]_ | _[`UC-ADMIN-manage-drive-plans, UC-ADMIN-manage-maneuvers-and-criteria`]_ |
---

OBD-II is confirmed hardware (four Bluetooth units purchased) but functionally unverified as of the meeting — whether CAN-bus turn-signal/brake data is exposed at all, and whether it works on EVs, are open questions, not settled requirements. Treat OBD use cases as spikes until that's resolved.


## 4. Use Cases

_[One `###` heading per use case, grouped under a `##` heading per area. Worked example below, taken from Project Pulse. Delete it and write your own.]_

### UC-RUB-find-criteria: The course admin finds criteria

**UC ID and Name:** `UC-RUB-find-criteria`: Find criteria
**Created By:** _[Name]_
**Date Created:** _[YYYY-MM-DD]_
**Primary Actor:** course admin
**Secondary Actors:** none
**Trigger:** The course admin indicates to find criteria.
**Description:** The course admin wants to find the peer evaluation criteria defined in her course so that she can review, edit, delete, or add one to a rubric.

**Preconditions:**

- PRE-1. The course admin is logged into the system.

**Postconditions:**

- POST-1. A list of matching criteria in the course admin's course is returned and displayed. The list may be empty.

**Main Success Scenario:**

1. The course admin indicates to find criteria.
2. The system asks the course admin to enter search values according to the "Search criteria" defined in the Associated Information of this use case.
3. The course admin enters one or more search values and confirms that she has finished entering.
4. The system finds all criteria in the course admin's course that match the provided search criteria.
5. The system displays the matching criteria according to the "Search results display strategy" and the "Sort criteria" defined in the Associated Information of this use case.
6. Use case ends.

**Extensions:**

- **4a. No matching criteria are found:**
    - 4a1. The system alerts the course admin that no matching criteria are found.
    - 4a2. The course admin either chooses `UC-RUB-create-criterion`: Create a criterion, or terminates the use case, or returns to step 2 of the normal flow.

**Priority:** High
**Frequency of Use:** Occasional; mostly at course setup and rubric revision.
**Business Rules:** `BR-role-based-access`

**Associated Information:**

Search criteria:

| Property name | Data type | Validation rule | Security or access concerns | Glossary reference |
|---|---|---|---|---|
| criterion name | String | Optional | Course-scoped to the course admin's course | Criterion |

Search results display strategy: criterion name, description, max score.

Sort criteria: criterion name, ascending.

**Related Use Cases:** `UC-RUB-create-criterion`: Create a criterion.
**Assumptions:** none
**Open Issues:** none
---

##SESS — Drive Session Management
**UC-SESS-start-drive-session:** The parent starts a drive session

**UC ID and Name:** UC-SESS-start-drive-session: Start a drive session Created By: Team 12 Date Created: 2026-09-25 Primary Actor: parent (or instructor) Secondary Actors: OBD-II device (optional, real or simulated); GPS provider (real or simulated) Trigger: The parent creates a new drive and taps "Begin Drive." Description: The parent wants to start a named drive session for a specific driver profile, with GPS and optional OBD-II tracking running, so the drive can be graded in real time and reviewed afterward.

**Preconditions:**

PRE-1. The user is logged in and authenticated.
PRE-2. A driver profile exists to associate with the session (e.g., "Test Driver").

**Postconditions:**

POST-1. A drive session record exists in "in progress" state, associated with the driver profile and a drive plan (e.g., "Basic Skills").
POST-2. GPS route data (real or simulated) and, if selected, OBD-II data begin logging against the session.
**
Main Success Scenario:**

The parent creates a new drive, naming the drive plan and selecting the driver profile.
The system presents session settings (GPS source, OBD-II device) with sensible defaults.
The parent accepts the defaults or selects a simulated GPS and/or simulated OBD-II device for testing, or a real device if in-vehicle.
The parent taps "Begin Drive."
The system starts logging GPS location, speed, and route, plus accelerometer data (hard braking, hard turns, G-forces).
The system displays the live in-progress view so the parent can grade the drive (see UC-GRAD-log-infraction) while it runs.
Use case ends (session continues until UC-SESS-end-drive-session).

**Extensions:**

3a. A real OBD-II device is selected but fails to pair:
3a1. The system alerts the parent that the device is unavailable.
3a2. The parent may retry pairing, switch to simulated/no OBD-II, or proceed without it.
4a. GPS lock cannot be acquired (real GPS selected, no simulated fallback):
4a1. The system alerts the parent and offers to retry or switch to simulated GPS.
5a. The app is backgrounded and the OS suspends location/Bluetooth tracking mid-session:
5a1. The system resumes tracking on foreground return and marks the gap as an interruption in the session log.

**Priority:** High Frequency of Use: Every practice drive or road test, potentially several times per week per driver. Business Rules: BR-role-based-access

**Associated Information:**

Property name	Data type	Validation rule	Security or access concerns	Glossary reference
driver profile	Reference	Required	Scoped to the parent's/org's account	Driver Profile
drive plan	Reference	Required, defaults to a standard plan	n/a	Drive Plan
gps source	Enum	real, simulated	n/a	GPS Source
obd device	Enum/Reference	none, simulated, or paired real device	Device data is vehicle-derived, not personal	OBD-II Device

**Failure handling:** session creation is all-or-nothing; a mid-session interruption is logged, not discarded — partial data captured before the interruption is retained.

**Related Use Cases:** UC-GRAD-log-infraction; UC-SESS-end-drive-session; UC-HOUR-track-progress (a completed session should count toward hour requirements). Assumptions: The teen driver does not need their own login — the client confirmed the parent inputs all grading data themselves. Open Issues: Whether a PWA gives sufficient Bluetooth/GPS access for real OBD-II devices, or whether the team must move to a Capacitor-wrapped native build — the client flagged this as something the team needs to determine, not something already decided.
---

##DL40 — Digital DL-40 Grade Sheet
B The instructor conducts a graded road test on the digital DL-40

**UC ID and Name:** UC-DL40-conduct-graded-test: Conduct a graded road test Created By: Team 12 Date Created: 2026-09-25 Primary Actor: instructor/examiner (or parent, for a practice road test) Secondary Actors: driver, parent/guardian (signs at end) Trigger: The instructor selects a driver and taps to begin a DL-40 graded test. Description: The instructor wants to grade a road test electronically, tapping each required maneuver as it happens along the actual route driven — rather than hunting for it on a fixed-order paper form — so that a completed, signed grade sheet can be printed at the end. This is the feature the client identified as the reason Drive Grader exists.

**Preconditions:**

PRE-1. The instructor is logged in and authenticated.
PRE-2. The DL-40 checklist has been reordered/configured to match the planned route (see UC-DL40-configure-checklist).
PRE-3. A driver profile and parent/guardian record are available to attach to this test.

**Postconditions:**

POST-1. Every maneuver on the DL-40 checklist has a recorded grade outcome (pass/fail/notes) tied to a timestamp.
POST-2. A completed DL-40 grade sheet exists with driver and parent/guardian signatures, ready to print.

**Main Success Scenario:**

The instructor selects the driver and the pre-configured, route-ordered DL-40 checklist.
The instructor selects the driver and parent/guardian for signature capture.
The instructor begins the test; the system displays the reordered checklist item by item.
As each maneuver occurs (parking, merge, lane change, approach-to-corner, traffic signal, traffic sign, left turn, right turn, backing, etc.), the instructor taps the matching item and records the grade.
The system timestamps and records each graded item against the session.
The instructor completes the last maneuver on the checklist.
The system presents the completed grade sheet for driver and parent/guardian signature (see UC-DL40-capture-signatures-and-print).
Use case ends.

**Extensions:**

4a. A maneuver happens that isn't next on the reordered checklist (route deviated from plan):
4a1. The instructor searches or scrolls to the correct item out of sequence and grades it there.
4a2. The system still records the correct timestamp and item; checklist order is a navigation aid, not a constraint on grading order.
4b. The instructor taps the wrong item by mistake:
4b1. The instructor may undo/correct the most recent grading action before moving on.
6a. The test is ended before every checklist item is graded (e.g., test aborted):
6a1. The system marks ungraded items as "not administered" rather than as a pass or fail.
6a2. The grade sheet indicates the test was incomplete.

**Priority:** High Frequency of Use: Once per official road test; lower volume than practice-drive grading but the feature the client most wants to see working. Business Rules: BR-dl40-dps-approved (Texas DPS has approved electronic grading with a printed result), BR-role-based-access

**Associated Information:**

Property name	Data type	Validation rule	Security or access concerns	Glossary reference
checklist order	Ordered list of references	Must contain every required DL-40 item exactly once	Instructor-configurable per route	DL-40 Checklist
grade outcome	Enum	pass, fail, not administered	Part of an official test record	Grade Outcome
signatures	Image/binary	Required before print (see UC-DL40-capture-signatures-and-print)	Signature data is sensitive; access scoped to the org	Signature

**Failure handling: **grade sheet is built incrementally as each item is tapped; a crash mid-test does not lose already-graded items, but the session must be resumed or explicitly marked incomplete rather than silently left open.

**Related Use Cases:** UC-DL40-configure-checklist (must happen first); UC-DL40-capture-signatures-and-print (happens last); UC-SESS-start-drive-session (a DL-40 test runs inside a session). Assumptions: The required maneuver set (three left turns, three right turns, two stop signs, two lights, two approach-to-corners, parallel park, reverse, etc.) is fixed by the state form and not something the app invents per route. Open Issues: None from the meeting — this is the most concretely specified feature the client described. Confirm the exact full DL-40 item list and required counts against the actual state form before building the checklist data model.
---

##HOUR — Hour & Requirement Tracking
**UC-HOUR-track-progress: **The parent views progress toward required hours

**UC ID and Name:** UC-HOUR-track-progress: Track progress toward required hours Created By: Team 12 Date Created: 2026-09-25 Primary Actor: parent Secondary Actors: none Trigger: The parent opens the driver's progress view. Description: The parent wants to see how many of the required hours their teen has completed — general driving, behind-the-wheel instruction, and observation — so they know how close the teen is to meeting Texas's 44-hour requirement before testing.

**Preconditions:**

PRE-1. The parent is logged in and authenticated.
PRE-2. At least one completed drive session exists for the driver, or the driver has zero logged hours (still a valid, empty state).
**
Postconditions:**

POST-1. The parent sees hours completed and hours remaining in each of the three categories (general, instruction, observation), against the configured target for each.

**Main Success Scenario:**

The parent opens the driver's progress view.
The system totals logged session time by category (general/instruction/observation) for that driver.
The system displays completed hours, remaining hours, and percentage complete per category, plus the combined total against the 44-hour requirement.
Use case ends.

**Extensions:**

2a. A session's category was never set or is ambiguous:
2a1. The system counts it toward "general" by default and flags it so the parent can recategorize it.

**Priority:** High Frequency of Use: Frequent — checked after most sessions. Business Rules: BR-hour-requirements (30 hours general + 7 hours behind-the-wheel instruction + 7 hours observation = 44 total)

**Associated Information:**

Property name	Data type	Validation rule	Security or access concerns	Glossary reference
session category	Enum	general, instruction, observation	n/a	Session Category
target hours per category	Number	Configurable per BR-hour-requirements	Org-level, not driver-level, unless overridden	Hour Requirement

**Failure handling:** this is a read-only aggregation; a failure to compute it should show a clear error rather than a wrong number, and never blocks the ability to start a new session.

**Related Use Cases:** UC-SESS-end-drive-session (writes the hours this view reads); UC-HOUR-configure-requirements (sets the targets referenced here). Assumptions: None beyond BR-hour-requirements as stated by the client. Open Issues: The client suggested modeling target hours as configurable variables per category rather than hardcoding 30/7/7 — confirm whether that configurability is org-level, state-level, or both, since Texas-specific rules may not generalize if the app is ever used outside Texas.



---

## Working these with your agent

_[Delegate: drafting the main success scenario once you have the trigger and the goal; proposing extensions you have not thought of, which it is genuinely good at; turning a filled-in use case into a first set of test cases; checking that every `BR-*` you cite exists in [business-rules.md](business-rules.md).]_

_Keep human: whether this is one use case or three, what the priority is, and whether an extension the agent proposed is a real path in your client's business or a generic one it has seen elsewhere. "The system handles concurrent edits" is a real requirement for some projects and invented complexity for others, and only you have met the client._

_The verification that catches the most: read the main success scenario aloud to someone who has not read the document, and stop wherever they ask a question. Every question is a missing step or a missing extension._

_**Checklist for each use case:** Does the name start with a verb? Can the system test every precondition? Does every step alternate actor and system? Is there at least one extension per step that can fail? Does every business rule appear as an identifier only? Could a tester write test cases from this without asking you anything?_

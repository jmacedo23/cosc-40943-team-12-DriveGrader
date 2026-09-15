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
| _[YYYY-MM-DD]_ | 0.1 | Initial use cases derived from the vision and scope feature list | _[Name]_ |

---

## 1. Introduction

### 1.1 Purpose

Drive Grader gives parents doing Texas Parent-Taught Driver Education a structured way to grade their teen's driving instead of "go that way, don't hit that cone" with no real criteria. It does this three ways, confirmed directly by the client: (1) real-time infraction logging against ~10 standard grading categories during a practice drive, (2) a digital version of the state's official DL-40 road-test grade sheet — reordered to match the actual test route instead of the sheet's fixed printed order — that ends in a signed, printed grade sheet, and (3) tracking progress toward the 44 hours (30 general + 7 instruction + 7 observation) Texas requires before testing. This document specifies those goals in enough detail that a developer knows what to build and a tester knows what to check.

### 1.2 Scope

_[Which feature areas from the vision and scope are covered here. Name the `FEAT-*` entries. If a feature has no use cases yet, say so rather than leaving the reader to notice.]_

Covers what the client confirmed as core: session tracking, real-time grading, the DL-40 digital grading mode, hour/requirement tracking, OBD-II vehicle data integration, and the existing admin panel (organizations, drive plans, maneuvers/score criteria). Explicitly out of scope for this version, per the client: AI evaluation/comparison of sessions ("AI is optional, not required — no specific use case identified"); instructional/how-to videos and real-time instructor fleet observation were in the original brief but were not raised or confirmed in the client meeting, so they are dropped from this list until confirmed. If they resurface, they belong under new area codes, not folded into the areas below.

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

## Working these with your agent

_[Delegate: drafting the main success scenario once you have the trigger and the goal; proposing extensions you have not thought of, which it is genuinely good at; turning a filled-in use case into a first set of test cases; checking that every `BR-*` you cite exists in [business-rules.md](business-rules.md).]_

_Keep human: whether this is one use case or three, what the priority is, and whether an extension the agent proposed is a real path in your client's business or a generic one it has seen elsewhere. "The system handles concurrent edits" is a real requirement for some projects and invented complexity for others, and only you have met the client._

_The verification that catches the most: read the main success scenario aloud to someone who has not read the document, and stop wherever they ask a question. Every question is a missing step or a missing extension._

_**Checklist for each use case:** Does the name start with a verb? Can the system test every precondition? Does every step alternate actor and system? Is there at least one extension per step that can fail? Does every business rule appear as an identifier only? Could a tester write test cases from this without asking you anything?_

# Business Rules

**Project:** Drive Grader
**Team:** Team 12
**Client:** Eric Brown
**Version:** 0.3

---

_**How to use this template.** Instructions appear in italic square brackets. Fill in underneath them and leave them in place until the document is stable._

_**What a business rule is.** A corporate policy, a government regulation, a law, an industry standard, or a computational formula. Business rules are a rich source of requirements, because they dictate properties your system must have in order to conform to them._

_**What a business rule is not: a software requirement.** This is the distinction students get wrong, so read it twice. A rule is a property of the **business**. It exists whether or not your software does, it was true before you arrived, and it will still be true if the project is cancelled. "A student may only submit a peer evaluation during an active week" is a rule the course had before anyone wrote code._

_What belongs to your software is the **enforcement** of that rule, and that is a functional requirement, written in the specification and cited back here. Keeping the two apart is what lets you answer the question that comes up every semester: "who decided this, and can we change it?" If it is a rule, the client's organization decides and you comply. If it is a requirement, your team decides and you can negotiate._

## How to hear one in a meeting

_[Rules almost never arrive announced. They surface in the middle of a story about something else, usually in one of these shapes:]_

- _"Must comply with..."_
- _"Only `<someone>` may `<do something>`"_
- _"If `<condition>`, then `<something happens>`"_
- _"Must be calculated according to..."_
- _"...unless it has been more than a year."_

_Examples of a client stating a rule without knowing it: "A new client must pay 30 percent of the estimated consulting fee and travel expenses in advance." "Time-off approvals must comply with the company's vacation policy."_

_When you hear one, write it down in the meeting. You will not reconstruct it afterward, and the exact wording matters because the rule is someone else's sentence, not yours._

## The five shapes a rule takes

_[Useful for recognizing rules, not for organizing this document. Sections below are grouped by topic, not by these categories.]_

| Shape | What it does | Example |
|---|---|---|
| **Fact** | States something always true about the business | Every senior design team belongs to exactly one course section. |
| **Constraint** | Restricts what may be done, or by whom | Only a course admin may create a course section. |
| **Action enabler** | Triggers an action when a condition holds | If a student has not completed safety training in 12 months, the request is refused. |
| **Inference** | Derives a new fact from known facts | A team with no submissions for two consecutive weeks is at risk. |
| **Computation** | Defines how a value is calculated | The peer evaluation score is the mean of all scores received that week. |

_Computations are the ones teams forget are rules. A formula the client uses today is a rule you must reproduce exactly, not a design decision you get to make. Ask for the spreadsheet._

## What a rule turns into

_[One rule usually propagates into several requirements of different kinds. This is why the document exists as its own artifact rather than being scattered through the specification.]_

| Requirement type | How the rule shows up | Example |
|---|---|---|
| Business requirement | A regulation drives a business objective | The system must enable compliance with all federal and state chemical reporting regulations within five months. |
| User requirement | A privacy policy dictates who may do what | Only laboratory managers may generate chemical exposure reports for anyone other than themselves. |
| Functional requirement | A company policy becomes system behavior | If an invoice is received from an unregistered vendor, the system shall email the vendor the supplier intake form and the W-9. |
| Quality attribute | A safety regulation becomes a checked property | The system must maintain safety training records and check them before a user can request a hazardous chemical. |

## Identifiers and traceability

_Each rule carries a stable `BR-<slug>` identifier, a name-based slug coined from the rule's gist: `BR-active-weeks`, `BR-section-admin-only`, `BR-artifact-key-unique`. Never renumber, rename, or repoint one. The thematic grouping into sections below is organizational only and does not affect a rule's identity, so moving a rule between sections is free and renaming it is not._

_**Cite rules, do not copy them.** When a use case is governed by a rule, its Business Rules field carries the identifier only, never the rule's text. One rule, one home. A rule copied into three use cases will be updated in one of them._

_A rule may cite another rule by identifier where one depends on another._

## Every rule needs a source

_[The column teams leave blank, and the one that matters most. For each rule, record where it comes from: a named policy document, a regulation, a page of the client's handbook, or the person who told you and the date.]_

_A rule you cannot attribute is usually not a rule. It is your team's design decision wearing a rule's clothes, and it belongs in the specification where it can be argued with. The test: if you asked your client to change it tomorrow, who would have to approve? If the answer is "you", it was never a rule._

_Where a rule is expected to change, say so and say when. Rules change on the business's schedule, not on yours._

## Where your AI teammate helps, and where it is dangerous

_[Delegate: turning your meeting notes into candidate rules, spotting sentences in a transcript that have the shape of a rule, and finding use cases in your specification that a given rule ought to govern but does not cite.]_

_**Do not let it invent rules.** This section is the single most dangerous place in your requirements for fabricated content, because an invented rule reads exactly like a real one. "Passwords must be at least 8 characters." "Records must be retained for 7 years." Both are plausible, both are common, and neither is your client's policy unless your client said so. A fabricated rule then propagates into functional requirements, tests that pass, and code that enforces something nobody asked for._

_The Source column is the defense. Every rule traces to a document or a person, or it does not go in the file. When your agent proposes a rule, the only question is: who told us this?_

## Revision History

| Date | Version | Description | Author |
|---|---|---|---|
| 2026-09-11 | 0.1 | Initial rules drafted from initial client meeting | Kanta Endo|
| 2026-09-25 | 0.2 | Added `BR-dl40-maneuver-deductions` | Kanta Endo |
| 2026-09-25 | 0.3 | Added rules from the 2026-09-11 client meeting (DL-40, road-test maneuvers, required hours); added section 3; replaced template placeholder 2.1 | Kanta Endo |

---

## 1. Introduction

### 1.1 Purpose

This document collects the policies and regulations — primarily from the Texas Department of Public Safety (DPS) governing parent-taught driver education — that Drive Grader must conform to, so that the specification can cite these rules rather than restate them.

### 1.2 Scope

Covers rules governing (a) the required driving-hour log for parent-taught driver education in Texas, and (b) the DL-40 road-test grade sheet. Does **not** cover the team's own technical or design decisions (backend database choice, UI polish, deployment platform, semester deadlines) — those are client/team constraints, not business rules, and belong in the specification instead. See the "Flagged as *not* a business rule" section below for the items pulled out of this file and why.

---

## 2. Rules

_[Group rules under topic headings that fit your project. The Project Pulse headings are one example, not a required set: Course Administration, Teams and Assignment, Access and Ownership, Identity and Uniqueness, Editing and Locking, Deletion Integrity, Review and Submission._

_Format each rule as a bold identifier, the rule in one sentence, then its source.]_

_[Flag rules you are not sure about rather than dropping them; deciding whether something is a rule or a requirement is a conversation to have with your client, and it is worth having.]_

_**Checklist:** Does every rule have a source? Could your client change it without asking you? Is it stated as one sentence about the business, rather than as a sentence about your software? Does any use case cite it, and if none does, is that correct?_

### 2.1 Road Test Scoring

- **`BR-dl40-maneuver-deductions`:** On a road test, each graded aspect of a maneuver (control, observation, position, signal) is rated Bad, Fair, or Good, and the rating deducts the points printed on the DL-40 for that aspect, with Good always deducting 0.
  **Source:** Texas DPS form DL-40 (Rev. 10/15), "Record of Examination" page: the Bad / Fair / Good point columns for each maneuver and the "Road Test Deductions" box.
- **`BR-dl40-official-form`:** A Texas road test is graded on the Texas DPS form DL-40.
  **Source:** Eric Brown (client), meeting 2026-09-11, [transcript](../docs/requirements/client-interview-2026-09-11.md) §4 and §5.
- **`BR-dl40-electronic-grading`:** A DL-40 may be graded electronically during the road test, provided a completed DL-40 is printed afterward.
  **Source:** Eric Brown (client), meeting 2026-09-11, §5, relaying an approval the client obtained from Texas DPS. **Needs confirmation:** the approval was reported verbally. Obtain it in writing (who at DPS approved it, when, and on what conditions), including whether the printout may list items in route order rather than the form's printed order.
- **`BR-dl40-signatures`:** A completed DL-40 carries the signatures of the driver and of the parent or guardian.
  **Source:** Eric Brown (client), meeting 2026-09-11, §5, said while demoing the app. **Needs confirmation** against the DL-40 form itself: which signatures the form requires, and whether the examiner also signs.

### 2.2 Road Test Maneuvers

- **`BR-road-test-route-maneuvers`:** A road-test route is built to include a required set of maneuvers: three left turns, three right turns, two stop signs, two traffic lights, two approach-to-corners, a parallel park, and a reverse.
  **Source:** Eric Brown (client), meeting 2026-09-11, §5. **Incomplete:** the client ended the list with "etc.", and did not say whether the counts are exact or minimums. Get the full list and its source document from the client.
- **`BR-stop-at-stop-line`:** When stopping at a stop line, the driver must stop at the line, neither past it nor short of it.
  **Source:** Eric Brown (client), meeting 2026-09-11, §4, as an item graded on the DL-40 (see `BR-dl40-official-form`).
- **`BR-approach-to-corner`:** When approaching an intersection that has no stop sign, the driver must look in both directions.
  **Source:** Eric Brown (client), meeting 2026-09-11, §4, as the DL-40 item "approach to corner" (see `BR-dl40-official-form`).

### 2.3 Driver Education Hours

- **`BR-ptde-required-hours`:** Before taking the road test, a Texas teen driver must log 7 hours of behind-the-wheel instruction, 7 hours of observation, and 30 hours of general driving.
  **Source:** Eric Brown (client), meeting 2026-09-11, §4 and §9. **Expected to change** on the state's schedule, not ours; the client asked for the required hours per category to be configurable (§9). **Needs confirmation** against the state's published parent-taught requirements, since the meeting did not cover whether any category has further conditions (for example, time of day) or whether hours in one category count toward another.
- **`BR-ptde-total-hours`:** The total required driving-education hours is the sum of the three category requirements in `BR-ptde-required-hours`, currently 7 + 7 + 30 = 44.
  **Source:** Eric Brown (client), meeting 2026-09-11, §4 and §9 ("44 hours total").
- **`BR-ptde-no-instructor-qualification`:** In Texas parent-taught driver education, the parent teaching the course is not required to hold an instructor qualification.
  **Source:** Eric Brown (client), meeting 2026-09-11, §1. **Needs confirmation** against the state program's eligibility rules for the parent instructor. This rule matters because it decides who may grade a drive.

---

## 3. Flagged as *not* a business rule

_[Items from the 2026-09-11 meeting that look like rules but fail the test "if we asked to change it, who would approve?" Each is the client's or team's decision, so it belongs in the specification or vision document, where it can be negotiated.]_

| Statement | Source | Why it is not a rule | Where it belongs |
|---|---|---|---|
| Nobody verifies the logged hours in practice. | Client, §4 | An observation about current practice, not a policy. It motivates tracking hours automatically. | Vision and scope (problem statement) |
| Practice drives are graded on about ten categories (following distance, smooth braking, speed control, lane discipline, situational awareness, ...). | Client, §2 and §5 | The client's product design. The client can change the list without anyone's approval. | Specification (functional requirement) |
| The DL-40 checklist can be reordered to match the route. | Client, §5 | A feature of the app. What DPS permits for the printout is covered by `BR-dl40-electronic-grading`. | Specification |
| The required hours are configurable, with progress shown per category. | Client, §9 | How the app enforces `BR-ptde-required-hours`. | Specification |
| The backend uses MySQL 8. | Client, §8 | A technical constraint set by the client. | Specification (constraints) |
| The app should be intuitive; no specific design preference. | Client, §9 | A quality attribute. | Specification (quality attributes) |
| AI integration is optional. | Client, §8 | A scope decision. | Vision and scope |
| Working MVP by end of semester; handoff around January. | Client, §8 | A project schedule constraint. | Vision and scope |
| Quasar/Node.js PWA, possibly wrapped in Capacitor; OpenStreetMap for maps. | Client, §6 | Technology choices. | Specification (constraints) |
| OBD2/CAN-bus data may provide turn-signal and brake use. | Client, §3 and §9 | An open technical question, not a policy. | Open issues |

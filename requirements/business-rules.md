# Business Rules

**Project:** Drive Grader
**Team:** Team 12
**Client:** Eric Brown
**Version:** 0.4

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
| 2026-09-30 | 0.4 | Completed DL-40 point values, pass/fail, and signatures from the client's Drive Tracker repo (`DL-40.pdf` and grading code); completed the route-maneuver list; added access rules from the 2026-09-29 meeting and the roster spec | Mameo007 |


---

## 1. Introduction

### 1.1 Purpose

This document collects the policies and regulations — primarily from the Texas Department of Public Safety (DPS) governing parent-taught driver education — that Drive Grader must conform to, so that the specification can cite these rules rather than restate them.

### 1.2 Scope

Covers rules governing (a) the required driving-hour log for parent-taught driver education in Texas, (b) the DL-40 road-test grade sheet, including the points printed on the form and the pass/fail computation the client's app applies to that sheet, and (c) who may see a student and that student's drives. Does **not** cover the team's own technical or design decisions (backend database choice, UI polish, deployment platform, semester deadlines) — those are client/team constraints, not business rules, and belong in the specification instead. See the "Flagged as *not* a business rule" section below for the items pulled out of this file and why.

---

## 2. Rules

_[Group rules under topic headings that fit your project. The Project Pulse headings are one example, not a required set: Course Administration, Teams and Assignment, Access and Ownership, Identity and Uniqueness, Editing and Locking, Deletion Integrity, Review and Submission._

_Format each rule as a bold identifier, the rule in one sentence, then its source.]_

_[Flag rules you are not sure about rather than dropping them; deciding whether something is a rule or a requirement is a conversation to have with your client, and it is worth having.]_

_**Checklist:** Does every rule have a source? Could your client change it without asking you? Is it stated as one sentence about the business, rather than as a sentence about your software? Does any use case cite it, and if none does, is that correct?_

### 2.1 Road Test Scoring

- **`BR-dl40-official-form`:** A Texas road test is graded on the Texas DPS form DL-40.
  **Source:** Eric Brown (client), meeting 2026-09-11, [transcript](../docs/requirements/client-interview-2026-09-11.md) §4 and §5. The form in the client's Drive Tracker repo is `DL-40.pdf`, titled DL-40 (Rev. 10/15).
- **`BR-dl40-maneuver-deductions`:** On a road test, each graded aspect of a maneuver (control, observation, position, signal) is rated Bad, Fair, or Good, and the rating deducts the points printed on the DL-40 for that aspect, with Good always deducting 0.
  **Source:** Texas DPS form DL-40 (Rev. 10/15), "Record of Examination" page: the Bad / Fair / Good point columns for each maneuver and the "Road Test Deductions" box. Copy: client's Drive Tracker repo, `DL-40.pdf`.
- **`BR-dl40-printed-points`:** The points deducted for Bad, Fair, and Good are the numbers printed in that maneuver's boxes on DL-40 (Rev. 10/15), as listed below (Bad / Fair / Good; a dash means the form has no box for that aspect).
  **Source:** `DL-40.pdf` in the client's Drive Tracker repo, Record of Examination. The same numbers are the circle positions in `frontend/src/utils/dl40Generator.js` (`SCORE_POSITIONS`). **Do not copy the lookup seed:** `api/db/seeds/001_lookup_data.js` stores `[2, 1, 0]` for almost every aspect and `[3, 2, 0]` only for Use of Lanes / Control, which does not match this form.

  | Maneuver | Control | Observation | Position | Signal |
  |---|---|---|---|---|
  | Start | 3 / 1 / 0 | 3 / 1 / 0 | — | 2 / 1 / 0 |
  | Quick stop | 2 / 1 / 0 | 3 / 1 / 0 | — | — |
  | Backing | 2 / 1 / 0 | 3 / 1 / 0 | 2 / 1 / 0 | — |
  | Parallel park | 2 / 1 / 0 | 3 / 1 / 0 | 2 / 1 / 0 | 2 / 1 / 0 |
  | Upshifting | 2 / 1 / 0 | — | 2 / 1 / 0 | — |
  | Downshifting | 2 / 1 / 0 | — | 2 / 1 / 0 | — |
  | Lane change | 3 / 2 / 0 | 4 / 2 / 0 | 3 / 2 / 0 | 3 / 2 / 0 |
  | Merge | 3 / 2 / 0 | 4 / 2 / 0 | 3 / 2 / 0 | 3 / 2 / 0 |
  | Use of lanes | 3 / 2 / 0 | 4 / 2 / 0 | 3 / 2 / 0 | 4 / 2 / 0 |
  | Right of way | 2 / 1 / 0 | 4 / 2 / 0 | — | 4 / 2 / 0 |
  | Posture | 2 / 1 / 0 | — | — | — |
  | Approach to corner (1st and 2nd) | 2 / 1 / 0 | 4 / 2 / 0 | — | — |
  | Traffic signal (1st and 2nd) | 2 / 1 / 0 | 3 / 2 / 0 | 2 / 1 / 0 | 2 / 1 / 0 |
  | Traffic sign (1st and 2nd) | 2 / 1 / 0 | 3 / 2 / 0 | 2 / 1 / 0 | 2 / 1 / 0 |
  | Left turn (1st, 2nd, and 3rd) | 2 / 1 / 0 | 3 / 2 / 0 | 2 / 1 / 0 | 2 / 1 / 0 |
  | Right turn (1st, 2nd, and 3rd) | 2 / 1 / 0 | 3 / 2 / 0 | 2 / 1 / 0 | 2 / 1 / 0 |

  The same page also prints a motorcycle off-street test and an air-brake pre-trip, with their own Bad/Good or Fail/Pass marks. Those are not part of the car road test in `BR-dl40-school-markings`.
- **`BR-dl40-deduction-total`:** The road-test deduction total is the sum of the circled point values, and that total is written in the Road Test Deductions box for that exam.
  **Source:** DL-40 (Rev. 10/15) "Road Test Deductions" box (1st, 2nd, and 3rd exam columns, split Off Street / On Street). The client's app sums grade scores into that box (`dl40Generator.js`, `totalDeductions`) and stores `100 - totalDeductions` as the session score (`api/src/services/SessionService.js`).
- **`BR-dl40-pass-threshold`:** A road test passes when 100 minus the deduction total is at least 70, and a test with 31 or more deductions fails with result code XFDD.
  **Source:** Client's Drive Tracker app: `SessionService.js` sets `finalScore = 100 - totalDeductions`, `passed = finalScore >= 70`, and `failReason` `XFDD` when it does not pass; `dl40Generator.js` comments XFDD as "fail-31+-deductions" and XP as pass. **Needs confirmation:** `DL-40.pdf` prints the point columns and the deductions box, and it does not print this 70-point line or the result-code legend. Confirm against the DPS scoring instructions before treating 70 / XFDD as the state's rule rather than the client's encoding of it.
- **`BR-dl40-instant-fail`:** A speed violation fails the road test immediately with result code XFVL, and a dangerous action fails it immediately with result code XFDA, whatever the deduction total is.
  **Source:** Client's Drive Tracker app: grading screen labels "Speed Violation — Instant Fail" and "Dangerous Action — Instant Fail" (`frontend/src/pages/GradingPage.vue`), and the API accepts only `XFVL` and `XFDA` as instant-fail reasons (`api/src/routes/sessions.js`). `dl40Generator.js` comments XFVL as "fail-speeding" and XFDA as "fail-dangerous-act". **Needs confirmation** the same way as `BR-dl40-pass-threshold`: these codes are not printed on the DL-40 sheet in the repo.
- **`BR-dl40-ungraded-maneuver`:** A maneuver with no deduction is recorded as zeros and lined through; if any aspect of that maneuver has a deduction, a zero on another aspect of the same maneuver is not kept.
  **Source:** Client's Drive Tracker app, `SessionService.js` (end-of-session fill) and `dl40Generator.js` (`drawZeroLine`). **Needs confirmation** with the client that examiners mark the paper DL-40 this way, and that it is not only how the app fills blank rows.
- **`BR-dl40-electronic-grading`:** A DL-40 may be graded electronically during the road test, provided a completed DL-40 is printed afterward.
  **Source:** Eric Brown (client), meeting 2026-09-11, §5, relaying an approval the client obtained from Texas DPS. **Needs confirmation:** the approval was reported verbally. Obtain it in writing (who at DPS approved it, when, and on what conditions), including whether the printout may list items in route order rather than the form's printed order.
- **`BR-dl40-signatures`:** A completed DL-40 for a minor carries the parent or guardian's signature and driver license number, and the applicant's signature on the record of examination.
  **Source:** DL-40 (Rev. 10/15) page 1, "Signature of Parent or Guardian" and "Driver License No."; page 2, the APPLICANT column. Eric Brown, meeting 2026-09-11, §5, said the app selects the driver and parent/guardian with signatures. The form also has an examiner column and a "Notary Public or Authorized Officer" line. The app captures the applicant signature and an optional parent block (`GradingPage.vue`) and does not capture a notary. **Needs confirmation:** whether a parent-taught sheet must be notarized, and whether the examiner signs separately from the parent.
- **`BR-dl40-minor-statement`:** On the parent or guardian's sworn statement, the minor is identified as son, daughter, or ward, and the statement authorizes a license class.
  **Source:** DL-40 (Rev. 10/15) page 1 sworn statement. The app requires relationship, driver license number, and signature together when any part of the parent block is used (`api/src/routes/sessions.js`). The parent block itself is optional in the app. **Needs confirmation** that a practice sheet may omit the sworn statement, since the form prints it for licensing a minor.
- **`BR-dl40-school-markings`:** The client's current car-test printout marks Class C, checks Driver Education Laboratory and not Classroom or Motorcycle, writes B on the REMOVED restriction line, and crosses out the motorcycle off-street test, identifying control, air-brake pre-trip, start, quick stop, upshifting, and downshifting.
  **Source:** `frontend/src/utils/dl40Generator.js`. The restriction code is commented "fixed B per school process," and the crossed-out blocks are commented "Sections not applicable to a standard car driving test." **Needs confirmation** with Eric. The form itself offers Class A, B, C, and M and prints those sections, so this is his filing practice until he says it is a DPS rule. See also `BR-dl40-printed-points`: start, quick stop, upshifting, and downshifting still have printed point values.

### 2.2 Road Test Maneuvers

- **`BR-road-test-route-maneuvers`:** A road-test route is built to include three left turns, three right turns, two stop signs, two traffic lights, two approaches to a corner, a parallel park, and a reverse, and the DL-40 on-street test also grades lane change, merge, use of lanes, right of way, and posture.
  **Source:** Eric Brown (client), meeting 2026-09-11, §5, for the counted set (he ended the list with "etc." and said "a reverse"). DL-40 (Rev. 10/15) Record of Examination for the printed names: three Left Turns, three Right Turns, two Traffic Signs, two Traffic Signals, two Approaches to Corner, Parallel Park, and Backing, plus Lane Change, Merge, Use of Lanes, Right of Way, and Posture. His "stop signs" and "traffic lights" are the form's Traffic Signs and Traffic Signals, and his "reverse" is the form's Backing. The form prints those counts as exact boxes, which matches the numbers he gave. **Needs confirmation** whether a route must contain exactly those counts or at least those counts. The seed maneuver list (`001_lookup_data.js`) has no Backing row; the PDF generator refers to backing as maneuver 22.
- **`BR-stop-at-stop-line`:** When stopping at a stop line, the driver must stop at the line, neither past it nor short of it.
  **Source:** Eric Brown (client), meeting 2026-09-11, §4, as an item graded on the DL-40 (see `BR-dl40-official-form`). The form scores Traffic Signs as control, observation, position, and signal (`BR-dl40-printed-points`); it does not print this stop-line sentence.
- **`BR-approach-to-corner`:** When approaching an intersection that has no stop sign, the driver must look in both directions.
  **Source:** Eric Brown (client), meeting 2026-09-11, §4, as the DL-40 item "approach to corner" (see `BR-dl40-official-form`). The form scores each approach as control and observation only (`BR-dl40-printed-points`).

### 2.3 Driver Education Hours

- **`BR-ptde-required-hours`:** Before taking the road test, a Texas teen driver must log 7 hours of behind-the-wheel instruction, 7 hours of observation, and 30 hours of general driving.
  **Source:** Eric Brown (client), meeting 2026-09-11, §4 and §9. **Expected to change** on the state's schedule, not ours; the client asked for the required hours per category to be configurable (§9). **Needs confirmation** against the state's published parent-taught requirements, since the meeting did not cover whether any category has further conditions (for example, time of day) or whether hours in one category count toward another. The Drive Tracker repo does not encode these hour totals.
- **`BR-ptde-total-hours`:** The total required driving-education hours is the sum of the three category requirements in `BR-ptde-required-hours`, currently 7 + 7 + 30 = 44.
  **Source:** Eric Brown (client), meeting 2026-09-11, §4 and §9 ("44 hours total").
- **`BR-ptde-no-instructor-qualification`:** In Texas parent-taught driver education, the parent teaching the course is not required to hold an instructor qualification.
  **Source:** Eric Brown (client), meeting 2026-09-11, §1. **Needs confirmation** against the state program's eligibility rules for the parent instructor. This rule matters because it decides who may grade a drive.

### 2.4 Who May See a Student

- **`BR-parent-own-students`:** A parent may see and manage only their own students and those students' drives.
  **Source:** Eric Brown (client), meeting 2026-09-29, [notes](../docs/requirements/client-interview-2026-09-29.md), "Data separation: parents see only their own kids' drives." The same boundary is an approved constraint in the client's repo, `_bmad-output/implementation-artifacts/spec-managed-student-rosters.md` (2026-08-28): "Parents can access only their personal roster," enforced in `api/src/services/RosterService.js`.
- **`BR-parent-multiple-students`:** A parent may have more than one student driver.
  **Source:** Eric Brown (client), meeting 2026-09-29: "Support multiple students (siblings/twins are common)."
- **`BR-org-roster`:** An examiner or instructor may access the student roster of their own organization, and not another organization's roster.
  **Source:** Client's approved roster spec, `spec-managed-student-rosters.md`: "examiners and instructors can access the full roster for their JWT organization," and "Never: expose global student search across organizations." Enforced in `RosterService.scopeFor`. The 2026-09-29 meeting stated the parent half (`BR-parent-own-students`) and did not restate this organization half.
- **`BR-platform-admin-settings`:** Only a user marked as platform administrator may open SaaS administration, and a platform administrator may access a roster outside their own organization.
  **Source:** Eric Brown (client), meeting 2026-09-29: the SaaS settings area "should only appear for admins via a platform admin switch on the user profile." Cross-scope roster access is the roster spec's "platform administrators retain authorized cross-scope access."
- **`BR-drive-active-student`:** A drive may be started only for an active student on the grader's own roster.
  **Source:** Client's approved roster spec: "Start Drive will only accept an active roster student," and archived or out-of-scope students are rejected. **Needs confirmation** that this is the client's operating policy for every drive, including a parent's practice drive, and not only the roster feature's acceptance criteria.
- **`BR-roster-no-hard-delete`:** A student who already has a recorded drive is not permanently deleted; the roster entry is archived so the drive history remains.
  **Source:** Client's approved roster spec: "Never: hard-delete roster entries referenced by sessions," with archive and restore instead. **Needs confirmation** with Eric that drive records must be retained, and for how long. The spec does not state a retention period.

---

## 3. Flagged as *not* a business rule

_[Items from the 2026-09-11 and 2026-09-29 meetings, and from the client's repo, that look like rules but fail the test "if we asked to change it, who would approve?" Each is the client's or team's decision, so it belongs in the specification or vision document, where it can be negotiated. A formula or access limit that the client or the DL-40 already fixed is in section 2 instead.]_

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
| OBD2/CAN-bus data may provide turn-signal and brake use. | Client, 2026-09-11 §3 and §9; 2026-09-29, the tested adapter does not capture CAN and "track what you can" | An open technical question, not a policy. | Open issues |
| OBD connection is not required; show connected / not connected. | Client, 2026-09-29 follow-up email | A product behavior. | Specification |
| Live parent viewing of a lesson ("like tracking an Uber"). | Client, 2026-09-29 | A product idea. Who may see a drive that already happened is `BR-parent-own-students`. | Vision and scope |
| Scheduling of upcoming drives, and a history of completed ones. | Client, 2026-09-29 | A feature request. | Specification |
| Reporting ("Justin drove on these dates for this long") and filtering by date, student, or instructor. | Client, 2026-09-29 | No report exists yet; the client asked for one. | Specification |
| Gating a profile to N students is the business model. | Client, 2026-09-29 follow-up email | He did not set N, and the repo does not enforce a student cap. Not a rule until he names the limit. | Open issues |
| More session types: highway, city, night, parallel parking. | Client, 2026-09-29 | A product idea. Night hours are not part of `BR-ptde-required-hours` until the state requirement is confirmed. | Specification |
| SaaS admin is a settings screen behind the platform-admin switch. | Client, 2026-09-29 | How the app presents `BR-platform-admin-settings`. | Specification |
| Expert mode hides descriptions; learning mode is for explanatory videos. | Client, 2026-09-29 | A display preference. | Specification |
| API-down banner, internet indicator, staging/dev banner with no banner in production, version 1.0.6 patch bumps, "What's New," aerial map layer, dark mode, PWA install prompts. | Client, 2026-09-29 | Quality and release mechanics. | Specification |
| Push to `STG`, not `main`; Quasar/Node deploy; redesign the thrown-together UI freely. | Client, 2026-09-29 | Engineering and design process. | Specification (constraints) |
| Official DL-40 sessions allow only examiner and instructor in the seed data. | Drive Tracker `api/db/seeds/001_lookup_data.js` | Conflicts with the client saying a parent uses the same app to grade a DL-40 (2026-09-11 §5). The seed is not a stated policy. | Open issues |
| Lookup-seed deduction values (`[2, 1, 0]`, with one `[3, 2, 0]` exception). | Drive Tracker `001_lookup_data.js` | They disagree with the form. The points to reproduce are `BR-dl40-printed-points`. | Specification (defect against the rule) |


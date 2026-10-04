# Architectural Design

**Project:** Drive grader
**Team:** Team 12
**Client:** THE Eric Brown
**Version:** 0.3

---

_**How to use this template.** Instructions appear in italic square brackets. Fill in underneath them and leave them in place until the document is stable. Every section says which checkpoint it is due at. A section that is not due yet stays as it is; do not fill it with guesses to make the document look finished._

_**What this document is.** Your system's **architecture-of-record**: the one map of the whole system, every use case area, every component, every external system, and the few decisions that are expensive to change later. It is **breadth-complete and depth-shallow**. Every part of the system is named, and nothing is designed further than its responsibility. How one use case works inside its component is a design-of-record, which comes in week 7, one per use case area, and it is written against real code._

_**What it is not.** A second copy of your requirements. The specification says what the system must do and how well; this document says how the system is shaped to do it. It **cites** `UC-*`, `CO-*`, `SEC-*`, `PER-*` and the rest by identifier and never restates them. A threshold that appears here and nowhere in the specification is a requirement hiding in the wrong document._

_**The test for what belongs here.** Decide now what is hard to reverse, affects the whole system, and is forced by a quality attribute or a constraint: how many deployables, where the data lives, how users sign in, which external systems you depend on. Leave to per-area design what is local and cheap to change: class names, endpoint shapes, table columns._

_**Structure.** The sections follow **arc42** (Starke and Hruschka), with **C4** diagrams (Simon Brown) for context and containers, written as mermaid so they diff in git. All twelve arc42 sections are here in arc42's order, numbering, and titles. The three subsections whose content another document already owns (the requirements overview, the stakeholders, and the quality requirements overview) are kept as one-line links to that document, so the numbering matches arc42's and nothing is written twice. arc42 orders sections by topic, not by when you write them, so Checkpoint 1 covers sections 1–5, 8, and 9, and sections 6 and 7 come later. The full worked example is Project Pulse's [architecture-of-record](https://github.com/Washingtonwei/project-pulse/blob/main/docs/design/architectural-design.md); read it for the shape, then write your own, because your client's quality attributes are not Project Pulse's.]_

## Identifiers

_[The new identifiers this document creates. Everything else it cites keeps the identifier of the document that owns it.]_

| Space | For | Example |
|---|---|---|
| `KD-<slug>` | Key architectural decisions | `KD-single-deployable` |
| `QS-<slug>` | Quality scenarios | `QS-cross-employee-order-denied` |
| `RISK-<slug>` | Technical risks | `RISK-payroll-api-unavailable` |
| `TD-<slug>` | Technical debt the architecture knowingly carries | `TD-no-rate-limiting` |

_[These are slugs, like every other identifier in your project, so an inserted decision renumbers nothing and a citation says what it points at. Project Pulse uses the same form: `KD-modular-monolith`, `QS-cross-team-denial`.]_

## Revision History

| Version | Date | Author | Change |
|---|---|---|---|
| 0.1 | | | Initial draft for Checkpoint 1 |
| 0.2 | 2026-10-02 | Mameo007 | First draft of section 8, from the client's proof of concept |
| 0.3 | 2026-10-02 | Team 12 | Section 9: architecturally significant requirements and key decisions drawn from the client's Drive Tracker application |

---

## 1. Introduction and Goals

_Due: Checkpoint 1._

### 1.1 Requirements overview

The requirements overview is defined by the [Software Requirements Specification](../requirements/software-requirements-specification.md) and the [Use Cases](../requirements/use-cases.md).

### 1.2 Quality goals

_[The **three** quality attributes that most shape your system, in priority order. Pick them from section 9 of your [specification](../requirements/software-requirements-specification.md) and cite their identifiers. If you cannot rank them, ask your client which one they would give up first; that answer is the ranking._

_These are usually the top rows of the table in section 9.1, and the two do different jobs. Here, say why each goal matters to your client. There, say which decision it forces._

| Priority | Quality goal | Specification handles | Why it shapes the architecture |
|---|---|---|---|
| 1 | Safe, low-distraction active-drive use | `SAF-interaction`, `PER-drive-tracking` | The app operates during driving instruction, so tracking must continue without unnecessary interaction while the vehicle is moving. This requires the active-drive workflow and device-location handling to be designed around safety first. |
| 2 | Intuitive mobile-first usability | `USE-mobile-first`, `USE-responsive`, `USE-intuitive` | Parents, students, and examiners need to start drives, log infractions, and review results quickly on mobile devices. The client’s success criterion—90% of representative users completing primary tasks without help—makes a simple responsive interface a system-wide priority. |
| 3 | Protection of driver and drive data | `SEC-account`, `SEC-permissions`, `SEC-driver-data`, `SEC-authorization` | The system stores student profiles, driving records, routes, and grading data. Account-based authorization and organization-level data separation must therefore be built into the application and API rather than added later. |


### 1.3 Stakeholders

The stakeholder profiles are defined in section 3.1 of the Vision and Scope (../requirements/vision-and-scope.md) document.

## 2. Architecture Constraints

The architecture honors the operating environment in section 2.3 of the [specification](../requirements/software-requirements-specification.md) and the design constraints in section 2.4. Identifiers are cited here and defined there.

**Operating environment:** `OE-responsive-web`, `OE-mobile-access`, `OE-pwa`.

**Design and implementation constraints:** `CO-frontend-framework`, `CO-backend`, `CO-database`, `CO-mobile-first`, `CO-pwa`, `CO-capacitor`, `CO-mapping`, `CO-existing-application`, `CO-obd2-testing`, `CO-obd-home`, `CO-github`, `CO-github-actions`, `CO-mvp`.

A sentence is added only where a constraint narrows a choice its text does not already make:

- `CO-frontend-framework`, `CO-backend`, `CO-database`, and `CO-mapping` belong in this section because the client's proof of concept already runs that stack and the client's developers inherit it after the January 2027 handoff (`CO-existing-application`).
- `CO-pwa` and `OE-pwa` make the phone client the same web application, installed to the home screen. `CO-capacitor` adds a native wrapper only when a requirement cannot be met in that browser app and the client agrees to maintain the wrapper.
- `CO-github` and `CO-github-actions` keep source control and staging deployment on the client's existing repository and GitHub Actions workflow, which deploys the UI and the API from the `STG` branch.
- `CO-obd2-testing` and `CO-obd-home` apply while the adapter is being proven. At runtime the user's phone reads the OBD-II device; those tools are not a container in the running system.

## 3. Context and Scope

_Due: Checkpoint 1._

_[One C4 context diagram: your system as a single box, every kind of user, and **every external system** it talks to (email, payment, an identity provider, a client database, an LLM, a file store). An external system discovered halfway through the build is a schedule risk you could have seen at the start._

_Your specification already lists the external systems. Every system named in a software interface (`SI-*`, section 8.3), a communications interface (`CI-*`, section 8.5), or a dependency (`DE-*`, section 2.5) is a box here. A box with none of those behind it is an interface your specification is missing, so add it there too._

_This is your project's one context diagram. Section 4.1 of [vision and scope](../requirements/vision-and-scope.md) holds the first draft: redraw it here in C4, then replace the drawing there with a link to this section, so there is one diagram to keep current._

_arc42 divides context into a **business context** (who and what crosses the boundary) and a **technical context** (the channels and protocols). This diagram is the business context. The protocols go on the arrows of the container diagram in section 5.1._

_The **trust boundary** is not drawn here. You name it in writing in section 8.1, as Project Pulse does._

_Example:]_

```mermaid
C4Context
    title System Context: Cafeteria Ordering System

    Person(patron, "Patron", "Employee ordering a meal")
    Person(staff, "Cafeteria Staff", "Prepares and delivers orders")
    Person(menu, "Menu Manager", "Maintains the daily menu")

    System(cos, "Cafeteria Ordering System", "Takes, prepares, and delivers meal orders")

    System_Ext(payroll, "Payroll System", "Deducts meal payments from pay")
    System_Ext(sso, "Corporate Sign-On", "Authenticates employees")
    System_Ext(email, "Corporate Email", "Order confirmations")

    Rel(patron, cos, "Orders meals")
    Rel(staff, cos, "Fulfils orders")
    Rel(menu, cos, "Edits menu")
    Rel(cos, payroll, "Submits payment requests")
    Rel(cos, sso, "Verifies identity")
    Rel(cos, email, "Sends confirmations")
```

Drive Grader is one system. This diagram is the business context: the people who use it, and the systems outside it. Protocols go on the container diagram in section 5.1. The trust boundary is named in section 8.1.

```mermaid
C4Context
    title System Context: Drive Grader

    Person(parent, "Parent / Guardian", "Supervises practice drives")
    Person(student, "Student", "The driver whose sessions are recorded")
    Person(instructor, "Instructor", "Runs lesson drives for the school")
    Person(examiner, "Examiner", "Grades the road test")
    Person(orgAdmin, "Organization Administrator", "Manages an organization's plans and settings")
    Person(sysAdmin, "System Administrator", "Manages accounts and organizations")

    System(dg, "Drive Grader", "Records, grades, and reviews driver-training sessions")

    System_Ext(sensors, "Mobile device sensors", "GPS and accelerometer on the user's phone")
    System_Ext(obd, "OBD-II adapter", "Vehicle data over Bluetooth")
    System_Ext(osm, "OpenStreetMap", "Map tiles for the driven route")
    System_Ext(reservations, "Reservation system", "An organization's bookings and student roster")
    System_Ext(ai, "AI service", "Optional drive analysis")

    Rel(parent, dg, "Starts drives, logs infractions, reviews results")
    Rel(student, dg, "Views results")
    Rel(instructor, dg, "Runs lesson drives")
    Rel(examiner, dg, "Grades the road test")
    Rel(orgAdmin, dg, "Manages drive plans and settings")
    Rel(sysAdmin, dg, "Manages accounts and organizations")
    Rel(sensors, dg, "Supplies location and motion")
    Rel(obd, dg, "Supplies vehicle data")
    Rel(dg, osm, "Loads map tiles")
    Rel(dg, reservations, "Reads appointments and students")
    Rel(dg, ai, "May request drive analysis")
```

The box is the client's proof of concept: a Quasar app, a Node.js API, and MySQL, continued rather than replaced. Instructor and examiner are separate roles in that application. Organization and account administration is a platform-admin flag on a user; anyone who can grade can edit drive plans.

Each outside box has an identifier in the specification:

| Box | Identifiers |
|---|---|
| Mobile device sensors | `SI-GPS`, `SI-ACCELEROMETER`, `AS-device-gps`, `AS-device-accelerometer` |
| OBD-II adapter | `SI-OBD2`, `SI-OBD2-BLUETOOTH`, `SI-OBD2-DATA`, `DE-obd2-hardware`, `DE-obd2-data`, `SI-OBDHOME`, `DE-obd-home` |
| OpenStreetMap | `SI-OSM`, `DE-openstreetmap` |
| Reservation system | none yet — closest is `FR-ADMIN-integrations` |
| AI service | `SI-AI`, `DE-ai` |

`SI-OBDHOME` and `DE-obd-home` are the same adapters the application connects to over Bluetooth while testing vehicle data. The proof of concept loads OpenStreetMap tiles on the live-drive and session-review maps. It already reads appointments and students from a reservation API configured per organization. The specification has no `SI-*` or `DE-*` for that system yet; `FR-ADMIN-integrations` is the requirement that covers integration settings, and vision and scope section 4.3 leaves further reservation work out of the MVP. `SI-AI` and `DE-ai` name an optional service. The proof of concept has no call to one.

`SI-NODE`, `SI-QUASAR`, `SI-MYSQL`, `SI-CAPACITOR`, `DE-mysql`, and `DE-capacitor` name parts of Drive Grader. They stay inside the one box and are drawn in the container view (section 5). `SI-GITHUB`, `SI-GITHUB-ACTIONS`, `DE-github`, and `DE-github-actions` are how the team stores and deploys the code. Section 8.5 of the specification names no `CI-*` interface. The channels it describes are the API inside Drive Grader, Bluetooth to the adapter, OpenStreetMap, the optional AI service, and Tailscale on the team's development network.

The examiner prints the DL-40 from a PDF the application generates on the device. The phone camera and Cloudflare are future items in vision and scope section 4.1, and the specification gives them no interface.

## 4. Solution Strategy

_Due: Checkpoint 1._

_[Three to five bullets: the few moves that shape everything else. arc42 suggests four kinds: the technology you build on, how the system is divided at the top level, how the quality goals in section 1.2 are met, and any organizational choice that shapes the code (who maintains what, what you buy instead of build)._

_Each bullet is one sentence, and it cites what explains it: the key decision in section 9.2 where one exists, and otherwise the quality goal and the building block in section 5 it shapes. Keep it short; the reasoning lives in section 9. A bullet that cites nothing is either not load-bearing, or it is a decision you have not written down yet._

- **One repository, the existing front end and API, and one MySQL database** (`KD-deployment-shape`), because the client's developers already run that shape and inherit it in January 2027 (`CO-existing-application`, `MNT-existing`).
- **The phone client is the only component that reads GPS, the accelerometer, and the OBD-II adapter** (section 5.2), so a missing adapter still leaves the drive usable (`ROB-obd2`) and recording continues while the car is moving (`SAF-interaction`).
- **Rules are split by use case area** — `SESS`, `GRAD`, `DL40`, `HOUR`, `OBD`, and `ADMIN` (section 5.2) — so a change to grading criteria or the DL-40 stays inside its own area (`MNT-existing`).
- **The phone client draws the route on OpenStreetMap, and the API stores the recorded route** (section 5.2, `CO-mapping`, `INT-mapping`).
- **Staging updates go out through the client's existing GitHub Actions deploy from the `STG` branch** (`CO-github-actions`), which is the workflow the inheriting developers already operate (`MNT-documentation`).

## 5. Building Block View

_Due: Checkpoint 1. This section is most of what your TA checks._

### 5.1 Containers

_[One C4 container diagram: the separately running or separately stored pieces inside your system box. For most projects that is a front end, a back end, and a database, and sometimes a file store. Name each container's technology. Every external system from section 3 appears again here, attached to the container that talks to it._

_Label every arrow with what it does and the protocol it uses ("Sends confirmations [SMTP]"). Those protocols are arc42's technical context._

_Under the diagram, one or two sentences on **why the system is divided this way**, citing `KD-deployment-shape`. A reader who sees three containers should not have to guess why there are not seven._

_Three containers is a normal answer. If you have more than five, check each one against section 9: which decision, driven by which quality attribute, requires it to run separately?_

```mermaid
C4Container
    title Container Diagram: Drive Grader

    Person(parent, "Parent / Guardian", "Supervises practice drives")
    Person(student, "Student", "The driver whose sessions are recorded")
    Person(instructor, "Instructor", "Runs lesson drives for the school")
    Person(examiner, "Examiner", "Grades the road test")
    Person(orgAdmin, "Organization Administrator", "Manages an organization's plans and settings")
    Person(sysAdmin, "System Administrator", "Manages accounts and organizations")

    System_Boundary(dg, "Drive Grader") {
        Container(web, "Phone client", "Quasar / Vue, PWA", "Drive, grading, DL-40, hours, and admin screens on the phone")
        Container(api, "API", "Node.js / Fastify", "Accounts, sessions, grades, plans, and the stored route")
        ContainerDb(db, "Database", "MySQL 8", "Users, organizations, sessions, grades, plans, and maneuvers")
    }

    System_Ext(sensors, "Mobile device sensors", "GPS and accelerometer on the user's phone")
    System_Ext(obd, "OBD-II adapter", "Vehicle data over Bluetooth")
    System_Ext(osm, "OpenStreetMap", "Map tiles for the driven route")
    System_Ext(reservations, "Reservation system", "An organization's bookings and student roster")
    System_Ext(ai, "AI service", "Optional drive analysis")

    Rel(parent, web, "Starts drives, logs infractions, reviews results", "HTTPS")
    Rel(student, web, "Views results", "HTTPS")
    Rel(instructor, web, "Runs lesson drives", "HTTPS")
    Rel(examiner, web, "Grades the road test", "HTTPS")
    Rel(orgAdmin, web, "Manages drive plans and settings", "HTTPS")
    Rel(sysAdmin, web, "Manages accounts and organizations", "HTTPS")
    Rel(web, api, "Calls", "JSON/HTTPS")
    Rel(api, db, "Reads and writes", "MySQL")
    Rel(web, sensors, "Reads location and motion", "Geolocation API, DeviceMotion")
    Rel(web, obd, "Reads vehicle data", "Bluetooth LE")
    Rel(web, osm, "Loads map tiles", "HTTPS")
    Rel(api, reservations, "Reads appointments and students", "JSON/HTTPS")
    Rel(api, ai, "May request drive analysis", "not yet chosen")
```

Drive Grader ships as the client's existing phone client, API, and one MySQL database, in one repository (`KD-deployment-shape`, section 4). The client's developers already run that shape and inherit it in January 2027 (`CO-existing-application`, `MNT-existing`). The phone client is its own container because it runs on the phone. The API is its own container because it stores the recorded route and is the only container that opens MySQL (`SI-NODE`, `SI-MYSQL`, `DE-mysql`).

The phone client is the only container that reads the mobile device sensors and the OBD-II adapter, and the only one that draws the route on OpenStreetMap (section 3, `SI-GPS`, `SI-ACCELEROMETER`, `SI-OBD2`, `SI-OSM`). The API is the only container that reads the reservation system. The AI service is the optional box from section 3; the proof of concept does not call it yet (`SI-AI`, `DE-ai`). A Capacitor build wraps that same phone client when the browser cannot reach the adapter (`CO-capacitor`, `SI-CAPACITOR`). OBD Home is the same adapter during testing (`SI-OBDHOME`), so it is not a separate box.

### 5.2 Use case areas and components

_[One row per use case area in your [use cases](../requirements/use-cases.md), taken from the area column of [traceability.md](../traceability.md) section 1, plus one row per **cross-cutting component** that no single area owns (authentication, notifications, file handling, an integration with an external system). A use case area with no row is a part of your system with no home; a component with no area and no cross-cutting reason is one nobody asked for._

_**Responsibility** is one sentence, what the component owns, not how it works. **Depends on** names other components and external systems, never classes. **Status** is `provisional` until the component has been built through at least one use case, and `proven` after that. At Checkpoint 1 every row is `provisional`; Checkpoint 2 turns at least one to `proven`._

_Project Pulse's component tables also name each component's package. They can because its code exists; yours does not yet, so a row here is a name and a responsibility, and packages come with the design-of-record in week 7._

Areas are the ten in [use cases](../requirements/use-cases.md) section 3. Section 4 names `SESS`, `GRAD`, `DL40`, `HOUR`, `OBD`, and `ADMIN`. Use cases v0.3 also lists `ACCT`, `SYNC`, `MON`, and `SETT`, so each of those has a row here too.

| Use case area | Component | Responsibility | Depends on | Status |
|---|---|---|---|---|
| `ACCT` | Accounts | Owns sign-in, account creation, and password reset | — | provisional |
| `SESS` | Drive sessions | Owns a drive from the moment it starts until it is saved, including the route stored by the API and the summary reviewed afterward | Phone client, Accounts | provisional |
| `GRAD` | Infraction grading | Owns each infraction logged during a drive and the log shown when that drive is reviewed | Drive sessions, Organization administration, Accounts | provisional |
| `DL40` | DL-40 grade sheet | Owns the route-ordered checklist, the graded road test, the suggested route, the captured signatures, and the printable grade sheet | Drive sessions, Organization administration, Phone client, Accounts | provisional |
| `HOUR` | Training hours | Owns hours recorded from completed drives and the progress shown against the configured targets | Drive sessions, Accounts | provisional |
| `OBD` | Vehicle data | Owns the vehicle readings attached to a drive and the adapter's connection status; a missing adapter still leaves the drive usable | Phone client, Drive sessions | provisional |
| `SYNC` | Offline sync | Owns session data recorded without a connection and the upload of that queue once the phone can reach the API | Drive sessions, Phone client | provisional |
| `ADMIN` | Organization administration | Owns an organization's drive plans, maneuvers, score criteria, student limit, and reservation-system settings | Accounts, Reservation system | provisional |
| `MON` | Live observation | Owns an optional live view of an in-progress drive | Drive sessions | provisional |
| `SETT` | App settings | Owns display preferences on the phone, including dark mode | Phone client | provisional |
| (cross-cutting) | Phone client | The only component that reads GPS, the accelerometer, and the OBD-II adapter, and the only one that draws the route on OpenStreetMap | Mobile device sensors, OBD-II adapter, OpenStreetMap | provisional |
| (cross-cutting) | Drive analysis | Owns the optional request for analysis of a recorded drive | AI service | provisional |

Every row is provisional at Checkpoint 1. Mobile device sensors, the OBD-II adapter, and OpenStreetMap appear only on the Phone client row, which is the split section 4 describes (`ROB-obd2`, `SAF-interaction`). The reservation system appears only on Organization administration. The AI service appears only on Drive analysis. Drive sessions stores the route the phone client records. Training hours is specified under `HOUR` and still has to be added in the proof of concept.

_[Check before Checkpoint 1: every area in your use case file appears in the first column, and every external system in section 3 appears in some Depends on cell.]_

## 6. Runtime View

_Due: Checkpoint 2. [One sequence diagram, for the use case your proving slice builds, from the user's action through every container and external system it touches. Leave this section empty until the slice exists; a sequence diagram of code nobody has written describes a guess._

_Draw it as a mermaid `sequenceDiagram`, and name the participants exactly as the containers in section 5.1 name them. If the use case calls an external system, show what happens when that system fails or does not answer; arc42 counts error scenarios among the most useful runtime views. Under the diagram, a sentence or two on anything a reader would not guess from it. Cite the use case by its `UC-*` identifier; do not restate its steps.]_

## 7. Deployment View

_Due: Checkpoint 3. [Filled in once your pipeline exists, after week 11. Three things:_

- _**Where each container runs.** Every container in section 5.1 is mapped to the host, service, or device it runs on, in each environment you have (at least development and production). A table is enough; a diagram helps once there are more than two hosts._
- _**How a change gets there.** From a merged pull request to production: what builds it, what tests it, and where it is released first._
- _**What survives a restart.** Which state is in the database or a file store, and which is lost when the application restarts._

_Section 4.4 of [vision and scope](../requirements/vision-and-scope.md) says who can operate the system and where its users are. Cite it; this section says how the deployment meets it.]_

## 8. Crosscutting Concepts

_[arc42 leaves this section an open list of concepts. This template fixes its first entry, 8.1 Security, because Checkpoint 1 asks for the trust boundary; 8.2 holds every other concept.]_

### 8.1 Security

_Due: named at Checkpoint 1, detailed at Checkpoint 2._

_[Four short paragraphs. The last three each cite the `SEC-*` requirement they answer:_

- _**Trust boundary:** the line between what you control and what you do not. Name the container that is the boundary and what sits outside it (the browser, every external system). Every request that crosses it is authenticated and authorized, and it covers every path your deployable answers, framework endpoints included. Project Pulse's Security & Compliance section shows the shape in three sentences._
- _**Authentication:** how a user proves who they are, and who issues the credential (your system, the client's sign-on, a third party)._
- _**Authorization:** the roles, and the rule for what a user may see beyond their role (a patron sees only their own orders). The second part is where most real breaches happen._
- _**Sensitive data:** what personal or regulated data the system stores, in which container, and which external systems receive any of it. How long it is kept and how it is disposed of are already in section 7.4 of your specification; cite them._

_Secrets (passwords, API keys, connection strings) never appear in this document or in the repository. Say where they will live, not what they are.]_

**Trust boundary.** The API is the trust boundary. The browser, the installed PWA, the Capacitor shell, OpenStreetMap, an optional school HTTP integration, and the phone's GPS, motion, and OBD2 adapters sit outside it. Every `/api` path is authenticated and authorized except `/api/health`, `/api/auth/login`, and `/api/auth/register`. A login check on the screen is outside that line.

**Authentication.** The API issues its own JWT after an email and password check (`SEC-account`). The signing secret is the `JWT_SECRET` environment variable, and the process refuses to start when that variable is missing.

**Authorization.** The token carries `examiner`, `instructor`, `parent`, or `student`, or the platform-admin flag (`SEC-permissions`, `SEC-authorization`). A student reads their own drives; an examiner, instructor, or parent reads drives they conducted; a platform admin reads every drive; a roster row is visible only inside its owner or organization (`SEC-driver-data`). The same filter applies on every read and write of a drive, grade, roster row, or signature.

**Sensitive data.** The database holds names, emails, password hashes, dates of birth, permit and license numbers, parent or guardian license numbers and signature images, GPS breadcrumbs, grades, and organization license numbers. The signed-in user's app receives the rows allowed for that caller, and map tiles send coordinates to OpenStreetMap. A configured school integration sends student and appointment records in; its API key lives on that organization's integration row. How long any of this is kept, and how it is disposed of, is section 7.4 of the [specification](../requirements/software-requirements-specification.md). Password hashes are removed before a user object is returned. The signing secret, the database password, and integration API keys live in the environment or on that integration row.

### 8.2 Other concepts

_Due: Checkpoint 1, a subsection for every concept in the table below; then kept current, adding the file that shows each rule once code exists and a new concept whenever one appears. [Anything every component must do the same way. Your agent starts every session with no memory of the last, so a convention that is not written here gets reinvented each time. Write every concept now, while each is still cheap to choose; the last column says when a missing one would start to hurt._

_One short subsection each: the rule in one sentence, why, and the file that shows it done right once one exists. Put the one-line instruction in your charter too, citing this subsection, because the charter is what your agent always reads. Project Pulse's Crosscutting Concepts section is a worked example; its headings differ from this template's.]_

| Concept | The question it settles | When it usually bites |
|---|---|---|
| _Error handling_ | _What does a failure look like to the caller, and where is it caught?_ | _The second endpoint_ |
| _Time and time zones_ | _Whose clock decides a deadline, what zone is stored, and can a test set the time?_ | _The first deadline or "submitted late"_ |
| _API conventions_ | _What shape does every response take, and how are endpoints named?_ | _The second endpoint_ |
| _Code conventions_ | _Which libraries and idioms does every file use, and which are banned? (Formatting belongs to a formatter, not here.)_ | _The first file an agent writes_ |
| _Validation_ | _Where is input checked, and which check is the one that counts?_ | _The first form_ |
| _Configuration and secrets_ | _What differs between development and production, and where does it live?_ | _The first deploy_ |
| _Logging_ | _What is logged, at what level, and what must never be?_ | _The first bug you cannot reproduce_ |
| _Persistence and concurrency_ | _Where does a transaction begin and end, and what happens when two people edit at once?_ | _The first shared record_ |
| _Auditing_ | _Who changed what, and when?_ | _The first "who did this?"_ |
| _Testing_ | _Which kinds of test, at which layer, with what data?_ | _The first pull request_ |

_Example, from the Cafeteria Ordering System:_

_**8.2.1 Error handling.** Every endpoint returns `{ "ok": false, "error": { "code", "message" } }` on failure, produced by one exception handler; no controller builds its own error body, and no response carries an exception's own message. Why: the ordering screen and the menu screen share one error display, and an exception's message can reveal the database behind it. Shown in: `ApiExceptionHandler`._

File paths below are in the client's proof of concept (`drive-tracker`), which this system builds on (`CO-existing-application`). They are the current picture of each rule.

**8.2.1 Error handling.** A failed request is an `Error` carrying a `statusCode`, thrown from the service and rendered by Fastify's sensible plugin. Why: every screen shares that error path, and a database exception stays out of the response text. A missing or expired token is 401, and the client clears the stored token and returns to login; the wrong role is 403. Missing GPS, motion, or OBD2 stays on the device (`ROB-gps`, `ROB-obd2`). A school integration that returns the wrong shape is 502. Shown in: `api/src/services/AuthService.js`, `frontend/src/boot/axios.js`.

**8.2.2 Time and time zones.** Instants are UTC from the API process clock, and calendar dates (date of birth, appointment date) are `YYYY-MM-DD` with no zone; the UI shows instants in the device's local zone. Why: a date of birth run through local midnight falls on the wrong day. A test cannot set the clock yet. Shown in: `api/src/services/SessionService.js`, `api/src/services/RosterService.js` (`normalizeDate`).

**8.2.3 API conventions.** Resources are `/api/<resource>` in kebab-case, JSON in and out, with an integer id in the path. One resource is returned as the object; a paged list is `{ data, total, page, limit }`; login and register return `{ user, token }`. Why: the next route otherwise invents another envelope. Shown in: `api/src/app.js`, `api/src/services/SessionService.js` (`listForUser`).

**8.2.4 Code conventions.** Frontend files are Vue 3 single-file components, Quasar components, Pinia stores, and Quasar boot files. Backend files are JavaScript ES modules: a Fastify route or plugin, and a service class that receives the Knex connection (`CO-frontend-framework`, `CO-backend`). Why: that is the shape of the proof of concept, and a second HTTP client or database library would split it. Shown in: `frontend/src/stores/auth.js`, `api/src/services/AuthService.js`.

**8.2.5 Validation.** The route's JSON Schema rejects a body of the wrong shape, and the service normalizer is the check that counts for a domain rule. Why: the schema cannot say that the parent or guardian section is all-or-nothing. A value that passes the schema and fails the normalizer is still rejected. Shown in: `api/src/routes/students.js`, `api/src/services/RosterService.js` (`normalizeRosterStudent`), `api/src/routes/sessions.js`.

**8.2.6 Configuration and secrets.** Development and production differ only by environment variables: the database connection, `JWT_SECRET`, `JWT_EXPIRES_IN`, `CORS_ORIGIN`, `PORT`, and the frontend `API_URL`. Production starts only when `JWT_SECRET` is set. Why: those are the values that change between a laptop and the deployed host. This document names the variables. Shown in: `api/knexfile.js`, `api/src/app.js`, `frontend/quasar.config.js`.

**8.2.7 Logging.** Application logs go through Fastify's logger, pretty-printed only when `NODE_ENV` is `development`. Log database connectivity and a failed integration call. A log line never includes a password, a token, a signature image, a license or permit number, or an integration API key. Why: the request that explains a bug is also the request that carries those values. Shown in: `api/src/app.js`.

**8.2.8 Persistence and concurrency.** One Knex connection talks to MySQL. A delete sets `deletedAt`, and reads omit those rows. A service method that writes more than one table opens and commits its own transaction. Two saves of the same session both succeed, and the later update is the one stored. Why: two people can grade or edit one drive, and the proof of concept has no row version. Shown in: `api/src/plugins/database.js`, `api/src/utils/audit.js`.

**8.2.9 Auditing.** Who last wrote a row is `createdBy` and `updatedBy` on that row, together with `createdAt`, `updatedAt`, and `deletedAt`. Creating a drive copies the student's name, date of birth, permit number, and school onto the session. Why: a later roster edit would otherwise rewrite the drive that was graded. The stamps keep the latest writer, not the previous value. Shown in: `api/src/utils/audit.js`, `sessionIdentityFromRosterAndAppointment` in `api/src/routes/sessions.js`.

**8.2.10 Testing.** A rule about which rows a caller may see is a Node built-in test (`node --test`) against MySQL, inside a transaction that rolls back. Why: that rule can break in a pull request with no visible change on the screen. The proof of concept has no frontend test suite. Shown in: `api/test/roster-service.test.js`.

## 9. Architecture Decisions

_Due: the table and one decision at Checkpoint 1; more as they are made._

### 9.1 Architecturally significant requirements

_[Not every requirement shapes the architecture. The **architecturally significant requirements** are the few that do: quality attributes and constraints where a wrong guess costs a redesign, not a bug fix. Functionality can be delivered by many structures; these are what choose among them._

_Your quality goals from section 1.2 are usually the top rows; cite them by identifier and do not explain them again. This table can also hold what is nobody's goal but still forces structure, such as a `CO-*` constraint._

_List three to six, ranked by importance to your client times difficulty to achieve. Reuse the specification's identifiers, never new ones. **At least one row is a `SEC-*` attribute.** Every system your team builds this year holds some personal data, and if no security requirement appears here, that data's protection was never designed; it will be added later, which is where security bugs come from.]_

| Rank | Requirement | Specification handles | Importance × difficulty | Drives |
|---|---|---|---|---|
| 1 | Users see only their own organization's and students' data | `SEC-authorization`, `SEC-driver-data`, `SCA-organizations` | High × Medium | `KD-org-row-tenancy`, `KD-own-accounts` |
| 2 | A drive survives losing the network or GPS mid-drive | `ROB-external`, `ROB-gps`, `ROB-failure-handling`, `UC-SYNC-sync-offline-data` | High × High | `KD-offline-first-capture` |
| 3 | OBD2 vehicle data reaches the app on the devices graders use | `INT-obd2`, `ROB-obd2`, `CO-pwa`, `CO-capacitor` | High × High | `KD-obd-adapter-layer` |
| 4 | Logging an infraction does not distract the grader | `SAF-interaction`, `PER-infraction` | High × Medium | `KD-offline-first-capture` |
| 5 | Build on the existing application and its deployment pipeline | `CO-existing-application`, `CO-github-actions` | Medium × Low | `KD-deployment-shape` |

`AVL-uptime` and `SCA-concurrent-users` are not listed: the client's existing hosting meets them, and neither forces a structural choice.

### 9.2 Key decisions

_[One entry per key decision (`KD-*`), in the form below; it is what the wider industry calls an architecture decision record (ADR). Checkpoint 1 requires exactly one: **`KD-deployment-shape`**, whether your system ships as one deployable or several, and why. Every team makes this decision, and it is where over-engineering usually shows up first. Add others when you make them; do not invent them to fill the section._

_A decision without a **rejected alternative** is not a decision, it is a description. Name what you did not do and why not, so the next person does not redo the argument._

_A decision that turns out wrong is not deleted or rewritten. Mark it **Superseded by `KD-<new-slug>`** and write the new decision as its own entry, so the reasoning behind both stays readable.]_

**`KD-deployment-shape`: one repository, two deployables (static UI and API), one shared database.** Accepted (inherited from proof of concept).

- **Driving requirements:** `CO-existing-application`; `CO-github-actions`; `CO-capacitor`.
- **Context:** The client's Drive Tracker repository holds the API and the front end together, and one push to `stg` deploys both through the existing GitHub Actions workflow to the client's Docker Swarm host. The database is a MySQL instance on the client's cluster. Expected load is in the low hundreds of users (`SCA-concurrent-users`).
- **Decision:** The Quasar front end is built to static files and served by its own container. The Node.js API runs in its own container. Both live in one repository and deploy together from one push, against one MySQL database.
- **Rejected:** (a) Bundling the front end into the API's package as a single deployable. The Capacitor build needs the front end as a standalone bundle that calls the API from the device, and the client's pipeline already ships the two separately. (b) Splitting the API into services by area (sessions, grading, administration). That would add network calls, more deployments, and failure modes between them, to solve a scaling problem this user count does not have.
- **Trade-off:** The UI and the API can drift apart if only one of the two deploys succeeds, so a release is not finished until both containers report the same version.

**`KD-own-accounts`: Drive Grader issues its own credentials.** Accepted (inherited from proof of concept).

- **Driving requirements:** `SEC-account`; `SEC-permissions`.
- **Context:** Users are parents, students, instructors, and examiners from many unrelated households and driving schools. There is no shared identity provider among them, and the client does not run one.
- **Decision:** The API stores password hashes and issues signed, expiring tokens. Each token carries the user's role and whether they are a platform administrator, and the API checks both on every protected route.
- **Rejected:** An external identity provider or school sign-on. Parents have no common provider, and adding one would put a third-party dependency in front of every drive.
- **Trade-off:** The team owns password reset (`UC-ACCT-reset-password`), credential storage, and the signing secret.

**`KD-org-row-tenancy`: one database, with every tenant-owned row scoped to its organization.** Accepted (inherited from proof of concept).

- **Driving requirements:** `SCA-organizations`; `SEC-driver-data`; `SEC-authorization`.
- **Context:** Several driving schools and many families share one deployment. A parent must see only their own students, and an organization only its own drives.
- **Decision:** All organizations share one database. Every tenant-owned row carries its organization, and parent access is further limited to the students on that parent's roster. The API applies these scopes in the service layer, not in the front end.
- **Rejected:** A separate database or schema per organization. It would multiply migrations and operational work on the client's cluster, and no requirement demands that level of isolation.
- **Trade-off:** One query that forgets its scope leaks data across tenants, so every protected function needs an authorization test with an account from another organization (`SEC-authorization`; see section 10.2).

**`KD-offline-first-capture`: drive data is written on the device first and synced to the API when the network allows.** Accepted (inherited from proof of concept).

- **Driving requirements:** `ROB-external`; `ROB-gps`; `PER-infraction`; `SAF-interaction`; `UC-SYNC-sync-offline-data`.
- **Context:** Drives happen in moving cars, where mobile coverage drops without warning. A grader logs infractions and maneuvers while supervising a teenage driver and cannot stop to retry a failed request.
- **Decision:** During an active drive the front end queues route points, grades, motion events, and OBD2 samples in on-device storage, and uploads them when connectivity returns. The API accepts queued data for an existing session.
- **Rejected:** Online-only capture, where each grade or route point is sent immediately. A dropped connection mid-maneuver would either lose the record or make the grader retry it while the car is moving.
- **Trade-off:** On-device storage is limited in size and can be cleared by the operating system before it syncs. Starting a session still needs the network, because the API assigns the session's identifier. Both are to be recorded as debt in section 11 at Checkpoint 2.

**`KD-obd-adapter-layer`: vehicle data goes through one interface with swappable transports, and iOS uses the native build.** Accepted (inherited from proof of concept).

- **Driving requirements:** `INT-obd2`; `ROB-obd2`; `CO-pwa`; `CO-capacitor`.
- **Context:** The OBD2 adapters connect over Bluetooth. Android browsers can reach them from the PWA, but iOS browsers and home-screen PWAs cannot open Bluetooth at all. The client's near-term milestone is a live brake signal in the app.
- **Decision:** The front end talks to the vehicle through a single connection interface, with interchangeable transports for Web Bluetooth, USB serial, native Bluetooth through Capacitor, and a simulator. The PWA uses whichever transport the browser supports. iOS users need the Capacitor build for OBD2, and everything else works without OBD2 (`ROB-obd2`).
- **Rejected:** (a) PWA only, which leaves every iOS user without vehicle data. (b) Native apps only, which drops the install-free PWA tier the client described for GPS-only use.
- **Trade-off:** Two builds to test, and OBD2 features differ by platform. The simulator lets the team test grading without a car, but it does not prove that a real adapter works.

**`KD-integration-isolated`: only the integration component talks to an organization's external scheduling and student system.** Proposed; the team confirms whether it is in scope this semester.

- **Driving requirements:** `ROB-external`; `INT-integrations`; `SI-SCHOOL-INTEGRATION`; `DE-school-integration`.
- **Context:** An organization can configure its own scheduling and student system. Drive Grader pulls students and appointments from it and pushes finished drive results back, using a per-organization address and key.
- **Decision:** One API component owns every call to that system, its credentials, and the translation of its data into Drive Grader's. When the system fails or does not answer, the component returns an empty result instead of an error, so drives and grading keep working.
- **Rejected:** Calling the external system from each feature that needs its data. That spreads its credentials and data formats across the codebase, and one outage would break several features.
- **Trade-off:** A silent empty result can hide an outage, so a failed call must still be visible to an organization administrator, for example through the connection test.

## 10. Quality Requirements

### 10.1 Quality requirements overview

_[Section 9 of your [specification](../requirements/software-requirements-specification.md) is the overview. Link it here; do not copy it.]_

### 10.2 Quality scenarios

_Due: one scenario at Checkpoint 2; one per top-ranked requirement in section 9.1 by Checkpoint 3._

_[A quality attribute says how good; a scenario says how you will know. Each one is: a **source** does a **stimulus** in an **environment**, the system gives a **response**, and a **measure** tells you it worked. The measure cites the specification's attribute for its number; it never introduces one._

_**Verified by** names the test, or the repeatable manual check, that shows the measure holds. Leave it empty until that test exists; an empty cell is an honest "not yet verified".]_

| ID | Source and stimulus | Environment | Response | Measure | Verified by |
|---|---|---|---|---|---|
| _`QS-cross-employee-order-denied`_ | _A signed-on patron requests another patron's order by its ID_ | _Normal operation_ | _Refused before any order data is read_ | _Every such request is refused and returns no order fields (`SEC-employee-own-orders`)_ | _An integration test that signs in as one patron and requests another patron's order_ |

## 11. Risks and Technical Debt

_Due: Checkpoint 2, kept current after._

_[**Technical** risks and debt only. Business risks are `RI-*` in [vision and scope](../requirements/vision-and-scope.md); do not copy them here. Project risks, such as a teammate dropping the course, belong in neither document. Seed this list from the technical `RI-*` items and from any [OPEN-ISSUES.md](../requirements/OPEN-ISSUES.md) entry whose answer could change the architecture._

_A **risk** might happen: an external system you have never called, a client dataset you have never seen. **Debt** has already happened: a shortcut you took on purpose and intend to pay back. Each row says how you would find out, or how you would fix it._

_A risk written as a category ("security", "performance") is not a risk. Write the mechanism: what fails, and what that breaks.]_

| ID | Type | What could go wrong, and what it breaks | Mitigation or fix | Cites |
|---|---|---|---|---|
| _`RISK-payroll-api-unavailable`_ | _Risk_ | _Nobody has seen the Payroll System's interface. If it only accepts a nightly batch file, ordering cannot confirm payment at order time._ | _Ask for the interface document at the next client meeting; build Payment against a stub until then._ | _`DE-payroll-integration`, `OI-4`_ |

## 12. Glossary

_[Domain terms live in your [project glossary](../requirements/project-glossary.md). Link it and add nothing here unless you need an architecture term your team uses in a special sense.]_

---

## Working this document with your agent

_[Delegate: drawing the C4 diagrams in mermaid from your use case list and your specification's interfaces; checking that every use case area has a component and every external system has a component that depends on it; checking that every identifier this document cites exists in the document that owns it; drafting the rejected alternative for a decision you have already made._

_Keep human: the ranking in section 9.1 and every `KD-*`. The decisions are the part of this document your client and the team that inherits this system will hold you to, and they depend on facts about your client that are not in any file._

_**The specific failure to watch for: over-engineering.** Ask an agent for an architecture and it will propose the one it has seen most often in writing, which is built for a company a thousand times your size: microservices, a message queue, Kubernetes, a cache in front of a database that holds ten thousand rows. Every one of those is a real answer to a problem you do not have, and each one adds something that can break at 2 a.m. with nobody to fix it. For every container and every decision the agent proposes, ask which requirement in section 9.1 forces it. If the answer is none, cut it.]_

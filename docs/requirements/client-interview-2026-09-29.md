# DriveGrader – Client Meeting Notes (Eric Brown)

Consolidated notes from the Zoom meeting with Eric Brown (two recordings: Part 1 and Part 2, split by the free Zoom 30-minute limit) and his follow-up email.

> Note: Notes are based on a machine transcription of noisy audio. Minor names and terms may be slightly off.

---

## Quick Reference

| Item | Value |
|---|---|
| Staging app (UI) | https://tcu-ui.drivegrader.com |
| Staging API | https://tcu-api.drivegrader.com |
| Deploy branch | `STG` (staging) – auto-deploys via GitHub Actions |
| Repo name | "Drive Tracker" (it is the DriveGrader app; API + front end in one repo) |
| Current app version | 1.0.6 (tracked in `package.json`) |
| BMAD resources | BMAD Method website (markdown files) |
| Client contact | Eric Brown – text or email; busy the day after the meeting, otherwise available |
| Demo day | ~3 weeks from the meeting |

---

## Part 1 – Meeting

### OBD2 / CAN Signals
- The team reported that the tested OBD2 adapter does not capture **CAN signals** (brake, turn signal, gear). Its documentation says support depends on the vehicle. Adapters that can do this exist but are expensive.
- Eric: not a big deal. **Track what you can.** OBD2 still provides rapid acceleration, distance, etc.
- Consider using the **phone's accelerometer** as an additional data source (unclear how well the app currently supports it).

### Dev Setup & Deployment
- Everyone should have a **Cursor** account and have pulled the GitHub repo.
- API and front end live in the same repo, so one push deploys both.
- **Push to `STG`** → GitHub Actions runs automatically → changes live within 1–2 minutes.
  - Check the repo's **Actions** tab; green = success.
  - Actions previously failed, possibly because build logic tries to create a database that already exists. If it fails, ask the AI to troubleshoot.
- **Do not push to `main`**; DNS pointers for main aren't set up yet.
- **Branching:** each person can work on their own branch to avoid overwriting each other. Push your branch, then open a **pull request into `STG`** to see changes live.

### Cloudflare (Eric's recommendation)
- Domain registrar/DNS host (like GoDaddy) with more features. Eric manages ~715 domains there.
- Features: DDoS mitigation, subdomain pointers (`api.`, `ui.`), email routing (e.g., `support@` any domain → one inbox), redirects, automatic SSL when proxied.

### BMAD
- A set of AI agent prompts used in Cursor. **Party mode** spins up multiple agents (e.g., "Winston" the architect) to brainstorm.
- Demoed by asking about the CAN signal problem; it suggested options such as a real CAN adapter.
- Warning: the AI often overestimates effort ("two weeks") for work it can do in minutes. Push back and ask whether that estimate assumes the AI is doing it. Eric has rarely seen a task take the AI more than 10–60 minutes.
- The repo's BMAD folder may contain only one file; download the rest from the BMAD Method website if needed.

### End of Part 1
- Eric was locked out of staging (forgot his password) and may add an override. The Zoom call then timed out.

---

## Part 2 – Meeting

### Fixes & Tips
- Eric had the AI add an **API health check** so login shows an "API unavailable" banner instead of silently failing.
- Recommended an **internet connectivity indicator**, since the app tracks drives from a moving car.
- **Peacock** extension (Cursor/VS Code): color-code windows when working on multiple apps.
- Local run: `npm run dev` in both the API folder and the UI folder. He hit an env-variable / port 3100 issue.
- Debugging: paste errors or **screenshots** into the AI (screenshots also work for UI alignment fixes).

### App Walkthrough
- **SaaS admin:** settings area; should only appear for admins via a **"platform admin"** switch on the user profile.
- **Getting admin access:** change your user record in the database, or Eric offered to set your profiles as admins. **Follow up:** confirm whether the team has connection details for the cluster database.
- **Drive setup:** session type (road test / practice), student, weather.
  - **Expert mode:** hides descriptions.
  - **Learning mode:** intended to show explanatory videos.
  - **Simulated GPS:** test without being in a car.
- Deploy it and **drive-test it as a team** to see what works in real life. It was "thrown together," so redesign freely (map size/position, pop-ups, menus, icons, colors, profile avatar dropdown, etc.).
- Export the database schema to the AI and ask for improvements and performance tuning (e.g., caching). "You don't know what you don't know" until production traffic hits.

### Demo Day Goal
- **Key milestone:** live OBD connectivity where **hitting the brake is reflected in the app** (e.g., "hard braking"). Proving one signal works is the big benchmark; the rest are similar.
- **Open question:** does Bluetooth work in **PWA** mode, or does it require a **Capacitor** native wrapper? The AI can add Capacitor and explain running it on a phone via Xcode.
- **Possible two-tier product:**
  - PWA: limited features (GPS, speed, possibly accelerometer).
  - Native app: full features including Bluetooth/OBD.

### Product Ideas from Eric
- **Parent tool:** parents teaching teens to drive use the app to know what to look for.
- **Live parent viewing:** during lessons with Eric's instructors, parents get a separate login to watch their teen's drive and score in real time ("like tracking an Uber").
- **Scheduling:** show upcoming drives and keep a history of completed ones.
- **Data separation:** parents see only their own kids' drives. Support multiple students (siblings/twins are common).
- **More session types:** highway, city, night, parallel parking, etc.
- **Reporting:** e.g., "Justin drove on these dates for this long." None exists yet.

### Eric's General Advice
- Experiment: "break it, fix it." Try ideas; if one doesn't work, try another.

---

## Follow-Up Email – Benchmark Ideas

1. **API down feedback at login.** Unlikely to happen, but good to have. (Started during the call; confirm it's pushed.)
2. **Internet connectivity indicator.**
3. **Environment/staging banner.** A banner across the top showing the environment (Staging, Dev). **No banner in production.**
4. **OBD II connection indicator.** The connection shouldn't be required, but show a visual status: connected / not connected, "click to connect," plus the full set of actions for setting up the connection.
5. **Student limits per profile.** Gating the product to N students is the business model.
6. **Reporting on completed drives.**
7. **Filtering drives** by date, student, instructor, etc. (goes hand in hand with reporting).
8. **Versioning + "What's New":**
   - Version is currently **1.0.6**, tracked in `package.json`.
   - Increment the **patch version** with each update.
   - Add a **"What's New"** section on the **About** screen.
   - Workflow: after each change, tell Cursor to *"increment the patch version and update what's new"* so it's self-documenting.
9. **Aerial layer** on the OpenStreetMap map.
10. **Dark mode** option.
11. **Install prompts:** encourage users to install the PWA (or the App Store app later) with in-app instructions. Cursor can work this out.

---

## Team Discussion (after Eric left)
- Hold an **in-person brainstorm session** (likely next week's meeting) to generate ideas and divide the work.
- Eric likely won't provide specific requirements this semester, so the **team should define its own goals** (e.g., the GPS feature) and tell him what we want to be graded on.
- Summarize the meeting recordings with AI and **push the notes to the team repo**.
- Meeting location TBD (library or senior design room). No class the next day. An assignment is due Friday.

---

## Action Items

- [ ] Push to `STG` and confirm the GitHub Actions deploy succeeds.
- [ ] Get admin access (ask Eric, or get cluster database connection details).
- [ ] Confirm the API health check / "API down" banner is committed and working.
- [ ] Add internet connectivity and environment (Staging/Dev) banners.
- [ ] Build the OBD II connection indicator and connect flow.
- [ ] **Demo day:** show a brake signal live in the app; decide PWA Bluetooth vs. Capacitor.
- [ ] Set up versioning + "What's New" on the About screen.
- [ ] Plan student limits, reporting, and drive filtering (by date, student, instructor).
- [ ] Nice-to-haves: aerial map layer, dark mode, install prompts.
- [ ] Drive-test the app as a team and list UX improvements.
- [ ] Hold the team brainstorm, set goals, and divide the work.
- [ ] Push these notes to the team repo.

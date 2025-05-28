# life-manager-app
# Website Specification – **Personal Life Manager**

> **Document purpose**: Single‑source blueprint to build and maintain a private, cross‑device life‑manager web & mobile app.

*Last updated: 2025‑05‑27 (rev‑2)*

---

## 1 Vision & Goals

* **Elevator pitch** A private, always‑accessible digital hub that unifies every facet of your life—ideas, schoolwork, meetings, projects, shopping, tasks, notes—into one intuitive workspace so you can replace the mental juggling act with calm clarity.
* **Primary objectives**

  1. Become the single source of truth for planning, capture, and review.
  2. Make adding an item friction‑free (< 5 seconds via natural language input).
  3. Surface the *right* next actions contextually (Today, Upcoming, Inbox).
  4. Sync instantly across desktop web and mobile PWA.
* **Success signals (qualitative)**

  * Daily usage streaks
  * % tasks completed on or before due date
  * Self‑reported stress reduction

## 2 Target Audience & Personas

| Persona ID | Description         | Needs & Pain Points                                                                   | Goals                                                                  |
| ---------- | ------------------- | ------------------------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| **P‑1**    | You (primary user)  | Chaotic schedule spanning school, work, projects, errands; existing tools too siloed. | Capture everything instantly and retrieve contextually across devices. |
| **P‑2**    | Future private user | Same as P‑1 but with different life categories.                                       | Tailor categories to their life mix.                                   |

## 3 Value Proposition & Differentiators

> *“Life management that bends to **your** categories instead of bending your life to an app.”*

* Sidebar categories are editable and can include custom ones.
* Natural‑language quick add parses free‑form text into tasks/events/notes ("Pay rent tomorrow 9 am" → Task with due date).
* Built‑in weekly recurring logic (lectures, supplements, etc.).
* Private by default, ad‑free, no social layer.

## 4 User Stories (selected)

| ID  | Priority | User Story                                                                     | Acceptance Criteria                                                      |
| --- | -------- | ------------------------------------------------------------------------------ | ------------------------------------------------------------------------ |
| U‑1 | High     | As a user, I want to add anything in < 5 sec via one input bar.                | Global quick‑add accepts natural language, categorises, and stores item. |
| U‑2 | High     | As a user, I want to see what to do today across all categories.               | Dashboard "Today" list aggregates tasks/events due before midnight.      |
| U‑3 | High     | As a student, I want lectures & problem sets to auto‑appear weekly.            | Recurring template creates tasks/events per schedule.                    |
| U‑4 | Medium   | As a user, I want inventory reminders for supplements that run out in 30 days. | Countdown shown; optional alert 3 days before depletion.                 |
| U‑5 | Low      | As a night‑owl, I want dark mode.                                              | Theme toggle persists per device.                                        |

## 5 Information Architecture

```
Sidebar (persistent)
  Dashboard
  School
  Work
  Shopping  (To‑Buy)
  Tasks      (Global)
  Socials
  Meetings / Contacts
  Projects
  Notes
  Diary
  Profile (avatar)
```

---

## 6 Functional Requirements

### 6.1 Core Features

1. **Cross‑device account** – Email+password; data synced across web & mobile PWA.
2. **Dashboard**

   * Greeting header ("Hello, Joe 👋").
   * Today: aggregate of due tasks/events.
   * Upcoming: next 7 days.
   * Quick‑add bar (natural language input + icons for Task, Note, Event, Idea).
3. **Natural‑Language Quick‑Add Engine**

   * NLP parses date/time, recurrence ("every Tue"), category hashtags ("#school"), and **leading category tokens** (e.g., typing `Work meeting 12:30` auto‑assigns to the *Work* category).
   * Defaults to Inbox if category not specified.
4. **Category Pages** (School, Work, Shopping, etc.)

   * List & board views.
   * Filtered to that category.
5. **Recurring Task Engine**

   * Supports daily/weekly/monthly/"every X days" rules.
   * When item marked complete, next instance auto‑scheduled.
6. **Inventory Countdown (Shopping➝Supplements)**

   * Quantity + servings/day → predicted run‑out date.
7. **Diary** – Markdown editor; auto‑new page per calendar day.
8. **Projects Module**

   * Multiple projects; each with kanban lanes or checklist.
9. **Meetings / Contacts**

   * Store contact info, notes, and meeting entries; list & calendar view.
10. **Tasks Module** (Global)

    * Kanban/table toggle; filters; bulk actions.

### 6.2 Nice‑to‑Have (Backlog)

* AI suggestions (priority nudges).
* Google/Outlook calendar sync.
* Push/email notifications for deadlines & inventory (opt‑in).

---

## 7 Non‑Functional Requirements

| Area          | Requirement                                                   |
| ------------- | ------------------------------------------------------------- |
| Performance   | Dashboard FCP < 1 s on broadband                              |
| Availability  | 99.5 % uptime                                                 |
| Security      | TLS; salted+hashed passwords; Postgres data encrypted at rest |
| Privacy       | No 3rd‑party analytics; data not shared                       |
| Accessibility | WCAG 2.2 AA                                                   |

## 8 UI / UX Design

* Minimal, card‑based dashboard (Notion × Linear vibe).
* Neutral/light palette + optional dark mode.
* Category accent color (small dot icon).
* 12‑column responsive grid (1280, 768, 480 px breakpoints).
* Components: Card, list row, kanban lane, modal, markdown editor.

---

## 9 Page‑by‑Page Specs (v1)

### 9.1 Dashboard

| Zone     | Component              | Details                                   |
| -------- | ---------------------- | ----------------------------------------- |
| Header   | Greeting, streak badge | Pull preferred name; show diary streak.   |
| Column 1 | **Today**              | Tasks & events due today, sorted by time. |
| Column 2 | **Upcoming**           | Rolling 7‑day lookahead.                  |
| Footer   | Quick‑add bar          | Text input + action icons.                |

### 9.2 School

* **Structure**: Nested list → Course → (To‑Do, Due).
* **Auto‑Templates**: Each course can define recurring "Lecture (Tue/Thu)" and "Problem Set (post Wed → due Tue 23:59)" rules.
* **Views**: Table (default) & calendar (due dates).

### 9.3 Work

* Two default buckets per project: "To‑Do" & "Due".
* Recurrence allowed (e.g., "Check work email" daily weekday 08:00).

### 9.4 Shopping / To‑Buy

* Category tabs: Food, Clothing, Supplements, Other.
* **Supplements** items have fields: quantity\_servings, servings\_per\_day → run‑out date & countdown.

### 9.5 Tasks (Global)

* Inbox (uncategorised) + board grouped by Category.

### 9.6 Meetings & Contacts

* List view (sortable); calendar toggle.
* Meeting modal: title, date/time, location/link, attendees, notes.

### 9.7 Projects

* Project list → click → kanban (To Do, Doing, Done lanes) or checklist.
* Supports nested subtasks & file attachments.

### 9.8 Notes

* Simple markdown notes; tag with Category.

### 9.9 Diary

* One page per day; timeline scroller; markdown.

### 9.10 Summarizer (Today View)

* Generates plain‑text agenda using data from Today & Upcoming.
* Option: export to clipboard.

### 9.11 Profile

* Manage account, theme toggle, data export (JSON / CSV / Markdown).

---

## 10 Data Model (key excerpts)

| Entity         | Fields                                                                                                               |
| -------------- | -------------------------------------------------------------------------------------------------------------------- |
| User           | id, email, hashed\_pw, name, created\_at                                                                             |
| Category       | id, user\_id, name, icon, color                                                                                      |
| Task           | id, user\_id, category\_id, title, description, status, priority, due\_date, recur\_rule, created\_at, completed\_at |
| Event          | id, user\_id, category\_id, title, start\_at, end\_at, location, recur\_rule                                         |
| Note           | id, user\_id, category\_id, content\_md, created\_at                                                                 |
| Project        | id, user\_id, name, description, status                                                                              |
| ProjectTask    | id, project\_id, title, status, due\_date                                                                            |
| SupplementItem | id, user\_id, name, quantity\_servings, servings\_per\_day, category\_id                                             |
| DiaryEntry     | id, user\_id, date, content\_md                                                                                      |

---

## 11 Integrations (Backlog)

* Google Calendar two‑way sync.
* Email/push via Postmark or Firebase.

## 12 Tech Architecture

| Layer      | Stack                            | Reason                                     |
| ---------- | -------------------------------- | ------------------------------------------ |
| Front‑end  | Next.js + React + TypeScript     | SSR & SPA hybrid; large ecosystem          |
| State Mgmt | TanStack Query + Zustand         | Offline caching, local mutations           |
| Mobile     | PWA (homescreen installable)     | No separate native app v1                  |
| Back‑end   | NestJS (Node)                    | Structured, modular, TypeScript end‑to‑end |
| DB         | Supabase (PostgreSQL)            | Managed, auth & storage included           |
| Hosting    | Vercel (front) + Supabase (back) | Simple CI/CD                               |

## 13 Auth & Roles

* Single "Owner" role.
* Email/password (bcrypt); JWT in HttpOnly cookies.
* Optional MFA (TOTP).

## 14 Admin & CMS

* Supabase Studio for DB; minimal internal admin panel for content tweaks.

## 15 Observability

* Sentry for errors; self‑hosted PostHog for usage metrics.

## 16 Testing

| Layer       | Framework                      |
| ----------- | ------------------------------ |
| Unit        | Vitest                         |
| Integration | Supertest                      |
| E2E         | Playwright (Chromium + WebKit) |

## 17 DevOps / CI

* GitHub Actions → build, test, deploy.
* Preview URLs per PR via Vercel.
* Nightly DB backups.

## 18 Roadmap

* **v1** Web (desktop) MVP: Dashboard, natural‑language quick add, Tasks, School, Work, Shopping, Meetings, Diary.
* **v1.1** Mobile PWA polish, offline capture.
* **v2** Calendar sync, push notifications, AI suggestions.

## 19 Risks & Mitigations

| Risk           | Impact | Strategy                                                     |
| -------------- | ------ | ------------------------------------------------------------ |
| Scope creep    | Medium | Freeze v1 scope; backlog new ideas.                          |
| NLP mis‑parses | Medium | Provide edit dialog on capture; allow fallback manual entry. |
| Data loss      | High   | Automated backups + export.                                  |

## 20 Glossary

| Term                       | Definition                                                    |
| -------------------------- | ------------------------------------------------------------- |
| Inbox                      | Unassigned tasks awaiting triage.                             |
| Natural‑Language Quick‑Add | One‑line input that converts sentences into structured items. |
| Category                   | User‑defined life area (School, Work, etc.).                  |

## 21 References

* Notion – modular pages.
* Fantastical – natural language input.
* Things 3 – task UX.
* Linear.app – minimal, fast interactions.

---

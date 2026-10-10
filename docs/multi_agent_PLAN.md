# [project name] — [one-line tagline]

## Project Specification

> **How to use this template**
>
> - This document is the shared contract between you and every coding agent working on the project. Agents read it before doing anything, so write it as the single source of truth: decisions, not options.
> - Replace every `[placeholder]`. Replace each `> Guidance:` block with real content, then delete the guidance.
> - Delete sections that don't apply (for example, §9 for a project without AI, or §10 for an API-only service) and renumber.
> - Be specific. Exact field names, status codes, defaults, limits and example payloads remove guesswork and prevent agents from inventing their own.
> - Cross-reference sections (`see §7`) instead of repeating rules, so each rule lives in one place.
> - When part of the system is already built, say so at the top of its section and mark the remaining work with **[change]** (see §6).

## 1. Vision

> Guidance: Two short paragraphs.
>
> - **What it is and who it's for:** what [project name] does, the problem it solves, and the experience it should feel like (compare it to something familiar if that helps, e.g. "feels like a modern X with a Y").
> - **Why it exists / how it's built:** the purpose behind the project (product, demo, course, internal tool) and anything notable about how it will be built, such as agents coordinating through files in `docs/`.

## 2. User Experience

### First Launch

> Guidance: Describe the first run, step by step, from the user's point of view: the command or script they run, the URL or screen that opens, whether there's login or signup, and exactly what they see straight away (default data, starting balances, empty states, panels).

The user runs [start command or script]. [Where the app opens, e.g. `http://localhost:[port]`]. [Login/signup behavior]. They immediately see:

- [Default content shown on first launch]
- [Starting state, e.g. seeded data or sample records]
- [Overall look and feel]
- [Key panels or features ready to use]

### What the User Can Do

> Guidance: One bullet per user-facing capability, written as **verb phrase** — details. Include the rules a user would notice (limits, defaults, what's deliberately left out, whether actions need confirmation). This list drives the API (§8), the frontend (§10) and the E2E scenarios (§12), so keep it complete.

- **[Capability]** — [details and rules]
- **[Capability]** — [details and rules]
- **Reset / start over** — [what a reset restores and what it clears, if applicable]

### Visual Design

> Guidance: The visual rules agents should follow: theme (light/dark, background ranges, what to avoid), animations and their timing, status indicators, density and layout inspiration, and responsive priorities (desktop-first, mobile-first, minimum supported width).

- **Theme**: [e.g. dark, backgrounds around `#xxxxxx`]
- **Feedback and animation**: [e.g. highlight on change, fading over ~Nms]
- **Status indicators**: [e.g. connection status dot and what each color means]
- **Layout**: [e.g. data-dense, spacious, inspired by X]
- **Responsiveness**: [e.g. desktop-first, functional on tablet]

### Color Scheme

- [Role, e.g. Accent]: `#xxxxxx`
- [Role, e.g. Primary]: `#xxxxxx`
- [Role, e.g. Secondary (submit buttons)]: `#xxxxxx`

## 3. Architecture Overview

### [Deployment shape, e.g. Single Container, Single Port]

> Guidance: A simple box diagram showing how the pieces fit together at runtime: what runs where, which ports are exposed, which routes are served by what, where data lives and which background tasks run.

```
┌─────────────────────────────────────────────────┐
│  [container] (port [port])                      │
│                                                 │
│  [middleware] ([programming language])          │
│  ├── /api/*          REST endpoints             │
│  ├── /api/stream/*   [real-time channel]        │
│  └── /*              Static file serving        │
│                      ([frontend framework])     │
│                                                 │
│  [DB type] database ([how it persists])         │
│  Background tasks: [list]                       │
└─────────────────────────────────────────────────┘
```

- **Frontend**: [frontend framework], [how it's built and served]
- **Backend**: [middleware] ([programming language]), managed with [dependency manager]
- **Database**: [DB type], [location and persistence]
- **Real-time data**: [e.g. Server-Sent Events, WebSockets, polling, or none] — [why]
- **AI integration**: [AI provider and model, how outputs are structured]
- **External data/services**: [what's used, and how the app chooses between real and simulated sources]

### Startup

> Guidance: Everything that happens before the app accepts requests, in order. Make each step idempotent and say where it runs (e.g. the framework's startup/lifespan hook). Avoid deferring work to the first request, which causes races.

1. [e.g. Create the database schema and seed default data (idempotent — see §7)]
2. [e.g. Start background services (see §6)]
3. [e.g. Record initial state, then start periodic tasks]

### Why These Choices

> Guidance: One row per significant decision, including deliberate simplifications ("X only", "no Y"). The rationale stops agents from "improving" a choice that was made on purpose.

| Decision | Rationale |
|---|---|
| [Choice A over B] | [Why] |
| [Simplification] | [What it avoids] |

---

## 4. Directory Structure

> Guidance: The top-level layout, with a one-line comment for each entry. Show only the depth agents need; leave internal structure to whoever owns each directory.

```
[project-name]/
├── frontend/                 # [frontend framework] project
├── backend/                  # [middleware] project ([programming language])
│   └── [app]/
│       ├── [subsystem]/      # [what it contains]
│       └── db/               # Schema, seed data, connection helpers
├── docs/                     # Project-wide documentation for agents
│   ├── multi_agent_PLAN.md   # This document
│   └── ...                   # Additional agent reference docs
├── scripts/                  # Start/stop scripts
├── test/                     # [e2e test framework] E2E tests and test infrastructure
├── db/                       # Local-dev database location (gitignored data file)
├── [container build file]    # [e.g. multi-stage build]
├── [container orchestration file]
├── .env                      # Environment variables (gitignored, .env.example committed)
└── .gitignore
```

### Key Boundaries

> Guidance: For each top-level directory, say what it owns, what it must not know about, how it talks to the other parts, and who (which agent or role) decides its internal structure. Clear ownership lets several agents work in parallel without colliding.

- **`frontend/`** — [ownership; talks to the backend only through `/api/*`]
- **`backend/`** — [ownership: server logic, database, API routes, integrations]
- **`docs/`** — project-wide documentation, including this plan. All agents treat it as the shared contract.
- **`test/`** — E2E tests and supporting infrastructure. Unit tests live inside `frontend/` and `backend/`, following each framework's conventions.
- **`scripts/`** — [what the scripts wrap]

---

## 5. Environment Variables

> Guidance: Every variable the app reads, with a comment saying whether it's required or optional, what it does and its default. Never put real secrets here.

```bash
# Required for [feature]: [description]
[AI_API_KEY]=your-key-here

# Optional: [description]. If not set, [fallback behavior]
[OPTIONAL_SERVICE_KEY]=

# Optional: set to "true" for deterministic mock [AI provider] responses (testing)
[AI_MOCK]=false

# Optional: database location. Defaults to [local default]
DB_PATH=
```

### Behavior

> Guidance: How each variable changes behavior, including what happens when a required one is missing. Prefer starting normally and failing only the affected feature with a clear error, over refusing to start.

- If `[OPTIONAL_SERVICE_KEY]` is set → [behavior]; otherwise → [fallback]
- If `[AI_MOCK]=true` → [behavior] (see §9)
- If `[AI_API_KEY]` is missing → [e.g. the app starts normally; the AI endpoint returns `503` with a message explaining how to configure it]

### Loading

- **[container]**: [how variables are injected, e.g. from `.env` via the orchestration file]
- **Local dev**: [how and from where `.env` is loaded, and which source takes precedence]

---

## 6. [Core Subsystem, e.g. Data Ingestion, Pricing, Notifications]

> Guidance: Use this section for the main domain-specific engine of the project: the thing that produces, processes or delivers the app's core data. Add more sections like this if there are several. Cover:
>
> - **Status:** whether it's already built (link to its summary doc) and which items below are still **[change]**.
> - **Interface and implementations:** if there are interchangeable implementations (e.g. a simulator and a real external service), the shared interface they implement and how one is selected.
> - **Behavior of each implementation:** update rates, rules, limits, edge cases (e.g. what happens to a newly added item before it has data).
> - **Shared state:** caches or buffers, what they hold, how long, and who reads and writes them.
> - **Definitions:** any derived values the UI shows (e.g. how "change %" is calculated) so the backend and frontend agree.
> - **Real-time delivery:** endpoint, transport, reconnect behavior, event frequency, and the exact payload with a field table.

> **Status:** [Not started | Built and tested — see `docs/[SUMMARY].md`]. Changes required by this plan are marked **[change]**.

### [Interface Name]: One Interface, [N] Implementations

[The interface, its methods, and how the implementation is selected (e.g. by environment variable). Downstream code must not depend on which implementation is active.]

### [Implementation A, e.g. Simulator (Default)]

- [Behavior, rates, rules]

### [Implementation B, e.g. External API (Optional)]

- [Behavior, rates, rules, limits]

### [Shared State, e.g. Cache]

- [What it holds, per item]
- [History or buffer size, and the endpoint that serves it]

### [Real-time Channel, e.g. Streaming]

- Endpoint: `GET /api/stream/[resource]`
- [Connection and reconnect behavior]
- [Event frequency and batching]

```
data: {"[key]": {"[field]": [value], ...}, ...}
```

| Field | Type | Notes |
|---|---|---|
| `[field]` | [type] | [units, rounding, format] |

---

## 7. Database

### [DB type], Initialized at Startup

> Guidance: Where the database lives, how the schema is created (e.g. idempotent `CREATE ... IF NOT EXISTS` at startup, or versioned migrations), how seed data is inserted, and whether an ORM is used. Explain why the approach means fresh environments work without manual steps.

### Conventions

- [Timestamp format, e.g. ISO 8601 UTC strings]
- [Normalization rules, e.g. identifiers trimmed and uppercased]
- [Retention, e.g. which tables are append-only or pruned]

### Schema

> Guidance: One block per table: its purpose, then each column with type, default and meaning, then constraints. Mention design choices that keep future options open (e.g. a `user_id` column on every table even while the app is single-user).

**[table_name]** — [purpose]
- `id` [type] PRIMARY KEY ([e.g. UUID])
- `user_id` [type] ([default])
- `[column]` [type] ([meaning, allowed values])
- `created_at` [type] ([format])
- [Constraints, e.g. UNIQUE on `(user_id, [column])`]

### Default Seed Data

- [Rows created on first run]

### Business Rules

> Guidance: The validation and calculation rules for the app's core operations, written once and shared by every entry point (UI, API, AI). Include input formats (with regexes where useful), limits and rounding, error messages, formulas, transaction boundaries, and what happens after a successful operation.

- **[Input]**: [validation rule]
- **[Operation]**: [preconditions, formula, error message]
- Each [operation] runs in one transaction, followed by [side effect].

---

## 8. API Endpoints

### Conventions

> Guidance: Body format, plus the status code and error body shape for each class of failure (business-rule failure, malformed request, not found, upstream failure, feature not configured).

- Request and response bodies are JSON.
- **Business-rule failures** return `400` with `{"error": "<human-readable message>"}`.
- **Malformed request bodies** return [status].
- **Not found** returns `404` with `{"error": "..."}`.

### [Resource Group]

> Guidance: Repeat for each group of endpoints. Give a table of routes, then for each route an example request and response with exact field names, plus notes on derived fields, ordering, idempotency, empty and null cases, and status codes. Returning updated state from mutations saves the frontend a re-fetch.

| Method | Path | Description |
|--------|------|-------------|
| GET | `/api/[resource]` | [description] |
| POST | `/api/[resource]` | [description]: `{[fields]}` |
| DELETE | `/api/[resource]/{id}` | [description] |

`GET /api/[resource]` → `200`
```json
{"[field]": "[value]"}
```
- [Notes on fields, ordering, null cases]

### System

| Method | Path | Description |
|--------|------|-------------|
| GET | `/api/health` | Health check → `200` `{"status": "ok"}` |

---

## 9. AI Integration

> Guidance: Delete this section if the project has no AI features.

[AI provider], model `[model]`, called through [client library or SDK]. Use structured outputs to interpret results. The API key is `[AI_API_KEY]` in the root `.env` file.

### How It Works

> Guidance: The full request lifecycle as numbered steps: what's stored, what context is loaded (and how much history), how the prompt is built, how the response is parsed, how actions are executed and failures reported, what's stored, and what's returned. Say whether responses are streamed or returned whole, and why.

1. Store the user message
2. Load context: [the current state the model needs]
3. Load the last [N] messages as conversation history
4. Build the prompt: system message, context, history, new message
5. Call the model, requesting structured output
6. Parse and validate the response
7. Execute requested actions (see Action Execution)
8. Add a note for any failed actions (see Reporting Failures)
9. Store the assistant message and action results
10. Return the stored assistant message (§8)

### Structured Output Schema

```json
{
  "message": "Conversational response to the user",
  "[action_type]": [
    {"[field]": "[value]"}
  ]
}
```

- `message` (required): text shown to the user
- `[action_type]` (optional): [what it does; which rules from §7 it goes through]

### Action Execution

> Guidance: Whether actions run automatically or need user confirmation, and why. The order in which action types run, whether one failure stops the rest, and how each outcome is recorded. AI-requested actions must go through the same validation and service layer as manual ones.

### Reporting Failures

> Guidance: How the user learns that an action failed, given the model wrote its message before anything ran (e.g. the backend appends a templated note per failed action, with no second model call).

### System Prompt Guidance

The model is prompted as "[assistant name], [role]" with instructions to:
- [Behavior]
- [Behavior]
- Always respond with valid structured JSON

### Mock Mode

> Guidance: When `[AI_MOCK]=true`, return deterministic responses instead of calling [AI provider], for fast, free, reproducible tests and development without an API key. Define the matching rules exactly, and make mock responses flow through the same execution and storage path as real ones.

| Message contains | Mock response |
|---|---|
| `[keyword]` | [message and actions] |
| anything else | [default message, no actions] |

---

## 10. Frontend Design

> Guidance: Delete this section for projects without a UI.

### Layout

> Guidance: The elements the UI must include, one bullet each, with what each shows, where its data comes from (endpoint or real-time stream) and how it behaves. Leave component architecture to the frontend owner unless it matters.

- **[Panel or view]** — [contents, data source, behavior]
- **[Panel or view]** — [contents, data source, behavior]
- **Header** — [contents]

### Data Flow

> Guidance: Which values are computed client-side, when to re-fetch, and what not to do (e.g. no polling when a stream already provides the data). Cover what happens after each kind of mutation.

- [Rule]
- **After [action]**, [what to use or re-fetch].

### Technical Notes

- [Real-time client, e.g. `EventSource` for `/api/stream/[resource]`]
- [Charting or visualization libraries allowed, and which are not]
- [Styling approach]
- All API calls go to the same origin (`/api/*`) — no CORS configuration needed

---

## 11. [container] & Deployment

### Build

> Guidance: The build stages, in order: base images, what each stage installs and copies, environment defaults, the exposed port and the start command.

```
Stage 1: [frontend build image]
  - [Install dependencies and build the frontend]

Stage 2: [runtime image]
  - [Install [dependency manager] and backend dependencies from the lockfile]
  - [Copy the frontend build output]
  - [Set environment defaults]
  - Expose port [port]
  - CMD: [start command]
```

### [Orchestration File]

> Guidance: The single source of truth for runtime config: image build, port mapping, env file and persistent volumes. Scripts and docs should defer to it rather than duplicate its settings.

```bash
[command to build and start]
```

### Start/Stop Scripts

- **`scripts/[start script]`** — [what it runs, flags, prints the URL, optionally opens the browser]
- **`scripts/[stop script]`** — [what it runs; whether data persists]
- [Equivalents for other platforms]

All scripts are idempotent — safe to run more than once.

### Cloud Deployment (Optional)

> Guidance: Target platforms, infrastructure-as-code location, and whether deployment is in scope or a stretch goal.

---

## 12. Testing Strategy

### Unit Tests (within `frontend/` and `backend/`)

> Guidance: For each part of the system, the behaviors and edge cases that must be covered. Tie them back to the rules in §6–§9 so nothing specified goes untested. Note any existing suites that are already complete.

**Backend ([test framework])**:
- [Subsystem]: [behaviors and edge cases]
- [Business rules]: [calculations, validation failures, boundary cases]
- [AI]: [parsing valid and malformed output, partial failures, mock-mode rules]
- API routes: status codes and response shapes from §8, error handling

**Frontend ([test framework])**:
- [Component rendering with mock data]
- [Interactive states: loading, error, empty]

### E2E Tests (in `test/`)

**Infrastructure**: [How the app and [e2e test framework] run together, keeping browser dependencies out of the production image.]

**Environment**: [e.g. run with `[AI_MOCK]=true`; reset state before each test for isolation.]

**Key Scenarios**:
- Fresh start: [what must be visible]
- [User journey from §2]
- [User journey from §2]
- Reset: [what must be restored]

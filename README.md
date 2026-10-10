# AI Agent Templates

Reusable instruction files for AI coding agents (Claude Code, Codex, Cursor, and similar), plus notes on prompts, debugging, and where to find MCP servers, skills, and plugins.

## Contents

### Global instructions

| File | Purpose |
| --- | --- |
| `agent-templates/root_AGENT.md` | General rules for every project: work in small validated steps, use the latest APIs, don't overengineer, find the root cause before fixing anything, use `uv` for Python, no emojis. |

### Project templates

Each template has the same sections: **Business Requirements**, **Limitations**, and **Technical Decisions**. The rental property and e-commerce templates also have a **Coding Standards** section that points to an example repo whose coding style the agent should match: [next-property](https://github.com/jstoops/next-property), [prostore](https://github.com/jstoops/prostore), and [proshop](https://github.com/jstoops/proshop) respectively.

| File | Stack | Example app |
| --- | --- | --- |
| `agent-templates/template_AGENT.md` | Blank | Empty skeleton with coding standards and pointers to [`docs/PLAN.md`](docs/PLAN.md) and [`docs/multi_agent_PLAN.md`](docs/multi_agent_PLAN.md) |
| `agent-templates/web_app_NextJS_python_model_AGENT.md` | Next.js, FastAPI, SQLite, Docker, `uv`, OpenRouter | App with accounts and an AI chat sidebar |
| `agent-templates/web_app_NextJS+Auth_React_Tailwind_Mongo_AGENT.md` | Next.js App Router (JS), NextAuth (Google), Tailwind, MongoDB/Mongoose, Cloudinary, Mapbox, Jest | Rental property listings |
| `agent-templates/web_app_NextJS+Auth_React_ShadCN_PostgreSQL_TS_Zod_AGENT.md` | Next.js App Router (TS), NextAuth v5, ShadCN, Prisma/PostgreSQL (Neon), Zod, PayPal, Stripe, Uploadthing, Resend, Jest | E-commerce store |
| `agent-templates/web_app_MERN_Redux_Mongoose_JWT_BCrypt_Multer_AGENT.md` | MongoDB, Express, React (CRA), Redux Toolkit/RTK Query, JWT cookies, bcrypt, Multer, PayPal, React Bootstrap | E-commerce store |

### Plan template

| File | Purpose |
| --- | --- |
| [`docs/PLAN.md`](docs/PLAN.md) | Ten-part MVP delivery plan (frontend inventory, containerized backend, auth, database design, persistence, AI chat) with checklists, tests, and success criteria. Uses placeholders such as `[project name]`, `[DB type]`, `[programming language]`, `[middleware]`, `[container]`, and `[AI provider]` for project and technology choices. |
| [`docs/multi_agent_PLAN.md`](docs/multi_agent_PLAN.md) | Project specification for agents working in parallel: vision, user experience, architecture, directory structure and ownership, environment variables, core subsystem, database, API, AI integration, frontend, deployment, and testing, with guidance on what to write in each section. Uses the same placeholders. |

### Reference notes

| File | Purpose |
| --- | --- |
| `Useful_Prompts_and_Tips.md` | Prompts for fixing stubborn bugs, adding test coverage, dependency and security updates, and code reviews, plus a step-by-step debugging method. |
| `tools.md` | Where to find MCP servers, skills, and Claude Code plugins, with a short list of useful ones. |

## Usage

1. Copy `agent-templates/root_AGENT.md` into your agent's global instructions file (for Claude Code, `~/.claude/CLAUDE.md`).
2. Copy the closest project template into the root of a new project as `CLAUDE.md` or `AGENTS.md`.
3. Replace `[project name]`, then edit the requirements, limitations, and technical decisions to match your project.
4. If you use `agent-templates/template_AGENT.md`, copy [`docs/PLAN.md`](docs/PLAN.md) into the project's `docs/` folder and fill in its placeholders before asking the agent to start. If several agents will work on the project, instead use [`docs/multi_agent_PLAN.md`](docs/multi_agent_PLAN.md) and fill it in.

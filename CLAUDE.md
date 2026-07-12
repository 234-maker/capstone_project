# CLAUDE.md

Guidance for AI assistants working in this repository.

## Project Overview

This is a capstone project for the **AI-assisted development track**. It is a full-stack web application with a **React** frontend and a **Node.js** backend.

The repository is organized as a monorepo with separate `client/` and `server/` directories. Keep frontend and backend concerns separated: UI logic stays in the client, API and business logic stay in the server.

When making changes:

- Prefer small, focused diffs over large refactors.
- Match existing patterns in the codebase before introducing new abstractions.
- Do not commit secrets (`.env`, API keys, credentials). Use `.env.example` for documented placeholders.

## Tech Stack

### Frontend (`client/`)

- **React** — UI framework
- **Vite** — dev server and build tooling (if present)
- **JavaScript or TypeScript** — follow whichever the client directory already uses
- **CSS** — component-scoped or module-based styles as established in the project

### Backend (`server/`)

- **Node.js** — runtime
- **Express** (or the framework already in use) — HTTP API layer
- **JavaScript or TypeScript** — follow whichever the server directory already uses

### Shared / Tooling

- **npm** — package management (per-package `package.json` in `client/` and `server/`)
- **ESLint / Prettier** — linting and formatting (when configured)
- **Git** — version control

When adding dependencies, install them in the correct package (`client/` or `server/`), not at the repo root unless a root workspace is explicitly set up.

## Folder Structure Conventions

```
capstone_project/
├── client/                 # React frontend
│   ├── public/             # Static assets
│   ├── src/
│   │   ├── components/     # Reusable UI components
│   │   ├── pages/          # Route-level page components
│   │   ├── hooks/          # Custom React hooks
│   │   ├── services/       # API client functions (fetch/axios wrappers)
│   │   ├── utils/          # Frontend-only helpers
│   │   ├── App.jsx         # Root app component (or .tsx)
│   │   └── main.jsx        # Entry point
│   └── package.json
├── server/                 # Node.js backend
│   ├── src/
│   │   ├── routes/         # Express route definitions
│   │   ├── controllers/    # Request handlers
│   │   ├── models/         # Data models / schemas
│   │   ├── middleware/     # Auth, validation, error handling
│   │   ├── services/       # Business logic
│   │   ├── utils/          # Backend-only helpers
│   │   └── index.js        # Server entry point (or .ts)
│   ├── package.json
│   └── .env.example        # Documented environment variables (no secrets)
├── README.md
├── CLAUDE.md
└── .gitignore
```

### Naming and placement rules

| What | Where | Convention |
|------|-------|------------|
| UI components | `client/src/components/` | PascalCase filenames (`UserCard.jsx`) |
| Pages / routes | `client/src/pages/` | PascalCase, one component per file |
| API routes | `server/src/routes/` | kebab-case or camelCase, match existing files |
| Business logic | `server/src/services/` | Keep routes thin; logic lives in services |
| Shared types | Co-locate or `types/` per package | Only share via duplication or a future `shared/` package if needed |
| Tests | `__tests__/` or `*.test.js` next to source | Mirror the source file structure |
| Environment config | `.env` (gitignored), `.env.example` (committed) | Never commit real secrets |

## Coding Style Conventions

### General

- Use **2-space indentation** unless the project already uses 4 spaces — match existing files.
- Use **semicolons** and **single quotes** only if that is already the dominant style in the file or package.
- Prefer `const` over `let`; avoid `var`.
- Keep functions small and single-purpose.
- Add comments only for non-obvious business logic, not for self-explanatory code.

### React (client)

- Use **functional components** and hooks; do not introduce class components.
- Colocate component-specific styles with the component when that pattern exists.
- Fetch data in `services/` or custom hooks, not directly inside presentational components.
- Handle loading, error, and empty states for async UI.
- Use meaningful `key` props in lists; avoid array indices when items can reorder.

### Node.js (server)

- Use `async/await` for asynchronous code; handle errors with try/catch or centralized error middleware.
- Validate request input at the route or middleware layer.
- Return consistent JSON response shapes (`{ data }` on success, `{ error, message }` on failure).
- Do not expose stack traces or internal details in production error responses.

### API contract

- REST-style routes: `GET /api/resource`, `POST /api/resource`, etc.
- Use appropriate HTTP status codes (200, 201, 400, 401, 404, 500).
- Keep the frontend API base URL configurable via environment variables.

### Git and commits

Use **Conventional Commits** format:

```
<type>(<optional scope>): <short description>

[optional body]
```

**Types:** `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`

**Examples:**

```
feat(client): add user login form
fix(server): handle missing JWT in auth middleware
docs: update README setup instructions
chore(server): upgrade express to 4.21
```

- Scope is optional but encouraged (`client`, `server`, or a feature name).
- Use imperative mood in the subject line ("add feature", not "added feature").
- Do not create commits unless the user explicitly asks.

## Instructions for AI Assistants

### Before you change code

1. **Read surrounding code** in the file and its directory before editing.
2. **Search the codebase** for existing utilities, components, or patterns before creating new ones.
3. **Minimize scope** — change only what the task requires.

### When to ask the user first

Ask before proceeding if the task involves:

- **Major architectural changes** (new database, auth strategy, state management library, monorepo restructure)
- **Adding new top-level dependencies** that affect the whole stack
- **Deleting or renaming public API routes** that the frontend depends on
- **Changing folder structure** away from the conventions above
- **Creating git commits or pushing** to remote (unless explicitly requested)
- **Modifying CI/CD, deployment, or environment configuration**

For routine bug fixes, small features, and style-aligned refactors within existing patterns, proceed without asking.

### What to do

- Run or suggest relevant commands (`npm install`, `npm run dev`, `npm test`) after meaningful changes.
- Check for linter errors in files you edit.
- Update `.env.example` when adding new environment variables.
- Prefer editing existing files over creating new ones when the change fits naturally.

### What not to do

- Do not commit `.env` files or credentials.
- Do not run destructive git commands (`git push --force`, `git reset --hard`) unless explicitly requested.
- Do not over-engineer (extra abstractions, premature optimization, unnecessary error handling for impossible cases).
- Do not add tests, docs, or refactors beyond what the task needs unless asked.

### Pull requests

When asked to create a PR:

1. Inspect `git status`, `git diff`, and commit history on the branch.
2. Write a concise summary and test plan.
3. Return the PR URL when done.

# Full-Stack Starter

Thin Turborepo monorepo composing [hono-starter](https://github.com/Addison-Enterprises/hono-starter) (backend) and [tanstack-starter](https://github.com/Addison-Enterprises/tanstack-starter) (frontend) into a single full-stack development environment.

```
full-stack-starter/
├── apps/
│   ├── backend/   ← hono-starter (git submodule)
│   └── frontend/  ← tanstack-starter (git submodule)
├── docker-compose.yml   ← PostgreSQL + MinIO + backend + frontend
├── turbo.json           ← Turborepo pipeline config
├── package.json         ← Root workspace config
└── .github/workflows/   ← CI (lint → format → typecheck → test)
```

## Quick Start

```bash
# Clone with submodules
git clone --recurse-submodules https://github.com/Addison-Enterprises/full-stack-starter.git
cd full-stack-starter

# Install dependencies
pnpm install

# Start everything (backend + frontend + PostgreSQL + MinIO)
docker compose up -d

# Or run dev servers directly (needs PostgreSQL + MinIO running separately)
pnpm dev
```

- **Backend** → http://localhost:3000 (API + `/health` + `/reference`)
- **Frontend** → http://localhost:5173

## Commands

| Command | What it does |
|---|---|
| `pnpm dev` | Start both apps in dev mode (requires PostgreSQL + MinIO) |
| `pnpm build` | Build both apps for production |
| `pnpm lint` | Lint both apps |
| `pnpm format:check` | Check formatting in both apps |
| `pnpm typecheck` | Type-check both apps |
| `pnpm test` | Run tests for both apps |
| `pnpm ci` | Run all checks (lint → format → typecheck → test) |
| `docker compose up -d` | Start full stack with hot reload |

## How It Works

The child repos (`hono-starter`, `tanstack-starter`) are pulled in as **git submodules**. This means:

- **No code duplication** — the monorepo is pure orchestration (config, CI, docker-compose)
- **Always gets latest** — `git submodule update --remote` pulls the newest commits from each child
- **Version-pinned** — committed submodule refs lock you to a known-good state until you choose to update

### Updating child repos

```bash
git submodule update --remote apps/backend
git submodule update --remote apps/frontend
git add apps/backend apps/frontend
git commit -m "chore: bump hono-starter and tanstack-starter"
```

## Architecture

- **Backend**: Hono + Kysely + better-auth + PostgreSQL + MinIO
- **Frontend**: TanStack Start + TanStack Query + Tailwind CSS + Base UI
- **Monorepo**: Turborepo with pnpm workspaces
- **Shared infra**: Single PostgreSQL and MinIO instance shared by both apps

## Reference Docs

- [Backend docs (hono-starter)](https://github.com/Addison-Enterprises/hono-starter)
- [Frontend docs (tanstack-starter)](https://github.com/Addison-Enterprises/tanstack-starter)
- [Full PRD](https://github.com/Addison-Enterprises/fullstack-blueprint/blob/main/PRD.md)
# Geomind infrastructure: agent guide

Shared services for geomind.team: the proxy (Traefik), the tunnel (cloudflared) and the data stores
(Postgres, Redis, MinIO, InfluxDB, ChromaDB, pgAdmin). One folder per service, each with its own
`compose.yml` and `.env` copied from its `.env.example`. The applications deploy from their own
repos. [README.md](README.md) has the start order, the addresses and the networks.

## Read both: `.claude/` for agents, `docs/` for people

`.claude/` is written for agents: start with [.claude/README.md](.claude/README.md). Read it, this
repo's README, and the project-wide context in the project root (`../AGENTS.md`, the shared memory
`../.claude/memory/MEMORY.md`, `../docs/`, where `docs/project/ops/` holds release and deploy and
`docs/infrastructure/` the running services).

## Rules

- Production runs from the `main` checkout on the NTNU Dell: never change, restart or stop a
  service there, and never run `docker compose down -v`, without Sahand's OK.
- Never write a secret into a file; `.env` files are not committed.
- One clone, no worktrees. The mode (project root `docs/project/ops/development-modes.md`) names the
  branch: hotfix on `main`, the others on `dev`.

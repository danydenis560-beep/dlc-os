# Contributing to DLC OS

First off — **thank you**. DLC OS exists to give every business an open, AI-native commerce platform they actually own. That only happens because people like you show up.

This guide explains how to get involved, whether you write code, design, document, test, or run a business that lives on these problems.

---

## Table of Contents

- [Ways to contribute](#ways-to-contribute)
- [Code of Conduct](#code-of-conduct)
- [Project status](#project-status)
- [Good first issues](#good-first-issues)
- [Development setup](#development-setup-planned)
- [Branching & commits](#branching--commits)
- [Pull request process](#pull-request-process)
- [Coding standards](#coding-standards)
- [Architecture Decision Records](#architecture-decision-records)
- [Community](#community)

---

## Ways to contribute

You do **not** need to be a senior engineer to make a real difference.

| If you are a… | You can… |
|---|---|
| **Backend dev** | Build FastAPI services, the data layer, channel integrations |
| **Frontend dev** | Build the Next.js dashboard & storefront from the [UI/UX system](./docs/08-ui-ux-system.md) |
| **AI / ML engineer** | Shape the [AI architecture](./docs/10-ai-architecture.md): agents, memory, forecasting |
| **DevOps / SRE** | Docker, Kubernetes, CI/CD, observability |
| **Designer** | Wireframes, components, design tokens, brand |
| **Technical writer** | Improve these docs, tutorials, API reference |
| **Business operator** | Tell us what's missing — your real-world workflows are gold |
| **Anyone** | Triage issues, answer questions, review PRs, star & share |

---

## Code of Conduct

All participation is governed by our [Code of Conduct](./CODE_OF_CONDUCT.md). By participating, you agree to uphold it. Report unacceptable behavior to `conduct@dlc-os.dev`.

---

## Project status

DLC OS is in the **vision & architecture** stage. The repository defines *what* we are building and *why*; implementation is beginning in public.

This means early contributors have outsized influence: you are helping pour the foundation, not patching someone else's. Read the [Development Roadmap](./docs/11-development-roadmap.md) and [MVP Roadmap](./docs/12-mvp-roadmap.md) to see where we are.

---

## Good first issues

We label beginner-friendly work with `good first issue` and `help wanted`. Until the issue tracker fills up, great starting points include:

1. **Refine a module spec** in [`docs/modules/`](./docs/modules/) — add edge cases, data fields, or API endpoints.
2. **Draft an OpenAPI snippet** for an endpoint in the [API design](./docs/06-api-design.md).
3. **Propose a wireframe** for a screen in the [UI/UX system](./docs/08-ui-ux-system.md).
4. **Write an ADR** (Architecture Decision Record) for an open question.
5. **Improve onboarding docs** — if something confused you, fix it for the next person.

Comment on an issue to claim it before starting, so we don't duplicate effort.

---

## Development setup (planned)

> The implementation is being built in public. This is the developer experience we are designing toward.

```bash
# 1. Fork & clone
git clone https://github.com/<you>/dlc-os.git
cd dlc-os

# 2. Environment
cp .env.example .env

# 3. Start infra + services
docker compose up        # Postgres, Redis, API, workers, web

# 4. Backend (without Docker)
cd apps/api
python -m venv .venv && source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -e ".[dev]"
uvicorn dlcos.main:app --reload

# 5. Frontend
cd apps/web
pnpm install
pnpm dev
```

See the [Folder Structure](./docs/07-folder-structure.md) for how the monorepo is laid out.

---

## Branching & commits

- Branch from `main`: `feat/discord-cart`, `fix/checkout-tax`, `docs/api-auth`.
- We use **[Conventional Commits](https://www.conventionalcommits.org/)**:

```
feat(checkout): add multi-currency support
fix(payments): handle Stripe webhook retries idempotently
docs(architecture): clarify AI memory data flow
chore(ci): cache pnpm store
```

Types: `feat`, `fix`, `docs`, `refactor`, `test`, `chore`, `perf`, `build`, `ci`.

---

## Pull request process

1. **Open an issue first** for anything non-trivial, so we can align on approach.
2. Keep PRs **focused and small** — easier to review, faster to merge.
3. Fill out the **[PR template](./.github/PULL_REQUEST_TEMPLATE.md)** completely.
4. Ensure CI passes (lint, type-check, tests).
5. Link the issue (`Closes #123`).
6. A maintainer (see [CODEOWNERS](./.github/CODEOWNERS)) reviews. Address feedback by pushing new commits.
7. We **squash-merge** with a Conventional Commit title.

Because DLC OS is **dual-licensed** — open source under the [AGPL-3.0](./LICENSE) **and** a commercial license that funds the project — contributors sign a lightweight **Contributor License Agreement (CLA)** on their first PR, granting the project the right to distribute their contribution under both licenses. Your work always remains available to everyone under the AGPL. We also use the **Developer Certificate of Origin** (sign commits with `git commit -s`).

---

## Coding standards

| Area | Standard |
|---|---|
| **Python** | `ruff` (lint+format), `mypy` (types), `pytest` (tests), type hints required |
| **TypeScript** | `eslint`, `prettier`, strict mode, no `any` without justification |
| **Commits** | Conventional Commits, signed (`-s`) |
| **Tests** | New logic ships with tests; aim to not lower coverage |
| **Docs** | Public APIs and modules are documented as they're built |
| **Security** | Never commit secrets; validate all input; follow [SECURITY.md](./SECURITY.md) |

---

## Architecture Decision Records

Significant technical decisions are recorded as ADRs in `docs/adr/` (e.g. *"ADR-0007: Use pgvector for AI memory"*). Propose one via PR when a decision has long-term consequences. This keeps our reasoning transparent and reviewable.

---

## Community

- 💬 **Discord** — the fastest way to get help and meet maintainers *(link in README)*
- 🗣️ **GitHub Discussions** — proposals, Q&A, show & tell
- 🐛 **Issues** — bugs and concrete feature requests

We try to respond to new issues and PRs within a few days. Be patient and kind — this is a community effort.

**Welcome aboard. Let's build the commerce OS the world actually owns.** 🚀

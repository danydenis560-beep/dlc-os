# 07 · Folder Structure

> The monorepo layout. Designed so every module has an obvious home, channels are
> swappable adapters, and the modular monolith can later split into services
> without a reorganization.

## Top level

```
dlc-os/
├── apps/                  # Deployable applications
│   ├── api/               # FastAPI backend (the core)
│   ├── web/               # Next.js dashboard + storefront
│   ├── workers/           # Celery workers & scheduled jobs
│   └── bots/              # Channel bot processes (Discord/Telegram/WhatsApp)
├── packages/              # Shared libraries (language-internal)
│   ├── sdk-ts/            # Generated TypeScript SDK
│   ├── sdk-py/            # Generated Python SDK
│   ├── ui/                # Shared React component library (design system)
│   └── config/            # Shared lint/tsconfig/tailwind presets
├── plugins/               # First-party & example plugins
├── infra/                 # Docker, Kubernetes, Terraform, CI helpers
├── docs/                  # This documentation (the blueprint)
├── scripts/               # Dev/ops scripts (seed, migrate, codegen)
├── .github/               # Issue/PR templates, workflows, CODEOWNERS
├── docker-compose.yml     # One-command local stack
├── pyproject.toml         # Python workspace config
├── package.json           # JS workspace (pnpm) config
└── README.md
```

## Backend — `apps/api/`

Organized by **bounded context** (see [Architecture](./04-architecture.md)), not by
technical layer. Each module is self-contained: router + service + models + schemas
+ events + tests. This is what makes later extraction into services mechanical.

```
apps/api/
├── src/dlcos/
│   ├── main.py                 # FastAPI app factory, router mounting
│   ├── core/                   # Cross-cutting infrastructure
│   │   ├── config.py           # Settings (env)
│   │   ├── db.py               # Async SQLAlchemy session
│   │   ├── security.py         # JWT, hashing, RBAC dependencies
│   │   ├── events.py           # Domain event bus
│   │   ├── pagination.py
│   │   ├── ratelimit.py
│   │   └── errors.py
│   ├── modules/
│   │   ├── identity/           # users, orgs, roles, auth, api keys
│   │   │   ├── router.py
│   │   │   ├── service.py
│   │   │   ├── models.py       # SQLAlchemy models
│   │   │   ├── schemas.py      # Pydantic request/response
│   │   │   ├── events.py
│   │   │   └── tests/
│   │   ├── catalog/
│   │   ├── inventory/
│   │   ├── cart/
│   │   ├── orders/
│   │   ├── payments/
│   │   │   └── providers/      # stripe.py, paypal.py, square.py, crypto.py
│   │   ├── crm/
│   │   ├── marketplace/
│   │   ├── shipping/
│   │   │   └── carriers/
│   │   ├── marketing/
│   │   ├── analytics/
│   │   ├── channels/           # canonical commands/events + normalizer
│   │   │   └── adapters/       # discord.py, telegram.py, whatsapp.py, web.py
│   │   └── ai/                 # agents, memory, tools, providers
│   │       ├── orchestrator.py
│   │       ├── memory.py
│   │       ├── tools.py        # maps core services → agent tools
│   │       ├── guardrails.py
│   │       └── providers/      # anthropic.py, openai.py, local.py
│   ├── migrations/             # Alembic
│   └── seeds/                  # demo data
├── tests/                      # integration / e2e
├── pyproject.toml
└── Dockerfile
```

**Anatomy of a module** (e.g. `orders/`): `router.py` (HTTP) → `service.py`
(business logic, the only place that writes) → `models.py` (tables) /
`schemas.py` (DTOs) → `events.py` (emitted domain events) → `tests/`. Modules call
each other **through services and events**, never by reaching into another
module's tables.

## Frontend — `apps/web/`

```
apps/web/
├── src/
│   ├── app/                    # Next.js App Router
│   │   ├── (dashboard)/        # Operator dashboard (auth-gated)
│   │   │   ├── overview/
│   │   │   ├── products/
│   │   │   ├── orders/
│   │   │   ├── customers/
│   │   │   ├── marketing/
│   │   │   ├── marketplace/
│   │   │   ├── analytics/
│   │   │   └── assistant/      # AI chat/voice surface
│   │   ├── (storefront)/       # Public web store
│   │   └── api/                # BFF route handlers
│   ├── components/             # App-specific components
│   ├── features/               # Feature modules (hooks + UI per domain)
│   ├── lib/                    # api client, auth, utils
│   └── styles/
├── public/
├── tailwind.config.ts
└── package.json
```

## Workers — `apps/workers/`

```
apps/workers/
├── src/
│   ├── celery_app.py
│   ├── tasks/
│   │   ├── emails.py
│   │   ├── webhooks.py
│   │   ├── broadcasts.py       # WhatsApp/SMS/email campaigns
│   │   ├── forecasting.py      # AI inventory forecasts
│   │   ├── payouts.py
│   │   └── exports.py
│   └── beat_schedule.py        # periodic jobs
└── Dockerfile
```

## Bots — `apps/bots/`

Thin processes that connect to platform gateways and forward to the core's channel
adapters. They hold connection logic only; commerce logic stays in `apps/api`.

```
apps/bots/
├── discord/    # gateway connection, slash commands, components
├── telegram/   # long-poll/webhook, inline keyboards
└── whatsapp/   # webhook receiver, interactive messages
```

## Plugins — `plugins/`

The extension mechanism (see [Phase 3](./14-phase-3-roadmap.md)). Each plugin
declares hooks, routes, UI slots, and AI tools it provides.

```
plugins/
├── example-loyalty-plus/
│   ├── manifest.json    # id, hooks, permissions, UI slots, tools
│   ├── backend/         # extra endpoints/handlers
│   └── frontend/        # dashboard widgets
```

## Infra — `infra/`

```
infra/
├── docker/              # service Dockerfiles, compose overrides
├── k8s/                 # Helm charts / manifests
├── terraform/           # cloud provisioning (optional)
└── observability/       # Prometheus, Grafana, OTEL configs
```

## Why this layout

- **Module = bounded context** → clear ownership, easy onboarding, mechanical service extraction.
- **Channels are adapters** → adding Instagram/iMessage later is one folder.
- **Apps vs packages** → deployables vs shared libs cleanly separated.
- **Plugins first-class** → ecosystem is designed in, not bolted on.

Next: [UI/UX System](./08-ui-ux-system.md)

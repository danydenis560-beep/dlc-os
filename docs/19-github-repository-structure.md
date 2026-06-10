# 19 · GitHub Repository & Org Structure

> How the project is organized on GitHub to be welcoming, navigable, and scalable —
> for users, contributors, and maintainers.

## Organization

```
github.com/dlc-os/
├── dlc-os/            # ⭐ the monorepo (core, web, workers, bots, docs)
├── docs-site/         # public documentation website (later)
├── plugins/           # official plugin examples & registry (later)
├── .github/           # org-level community health files & profile README
└── awesome-dlc-os/    # community-curated list (later)
```

Start as a **single monorepo** (`dlc-os/dlc-os`) — easiest to discover, star, and
contribute to. Split out repos only when scale demands.

## Repository anatomy (`dlc-os/dlc-os`)

```
dlc-os/
├── README.md                 # the hook: vision, features, quickstart, links
├── LICENSE                   # AGPL-3.0
├── CONTRIBUTING.md           # how to contribute
├── CODE_OF_CONDUCT.md        # Contributor Covenant
├── SECURITY.md               # responsible disclosure
├── CHANGELOG.md              # Keep a Changelog
├── ROADMAP.md                # high-level roadmap
├── .env.example              # config surface
├── docker-compose.yml        # one-command local stack
├── apps/ packages/ plugins/ infra/ scripts/   # code (see Folder Structure)
├── docs/                     # the full blueprint (this tree)
└── .github/                  # templates, workflows, CODEOWNERS, FUNDING
```

See [Folder Structure](./07-folder-structure.md) for the code layout.

## `.github/` contents

```
.github/
├── ISSUE_TEMPLATE/
│   ├── bug_report.yml
│   ├── feature_request.yml
│   └── config.yml            # links to Discord/Discussions; disables blank issues
├── PULL_REQUEST_TEMPLATE.md
├── CODEOWNERS                # auto-request reviews by area
├── FUNDING.yml               # GitHub Sponsors / Open Collective
├── dependabot.yml            # dependency updates (planned)
└── workflows/
    ├── ci.yml                # lint, type-check, test, build, scan
    ├── release.yml           # tagged releases, changelog, images (planned)
    └── codeql.yml            # security analysis (planned)
```

## Labels (triage system)

| Label | Meaning |
|---|---|
| `good first issue` | newcomer-friendly |
| `help wanted` | maintainers want help |
| `type: bug` / `type: feature` / `type: docs` | category |
| `area: api` / `area: web` / `area: ai` / `area: discord` … | maps to CODEOWNERS |
| `priority: p0…p3` | urgency |
| `status: needs-triage` / `blocked` / `in-progress` | workflow |
| `phase: mvp` / `phase: 2` / `phase: 3` | roadmap alignment |

## Branching & releases

- **Trunk-based:** short-lived branches → PR → squash-merge to `main`.
- `main` is always releasable; CI green required.
- **Releases:** SemVer tags; `release.yml` builds images, publishes changelog & SBOM.
- **Conventional Commits** drive automated changelog generation.

## Projects & milestones

- **GitHub Projects** board tracks the roadmap (Phase 0→3 columns / iterations).
- **Milestones** map to roadmap phases ([Dev Roadmap](./11-development-roadmap.md)).
- **Discussions** for proposals/RFCs, Q&A, show & tell.

## Profile README & discoverability

- Org profile README pitches DLC OS in 10 seconds with the demo GIF.
- Rich repo `About` + topics: `ecommerce`, `ai`, `commerce`, `discord`, `whatsapp`,
  `telegram`, `fastapi`, `nextjs`, `open-source`, `self-hosted`, `marketplace`.
- Badges, screenshots, and a 30-second demo in the README (the single highest-leverage asset).

## Automation (bots & actions)

- **CI** on every PR; required checks before merge.
- **Stale** bot (gentle), **welcome** bot for first-time contributors.
- **Dependabot** + **CodeQL** for supply-chain & security.
- **all-contributors** to recognize every kind of contribution.

## Governance

- Maintainers listed in [CODEOWNERS](../.github/CODEOWNERS); decisions via ADRs/RFCs.
- Transparent contributor → maintainer path (in [CONTRIBUTING](../CONTRIBUTING.md)).
- Trademark/brand policy protects the name while keeping the core AGPL-3.0.

Next: [Implementation Plan](./20-implementation-plan.md)

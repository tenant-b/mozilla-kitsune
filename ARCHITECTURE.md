# Kitsune Architecture

Kitsune is the platform that powers [SuMo (support.mozilla.org)](https://support.mozilla.org) —
Mozilla's support site, knowledge base, and community forums. It is a
[Django](https://www.djangoproject.com/) application.

This document is a high-level map of how the pieces fit together. For deeper
detail see the [online documentation](https://mozilla.github.io/kitsune/) and the
[Architecture Decision Records](docs/architectural-decisions.md).

## Overview

```
        Browser
           │
           ▼
   ┌───────────────┐      ┌──────────────┐
   │  Django (web) │◄────►│  PostgreSQL  │  primary data store
   │  gunicorn     │      └──────────────┘
   │  + Jinja/     │      ┌──────────────┐
   │    Svelte UI  │◄────►│    Redis     │  cache / broker
   └───────┬───────┘      └──────────────┘
           │              ┌──────────────┐
           ├─────────────►│ Elasticsearch│  search index
           │              └──────────────┘
           ▼
   ┌───────────────┐      ┌──────────────┐
   │ Celery workers│◄────►│    Redis     │  async tasks
   │ + celery beat │      └──────────────┘  (scheduled jobs)
   └───────────────┘
```

## Tech stack

| Layer      | Technology                                                        |
| ---------- | ----------------------------------------------------------------- |
| Language   | Python (3.14), JavaScript/TypeScript                              |
| Web        | Django 5.2, gunicorn, WhiteNoise                                  |
| Templating | django-jinja (Jinja2) + Svelte components                        |
| Frontend   | Webpack, PostCSS, Svelte, ESLint                                 |
| Database   | PostgreSQL (via `dj-database-url`)                               |
| Cache      | Redis / memcached (`django-redis`, `django-cache-url`)          |
| Search     | Elasticsearch (`elasticsearch` client)                          |
| Async      | Celery + Celery Beat (Redis broker), Flower for monitoring      |
| Auth       | `mozilla-django-oidc` (OpenID Connect)                          |
| Monitoring | Sentry (`sentry-sdk`)                                            |
| Integrations | Zendesk (`zenpy`)                                             |

## Backend layout (`kitsune/`)

Kitsune is organized as a set of Django apps under `kitsune/`. The main
functional areas:

- **`wiki/`** — the knowledge base: articles, revisions, localization.
- **`questions/`** — "Ask a Question" (AAQ) support Q&A.
- **`forums/`, `kbforums/`** — discussion forums.
- **`users/`, `groups/`, `access/`** — accounts, profiles, permissions.
- **`products/`** — the products/topics taxonomy support content hangs off.
- **`search/`** — Elasticsearch indexing and query layer.
- **`llm/`, `graphql/`** — LLM-assisted features and the GraphQL API (`schema.py`).
- **`dashboards/`, `kpi/`, `community/`, `karma/`, `kbadge/`** — metrics, contributor
  tooling, gamification.
- **`customercare/`, `messages/`, `notifications/`, `tidings/`** — messaging and
  notification subsystems.
- **`sumo/`** — shared plumbing: base settings helpers, decorators, API utilities,
  context processors, email utilities.

Cross-cutting configuration lives in `kitsune/settings.py`, `kitsune/urls.py`,
`kitsune/celery.py`, and `kitsune/celery_beat.py`.

## Frontend

Assets are built with Webpack (`webpack.*.js`) and PostCSS, with newer interactive
UI written as Svelte components (`svelte/`, and per-app Svelte under the Django
apps). Static assets are served in production via WhiteNoise. See
[docs/frontend.md](docs/frontend.md) and [docs/svelte.md](docs/svelte.md).

## Data & background processing

- **PostgreSQL** is the system of record.
- **Redis** serves as cache backend and as the Celery message broker.
- **Elasticsearch** backs on-site search; indexing happens both synchronously and
  via Celery tasks. See [docs/elastic_search.md](docs/elastic_search.md).
- **Celery** runs asynchronous and scheduled work (email, indexing, syncs);
  **Celery Beat** schedules periodic jobs and **Flower** provides monitoring.
  See [docs/celery.md](docs/celery.md).

## Local development

Local development runs through Docker Compose (`docker-compose.yml`), which brings
up the `web`, `node`, `celery`, `beat`, `flower`, `postgres`, `elasticsearch`,
`kibana`, `redis`, and `mailcatcher` services. See
[docs/development.md](docs/development.md) and the
[hacking howto](https://mozilla.github.io/kitsune/hacking_howto/).

## Deployment

Releases follow [semantic versioning](https://semver.org/) via signed git tags; a
tagged release is then rolled out through the deploy pipeline. Kitsune runs on
Kubernetes in production. See [docs/deployments.md](docs/deployments.md) and
[docs/k8s.md](docs/k8s.md).

## Further reading

- [Full documentation](https://mozilla.github.io/kitsune/)
- [Architecture Decision Records](docs/architectural-decisions.md)
- [Localization system](docs/l10n-system.md)
- [API documentation](docs/api.md)
- [Zendesk integration](docs/zendesk.md)

# Haske Documentation

Welcome to the official documentation for the **Haske web framework**. Haske pairs the ergonomics of familiar Python frameworks with a Rust-accelerated core to deliver high-throughput services without sacrificing developer velocity. This GitBook collects everything you need to build, test, and ship Haske applications—from quickstart guides to deep dives on the runtime and tooling.

## Why Haske?

- **Productive APIs** inspired by Flask and FastAPI for routing, requests, and responses.
- **Performance-first architecture** powered by optional Rust extensions that accelerate routing, JSON parsing, template rendering, ORM operations, and more.【F:haske/__init__.py†L3-L49】
- **Batteries included**: templating, sessions, ORM helpers, CLI tooling, WebSockets, admin scaffolding, and frontend integration ship out of the box.【F:haske/__init__.py†L5-L48】【F:haske/app.py†L186-L207】【F:haske/cli.py†L1-L107】

## How the GitBook is organised

- **Getting Started** introduces installation, project creation, and the default layout.
- **Core Concepts** documents routing, middleware, requests, responses, and session/state helpers.
- **Frontend & Templates** explores server-side rendering, static assets, and single-page app integration.
- **Data Access** covers the async ORM and database workflows.
- **Real-time Features** explains WebSocket primitives and background channels.
- **Tooling & Operations** highlights the CLI, testing strategies, deployment, and configuration reference.

Use the navigation sidebar or the table of contents below to jump to specific topics. Each chapter contains runnable snippets and practical tips sourced from the Haske codebase and example projects.

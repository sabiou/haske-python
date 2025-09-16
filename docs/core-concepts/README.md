# Core Concepts

Haske builds upon Starlette’s ASGI foundations while layering ergonomic helpers for routing, requests, responses, middleware, and session management. This section explains how each component interacts so you can reason about the lifecycle of a request from initial dispatch through to the response being sent back to the client.【F:haske/app.py†L186-L353】【F:haske/request.py†L1-L116】【F:haske/response.py†L1-L80】【F:haske/middleware.py†L1-L88】

Use the sub-pages to drill into specific APIs and idioms. You will find both high-level patterns and low-level hooks for extending Haske to match your application’s needs.

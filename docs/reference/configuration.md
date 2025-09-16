# Configuration Reference

This page summarises the core configuration knobs available on the `Haske` application class and supporting utilities. Use it as a checklist when wiring a new project or deploying to different environments.【F:haske/app.py†L157-L335】

## Application constructor

```python
from haske import Haske

app = Haske(
    name="myapp",
    template_dir="templates",
    static_dir="static"
)
```

- `name` – logical name used by Starlette when constructing the ASGI app.
- `template_dir` / `static_dir` – locations passed to the templating subsystem and used when automatically mounting static files.【F:haske/app.py†L162-L215】

Creating the app sets up template paths, initialises the optional Rust router, prepares frontend state, and attaches a WebSocket broadcaster. Haske also installs default CORS and gzip middleware and attempts to mount `/static` immediately so assets are served without extra steps.【F:haske/app.py†L186-L217】

## Middleware & mounts

- `app.middleware(cls, **options)` – append Starlette-compatible middleware to the stack. Handy for adding authentication, sessions, or compression beyond the defaults.【F:haske/app.py†L475-L548】
- `app.mount(path, app, name=None)` – mount other ASGI apps (e.g., admin dashboards or documentation sites).【F:haske/app.py†L480-L482】
- `app.static(path="/static", directory=None, name=None)` – mount a static directory and synchronise the template environment with the new path.【F:haske/app.py†L483-L499】

## Frontend integration

`app.setup_frontend(config, mode)` detects build directories, mounts static assets, and—when running in development—launches a frontend dev server and proxies requests to it. Key configuration keys include `frontend_dir`, `build_dir`, `dev_command`, `dev_port`, and `force_dev`. The helper also honours the `HASKE_SKIP_FRONTEND` environment variable to avoid launching duplicate dev servers.【F:haske/app.py†L358-L466】

Retrieve a public URL for a given path with `app.get_frontend_url(path)`, which returns a relative path in production and the proxied dev server URL in development.【F:haske/app.py†L466-L472】

## Environment variables

- `HASKE_DEBUG` – toggles Starlette’s debug mode and is set automatically when you pass `debug=True` to `app.run()`. You can also export it manually before building the app.【F:haske/app.py†L545-L602】
- `HASKE_SKIP_FRONTEND` – prevents `setup_frontend()` from launching a dev server if one is already running (set to `1` by the helper after it spawns the process).【F:haske/app.py†L402-L405】

## Runtime helpers

- `app.route()` / `app.websocket()` – register HTTP and WebSocket routes while keeping middleware-aware ordering through `_reorder_routes`.【F:haske/app.py†L318-L335】【F:haske/app.py†L228-L280】
- `app.run(host="0.0.0.0", choosen_port=8000, debug=False)` – wrap Uvicorn, ensure a free port, and apply reload/debug settings. Use this for local development; production deployments should invoke Uvicorn or Gunicorn directly.【F:haske/app.py†L598-L624】

Keep these options in mind as you grow your Haske project—they provide the hooks needed to customise behaviour without forking the framework.

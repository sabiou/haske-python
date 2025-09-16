# Command-line Interface

Haske bundles a Typer-powered CLI that streamlines common workflows such as scaffolding projects, running the development server, and preparing production builds. Install the package and run `haske --help` to see the available commands.【F:haske/cli.py†L1-L122】

## `haske dev`

Starts a Uvicorn-powered development server for the application referenced by `module:app`.

```bash
haske dev --module examples.blog_app.backend.app:app --host 0.0.0.0 --port 8000
```

Flags map directly to Uvicorn options: host, port, reload, and workers.【F:haske/cli.py†L19-L56】

## `haske new`

Interactive project generator that creates a folder, optional template/static directories, starter `app.py`, sample ORM models, and supporting files such as `requirements.txt` and `.env`. Use this to bootstrap prototypes quickly.【F:haske/cli.py†L39-L141】

```bash
haske new my-project
```

The command asks whether you want HTML templates, then writes example routes demonstrating both JSON and template responses.【F:haske/cli.py†L65-L118】

## `haske build`

Prepares your app for production by compiling Rust extensions when available and bundling frontend assets. The command reports whether the native core is accessible so you know if you are running in accelerated mode.【F:haske/cli.py†L143-L167】

Run `haske build` inside your project directory before containerising or deploying to production servers.

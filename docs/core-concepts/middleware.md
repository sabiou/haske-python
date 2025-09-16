# Middleware

Middleware wraps your application to inspect or modify traffic before it reaches route handlers. Haske exposes Starlette’s middleware interface while providing factories and accelerated implementations for common concerns such as sessions, CORS, compression, and rate limiting.【F:haske/app.py†L475-L499】【F:haske/middleware.py†L1-L334】

## Registering middleware

Call `app.middleware()` with a Starlette-compatible middleware class and keyword arguments. When the application builds, the middleware stack is passed to the underlying Starlette instance in the order you registered them.【F:haske/app.py†L475-L548】

```python
from haske.middleware import Middleware, SessionMiddlewareFactory

app.middleware(*SessionMiddlewareFactory(secret_key="super-secret"))
```

The helper factories return a `(cls, options)` tuple, so you can use argument unpacking to keep your configuration concise.【F:haske/middleware.py†L40-L135】

## Built-in helpers

- **SessionMiddlewareFactory** – Configures signed cookie sessions using Starlette’s `SessionMiddleware`. Supply a `secret_key` and optional cookie settings.【F:haske/middleware.py†L40-L69】
- **CORSMiddlewareFactory** – Applies Cross-Origin Resource Sharing headers with configurable origins, methods, headers, and credentials.【F:haske/middleware.py†L70-L106】
- **CompressionMiddlewareFactory** – Wraps Starlette’s gzip middleware and lets you tweak minimum size and compression level.【F:haske/middleware.py†L107-L135】
- **CompressionMiddleware** – Custom ASGI middleware that negotiates gzip/brotli compression using the Rust helpers for maximum throughput.【F:haske/middleware.py†L137-L243】
- **RateLimitMiddlewareFactory** – Adds IP-based request throttling with configurable limits per time window.【F:haske/middleware.py†L244-L334】

You can also pass any third-party Starlette middleware class directly to `app.middleware()`.

## Writing custom middleware

Implement the ASGI callable interface—accept `(scope, receive, send)` and forward to the downstream app when appropriate. The custom compression and rate limiting implementations in the source serve as references for intercepting response bodies and maintaining request state.【F:haske/middleware.py†L137-L304】

Remember to keep middleware order in mind: earlier entries see requests before later ones and receive responses on the way back up the chain.

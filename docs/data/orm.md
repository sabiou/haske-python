# ORM & Database Toolkit

`haske.orm.AsyncORM` wraps SQLAlchemy’s async engine with ergonomic, sync-friendly helpers and a Rust-accelerated core. You can import most of SQLAlchemy’s column types directly from the module, define models as usual, and rely on the helper methods for sessions, CRUD, and pagination.【F:haske/orm.py†L221-L399】【F:examples/orm-example/main.py†L1-L31】

## Initialise the engine

Create an `AsyncORM` instance and call `init_engine()` with your database URL. The method detects whether you are in a running event loop and either executes immediately (sync context) or returns a coroutine (async context). It also enables the Rust connection pool for supported backends like SQLite, PostgreSQL, and MySQL.【F:haske/orm.py†L257-L333】

```python
from haske.orm import AsyncORM

db = AsyncORM()
db.init_engine("sqlite+aiosqlite:///./app.db")
```

Call `create_all()` or `drop_all()` to manage schema metadata, again benefiting from the sync-friendly wrappers around async operations.【F:haske/orm.py†L334-L357】

## Define models & relationships

Import `Base`, `Column`, `Integer`, `String`, and relationship helpers from `haske.orm`. The module re-exports SQLAlchemy constructs and provides convenience wrappers for one-to-one, one-to-many, and many-to-many relationships.【F:haske/orm.py†L236-L255】【F:haske/orm.py†L200-L219】

```python
from haske.orm import Base, Column, Integer, String, OneToMany

class User(Base):
    __tablename__ = "users"
    id = Column(Integer, primary_key=True)
    username = Column(String, unique=True, nullable=False)
    posts = OneToMany("Post", back_populates="author")
```

## CRUD operations

`AsyncORM` exposes sync wrappers for the most common operations: `add`, `add_all`, `update`, `delete`, and `commit`. Each method opens an async session under the hood, commits, and refreshes instances where needed. Use them from synchronous code or await the async counterparts inside coroutines.【F:haske/orm.py†L368-L423】

Retrieval helpers cover:

- `all(model)` – fetch every row.
- `filter`/`filter_by` – apply custom criteria or keyword filters.
- `get(model, **kwargs)` – return the first matching row.
- `paginate(model, page, per_page, filters, order_by)` – return a `Pagination` object with counts and navigation helpers.【F:haske/orm.py†L448-L737】

## Raw SQL & prepared statements

Need lower-level access? Pass raw SQL strings to `fetch_all`, `fetch_one`, `execute`, or `execute_many`. The Rust extension precompiles queries, optimises type conversions, and caches prepared statements for repeated execution via `prepare()` and `execute_prepared()`. Clearing both Python and Rust caches is one call away with `clear_prepared_cache()`.【F:haske/orm.py†L424-L669】

## Batch operations & pagination

`batch_insert()` builds high-performance insert statements, while `paginate()` leverages both SQLAlchemy and Rust helpers to compute totals and apply ordering efficiently.【F:haske/orm.py†L573-L737】

## Health checks & diagnostics

Finally, `health_check()` pings the database with `SELECT 1`, `is_rust_pool_enabled()` reports whether the native pool is active, and `get_prepared_cache_size()` exposes cached statement counts—useful for monitoring and debugging.【F:haske/orm.py†L741-L775】

With these utilities you can prototype quickly without giving up advanced SQL features or async performance.

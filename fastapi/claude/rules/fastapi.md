# FastAPI Rules

## Project Structure

```
app/
├── main.py              # FastAPI app, lifespan, middleware
├── core/
│   ├── config.py        # Settings (pydantic-settings)
│   ├── dependencies.py  # Shared dependencies
│   └── exceptions.py    # Custom exception handlers
├── routers/             # Route handlers (one domain = one file)
├── services/            # Business logic
├── schemas/             # Pydantic request/response models
├── models/              # DB models (SQLAlchemy, etc.)
└── tests/
    ├── conftest.py
    └── test_*.py
```

## App Initialization & Lifespan

- Use the `lifespan` context manager for startup/shutdown logic. Never use deprecated `on_event("startup")` / `on_event("shutdown")`.
- Initialize shared resources (DB connections, SDK clients, etc.) in lifespan and store them in `app.state`.

```python
from contextlib import asynccontextmanager
from fastapi import FastAPI

@asynccontextmanager
async def lifespan(app: FastAPI):
    # startup
    app.state.db = await create_db_pool()
    yield
    # shutdown
    await app.state.db.close()

app = FastAPI(lifespan=lifespan)
```

## Package Management

- Use **uv** for package management. Never use `pip install` directly.
- Manage dependencies via `pyproject.toml`.
- Use `uv add`, `uv remove`, `uv run`, `uv lock`.

## Code Quality

- Use **ruff** for linting and formatting.
- Run `ruff check` and `ruff format` before committing.
- Add type hints to all function signatures.

## Pydantic v2

- Use `BaseModel`. Never use Pydantic v1 patterns (`class Config`, `validator`, `root_validator`).
- v2 patterns: `model_validate()`, `model_dump()`, `ConfigDict`, `field_validator`, `model_validator`.
- Separate request/response schemas in `schemas/`.
- Use `BaseSettings` from `pydantic-settings` for configuration.

```python
# Good
from pydantic import BaseModel, ConfigDict, field_validator

class UserResponse(BaseModel):
    model_config = ConfigDict(from_attributes=True)
    id: int
    name: str

# Bad - never use v1 patterns
class UserResponse(BaseModel):
    class Config:
        orm_mode = True
```

## Async First

- Use `async def` for route handlers and services by default.
- All I/O operations (DB, external APIs) must be async.
- Use `def` (sync) only for CPU-bound tasks.
- Use `asyncio.sleep()`, never `time.sleep()`.

## Dependency Injection

- Inject shared logic (auth, DB sessions, etc.) via `Depends()`.
- Keep dependency functions in `core/dependencies.py`.

```python
async def get_current_user(token: str = Depends(oauth2_scheme)) -> User:
    ...

@router.get("/me")
async def read_me(user: User = Depends(get_current_user)):
    ...
```

## Error Handling

- Raise `HTTPException` directly in routers.
- Create custom exception classes for business logic errors and handle them with `exception_handler`.
- Never expose raw 500 errors to the client.

```python
# core/exceptions.py
class NotFoundError(Exception):
    def __init__(self, resource: str, id: str):
        self.resource = resource
        self.id = id

# main.py
@app.exception_handler(NotFoundError)
async def not_found_handler(request, exc):
    return JSONResponse(status_code=404, content={"detail": f"{exc.resource} {exc.id} not found"})
```

## Security

- CORS: Manage allowed origins via environment variable (`ALLOWED_ORIGINS`). Never use `allow_origins=["*"]` in production.
- Auth: Use Bearer token or API key pattern.
- Store secrets (API keys, DB URLs, etc.) in `.env`. Never hardcode them.
- Always include `.env` in `.gitignore`.

```python
from pydantic_settings import BaseSettings

class Settings(BaseSettings):
    allowed_origins: list[str] = ["http://localhost:3000"]
    database_url: str
    secret_key: str

    model_config = ConfigDict(env_file=".env")

# main.py
from fastapi.middleware.cors import CORSMiddleware

settings = Settings()

app.add_middleware(
    CORSMiddleware,
    allow_origins=settings.allowed_origins,
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)
```

## Testing

- Use **pytest** + **httpx** `AsyncClient`.
- Place test files in `tests/` with `test_` prefix.
- Keep fixtures in `conftest.py`.

```python
import pytest
from httpx import AsyncClient, ASGITransport
from app.main import app

@pytest.fixture
async def client():
    transport = ASGITransport(app=app)
    async with AsyncClient(transport=transport, base_url="http://test") as ac:
        yield ac

@pytest.mark.anyio
async def test_health(client: AsyncClient):
    response = await client.get("/health")
    assert response.status_code == 200
```

## Naming Conventions

- Files: snake_case (`user_router.py`)
- Classes: PascalCase (`UserResponse`)
- Functions/variables: snake_case (`get_current_user`)
- Router prefixes: plural (`/users`, `/items`)
- Environment variables: UPPER_SNAKE_CASE (`DATABASE_URL`)

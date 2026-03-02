# AI Rules

## AI Client Initialization

- Initialize AI SDK clients in the FastAPI `lifespan` and store in `app.state`. Never create a new client per request.
- Close clients on shutdown.

```python
from contextlib import asynccontextmanager
from anthropic import AsyncAnthropic

@asynccontextmanager
async def lifespan(app: FastAPI):
    app.state.ai_client = AsyncAnthropic(timeout=60.0)
    yield
    await app.state.ai_client.close()
```

- Access via dependency injection:

```python
from fastapi import Request

def get_ai_client(request: Request) -> AsyncAnthropic:
    return request.app.state.ai_client
```

## Streaming (SSE)

- Return AI responses via **Server-Sent Events (SSE)** streaming by default.
- Use `StreamingResponse` with `text/event-stream` media type.
- Use the SDK's native streaming API (`client.messages.stream()`), not `stream=True` on `create()`.

```python
from fastapi.responses import StreamingResponse

@router.post("/generate")
async def generate(request: GenerateRequest, client: AsyncAnthropic = Depends(get_ai_client)):
    return StreamingResponse(
        stream_response(client, request),
        media_type="text/event-stream",
    )

async def stream_response(client: AsyncAnthropic, request: GenerateRequest):
    async with client.messages.stream(
        model="claude-sonnet-4-20250514",
        max_tokens=1024,
        messages=[{"role": "user", "content": request.prompt}],
    ) as stream:
        async for text in stream.text_stream:
            yield f"data: {json.dumps({'text': text})}\n\n"
    yield "data: [DONE]\n\n"
```

## Background Tasks

- Use `BackgroundTasks` for long-running AI tasks that don't need real-time responses (batch generation, summarization, etc.).
- Return a task ID immediately and provide a separate status endpoint.

```python
@router.post("/batch", status_code=202)
async def create_batch(request: BatchRequest, bg: BackgroundTasks):
    task_id = str(uuid4())
    bg.add_task(process_batch, task_id, request)
    return {"task_id": task_id}

@router.get("/batch/{task_id}")
async def get_batch_status(task_id: str):
    ...
```

## Error Handling & Retry

- Always wrap AI API calls in try/except.
- Retry on rate limit (429) with exponential backoff. Max 3 attempts.
- Set timeout on client initialization (see AI Client Initialization section).
- Never expose raw provider errors to the client. Return generalized error messages.

```python
from anthropic import RateLimitError, APITimeoutError

async def generate_with_retry(client: AsyncAnthropic, prompt: str, max_retries: int = 3):
    for attempt in range(max_retries):
        try:
            return await client.messages.create(
                model="claude-sonnet-4-20250514",
                max_tokens=1024,
                messages=[{"role": "user", "content": prompt}],
            )
        except RateLimitError:
            if attempt == max_retries - 1:
                raise
            await asyncio.sleep(2 ** attempt)
        except APITimeoutError:
            if attempt == max_retries - 1:
                raise
            await asyncio.sleep(1)
```

## Token Usage Logging

- Log token usage for every AI API call.
- Record input tokens, output tokens, model name, and elapsed time.
- Use structured logging for cost tracking.

```python
import logging
import time

logger = logging.getLogger("ai.usage")

async def generate(client: AsyncAnthropic, prompt: str) -> str:
    start = time.monotonic()
    response = await client.messages.create(...)
    elapsed = time.monotonic() - start

    logger.info(
        "ai_call",
        extra={
            "model": response.model,
            "input_tokens": response.usage.input_tokens,
            "output_tokens": response.usage.output_tokens,
            "elapsed_seconds": round(elapsed, 2),
        },
    )
    return response.content[0].text
```

## Prompt Management

- Never write prompts inline in code.
- Separate prompts into dedicated files or a constants module (`prompts/`).
- Clearly distinguish system prompts from user prompts.

## API Key Security

- Manage AI provider API keys via environment variables.
- Never expose API keys to the client (frontend).
- All AI calls must go through the backend only.

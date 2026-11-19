# syntax=docker/dockerfile:1
FROM ghcr.io/astral-sh/uv:0.12.5 AS uv

FROM python:3.12-slim AS base
COPY --from=uv /uv /usr/local/bin/uv
ENV UV_PYTHON_DOWNLOADS=never \
    UV_LINK_MODE=copy \
    UV_PROJECT_ENVIRONMENT=/opt/ozon-venv
WORKDIR /app
COPY .python-version pyproject.toml uv.lock README.md ./
COPY src/ ./src/

FROM base AS checks
RUN --mount=type=cache,target=/root/.cache/uv uv sync --locked --no-default-groups --group dev --no-editable
ENV PATH="/opt/ozon-venv/bin:$PATH"
CMD ["sh", "-ec", "ruff check --no-cache src && ruff format --check --no-cache src"]

FROM base AS runtime-install
RUN --mount=type=cache,target=/root/.cache/uv uv sync --locked --no-default-groups --no-editable

FROM python:3.12-slim AS runtime
COPY --from=runtime-install /opt/ozon-venv /opt/ozon-venv
ENV PATH="/opt/ozon-venv/bin:$PATH" \
    PYTHONUNBUFFERED=1 \
    PYTHONDONTWRITEBYTECODE=1
WORKDIR /tmp
USER 10001:10001
CMD ["python", "-c", "import ozon_demand_service; print('ozon_demand_service: import OK')"]

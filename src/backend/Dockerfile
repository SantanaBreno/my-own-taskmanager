FROM python:3.11-slim AS builder

ENV POETRY_VERSION=1.8.4
RUN apt-get update && apt-get install -y build-essential python3-pip && \
    pip install "poetry==$POETRY_VERSION" && \
    apt-get remove -y build-essential && \
    apt-get autoremove -y && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*


WORKDIR /app

COPY pyproject.toml poetry.lock* ./
RUN poetry config virtualenvs.create false && poetry install --no-interaction --no-ansi

WORKDIR /app

COPY --from=builder /usr/local /usr/local
COPY --from=builder /app /app
COPY . .

EXPOSE 8000

CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
# Stage 1: Build dependencies
FROM python:3.10 as builder

WORKDIR /app

# Install Poetry
RUN pip install poetry

# Copy project files
COPY pyproject.toml poetry.lock ./

# Install dependencies without virtualenvs and dev dependencies
RUN poetry config virtualenvs.create false \
    && poetry install --no-interaction --no-ansi --no-root --without dev

# Stage 2: Final runtime container
FROM python:3.10-slim

WORKDIR /app

# Install dependencies from the builder stage
COPY --from=builder /usr/local/lib/python3.10/site-packages/ /usr/local/lib/python3.10/site-packages/
COPY --from=builder /usr/local/bin/ /usr/local/bin/

# Copy application files
COPY . .

# Create a non-root user
RUN adduser --disabled-password --gecos "" appuser
RUN chown -R appuser:appuser /app

# Switch to non-root user
USER appuser

# Set environment variables
ENV PYTHONPATH=/app
ENV PORT=8000

# Expose the application port
EXPOSE 8000

# Wait for PostgreSQL to be ready before starting the app
CMD ["sh", "-c", "bash ./prestart.sh && uvicorn app.main:app --host 0.0.0.0 --port $PORT"]

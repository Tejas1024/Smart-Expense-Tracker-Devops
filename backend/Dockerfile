# -------------------------------
# Stage 1: Build dependencies
# -------------------------------
FROM python:3.11-slim AS builder

WORKDIR /app

# Install dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt


# -------------------------------
# Stage 2: Runtime
# -------------------------------
FROM python:3.11-slim

WORKDIR /app

# Copy installed Python packages
COPY --from=builder /usr/local/lib/python3.11/site-packages /usr/local/lib/python3.11/site-packages
COPY --from=builder /usr/local/bin /usr/local/bin

# Copy project files
COPY . .

# Create non-root user
RUN useradd -m appuser && chown -R appuser:appuser /app

USER appuser

# Expose Django port
EXPOSE 8000

# 🚀 Start Gunicorn properly (CRITICAL  FIX)
CMD ["gunicorn", "expense_tracker.wsgi:application", "--bind", "0.0.0.0:8000", "--workers", "2", "--timeout", "120"]

# Build from the repo root (the backend reads ./skills next to ./backend):
#   docker build -f backend/Dockerfile -t taste-test-dummy-backend .
FROM python:3.12-slim

ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    PIP_NO_CACHE_DIR=1

WORKDIR /app

COPY backend/requirements.txt backend/requirements.txt
RUN pip install -r backend/requirements.txt

COPY backend/app backend/app
COPY skills skills

# Studies are stored as JSON here; mount a volume to keep them across restarts.
RUN useradd --create-home appuser \
    && mkdir -p /app/backend/data \
    && chown -R appuser /app/backend/data
USER appuser

WORKDIR /app/backend
EXPOSE 8000

HEALTHCHECK --interval=30s --timeout=5s --start-period=10s \
    CMD python -c "import os,urllib.request; urllib.request.urlopen('http://127.0.0.1:%s/api/health' % os.environ.get('PORT','8000'))"

# CFST_ALLOWED_ORIGINS must include the frontend origin; PORT is honored for PaaS hosts.
CMD ["sh", "-c", "uvicorn app.main:app --host 0.0.0.0 --port ${PORT:-8000}"]

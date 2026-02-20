# syntax=docker/dockerfile:1

FROM python:3.12-slim AS builder

WORKDIR /build

COPY requirements.txt .
RUN pip install --no-cache-dir --upgrade pip setuptools wheel \
    && pip wheel --no-cache-dir --wheel-dir /build/wheels -r requirements.txt

FROM python:3.12-slim AS runtime

ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    PROMETHEUS_MULTIPROC_DIR=/tmp/prometheus_metrics \
    FLASK_APP=index.py

WORKDIR /app

COPY --from=builder /build/wheels /wheels
COPY requirements.txt .
RUN pip install --no-cache-dir --upgrade pip \
    && pip install --no-cache-dir --no-index --find-links=/wheels -r requirements.txt \
    && rm -rf /wheels

COPY . /app

RUN mkdir -p /tmp/prometheus_metrics \
    && chmod +x /app/entrypoint.sh

EXPOSE 5000

ENTRYPOINT ["./entrypoint.sh"]

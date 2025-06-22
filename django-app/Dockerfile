# Stage 1: Install dependencies and copy code
FROM python:3.11-slim AS builder

WORKDIR /app

COPY requirements.txt .

RUN pip install --upgrade pip && pip install --no-cache-dir -r requirements.txt

COPY . .

# Stage 2: Runtime only (copy dependencies + code)
FROM python:3.11-slim

WORKDIR /app

COPY --from=builder /app /app

RUN pip install --upgrade pip && pip install --no-cache-dir -r requirements.txt

EXPOSE 8000

CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]

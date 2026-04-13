FROM python:3.9-slim

# PostgreSQL зависимости
RUN apt-get update && apt-get install -y \
    gcc \
    libpq-dev \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get clean

WORKDIR /app

# Обновляем pip
RUN pip install --no-cache-dir --upgrade pip setuptools

# Копируем только requirements (кеширование слоёв!)
COPY requirements/ ./requirements/
RUN pip install --no-cache-dir -r requirements/base.txt \
    && pip install --no-cache-dir -r requirements/dev.txt

# Копируем код
COPY . .

# Миграции + статика + gunicorn
CMD ["sh", "-c", "python manage.py migrate && python manage.py collectstatic --noinput && gunicorn --bind 0.0.0.0:8000 kittygram_backend.wsgi"]
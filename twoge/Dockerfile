FROM python:3.9-slim AS build

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY . /app

EXPOSE 80

CMD ["flask", "run", "--host=0.0.0.0", "--port=80"]
# CMD ["gunicorn", "--bind", "0.0.0.0:80", "--config", "gunicorn_config.py", "app:app"]
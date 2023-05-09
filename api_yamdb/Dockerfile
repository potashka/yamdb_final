FROM python:3.7-slim

WORKDIR /app

COPY requirements.txt .

RUN pip3 install -r ./requirements.txt --no-cache-dir

COPY ./ .

COPY ./reviews/static/redoc.yaml /app/static

# COPY infra/fixtures.json ./static/

CMD ["gunicorn", "api_yamdb.wsgi:application", "--bind", "0:8000" ]
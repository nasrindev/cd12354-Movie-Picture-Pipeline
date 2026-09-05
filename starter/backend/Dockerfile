FROM public.ecr.aws/docker/library/python:3.10-alpine

ENV FLASK_RUN_HOST=0.0.0.0

WORKDIR /

RUN adduser -u 1000 -D app

RUN apk add --no-cache \
    build-base \
    gcc \
    musl-dev \
    python3-dev \
    linux-headers \
    pcre \
    pcre-dev \
    libffi-dev \
    openssl-dev && \
    pip install --no-cache-dir pipenv

RUN mkdir -p /var/www/app && mkdir /app

COPY Pipfile Pipfile.lock /app/

RUN cd /app && pipenv sync --system

COPY . /app

CMD ["uwsgi", "--http", ":5000", "--master", "--enable-threads", "-s", "/var/www/app/app.sock", "--manage-script-name", "--mount", "/=app:app"]
FROM public.ecr.aws/docker/library/python:3.10-alpine
ENV FLASK_RUN_HOST=0.0.0.0
ENV PYTHONPATH=/app

WORKDIR /app

RUN adduser -u 1000 -D app

RUN apk update && \
    apk add \
    pcre \
    pcre-dev \
    build-base \
    gcc \
    linux-headers \
    openssl \
    libffi-dev \
    && pip install pipenv

COPY Pipfile* ./
RUN pipenv install --skip-lock --verbose --system

COPY . .
RUN mkdir -p /var/www/app

USER app

CMD ["uwsgi", "--http", ":5000", "--master", "--enable-threads", "--pythonpath", "/app", "--module", "app:app"]
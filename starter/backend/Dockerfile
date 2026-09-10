FROM public.ecr.aws/docker/library/python:3.10-slim

ENV FLASK_RUN_HOST=0.0.0.0

WORKDIR /

# Create application user
RUN useradd -u 1000 -m app

# Install required system dependencies and Pipenv
RUN apt-get update && \
    apt-get install -y \
    gcc \
    build-essential \
    libpcre2-dev \
    libssl-dev \
    libffi-dev \
    && rm -rf /var/lib/apt/lists/* \
    && pip install pipenv

# Create required directories
RUN mkdir -p /var/www/app && \
    mkdir -p /app

# Copy dependency files
COPY Pipfile* /app/

# Install Python dependencies
RUN cd /app && pipenv install --system --deploy

# Copy application source code
COPY . /app

# Start uWSGI
CMD ["uwsgi", "--http", ":5000", "--master", "--enable-threads", "-s", "/var/www/app/app.sock", "--manage-script-name", "--mount", "/=app:app"]
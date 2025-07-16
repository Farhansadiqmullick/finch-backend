# Docker Containerization for Django Application

This document describes the Docker setup for containerizing the Django application with PostgreSQL support.

## Overview

The Docker configuration uses a multi-stage build process to:
1. Create a lean production image
2. Minimize attack surface
3. Reduce final image size
4. Follow security best practices

## Dockerfile Architecture

### Build Stage (`builder`)

**Base Image:** `ubuntu:22.04`

**Dependencies Installed:**
- Python 3 and pip
- Virtual environment support
- Build tools (python3-dev, build-essential)
- Database support (libpq-dev for PostgreSQL)
- Cryptographic libraries (libssl-dev, libffi-dev)
- Compression library (zlib1g-dev)

**Key Actions:**
1. Creates a virtual environment (`venv1`)
2. Upgrades pip
3. Installs all Python dependencies from `requirements.txt`

### Production Stage

**Base Image:** `ubuntu:22.04` (fresh instance)

**Runtime Dependencies:**
- Python 3
- Virtual environment support
- PostgreSQL client libraries (libpq5)

**Key Actions:**
1. Copies only the virtual environment from builder stage
2. Copies application code
3. Creates a non-root user (`appuser`) for security
4. Sets appropriate permissions
5. Switches to non-root user context

## Security Features

1. **Non-root user:** The container runs as `appuser` (UID 1000) instead of root
2. **Minimal dependencies:** Only essential runtime dependencies are included
3. **Clean layer:** Build dependencies are not included in final image
4. **Proper permissions:** Application directory owned by `appuser`

## Port Configuration

- Exposes port **8000** (default Django development server port)

## Startup Process

The container executes the following commands on startup:
1. Activates the Python virtual environment
2. Runs database migrations (`python manage.py migrate`)
3. Creates a superuser non-interactively (`--noinput` flag)
4. Starts Django development server (`runserver 0.0.0.0:8000`)

## Building the Image

To build the Docker image:

```bash
docker build -t your-image-name .
```
Running the Container

To run the container:

```bash

docker run -p 8000:8000 -e "ENV_VAR=value" your-image-name
```
Environment Variables

The application expects the following environment variables (should be passed when running the container):
plaintext

POSTGRES_HOST
POSTGRES_USER
POSTGRES_PASSWORD
POSTGRES_DB
STRIPE_SECRET_KEY
STRIPE_PUBLIC_KEY
STRIPE_WEBHOOK_SECRET
DJANGO_SUPERUSER_USERNAME
DJANGO_SUPERUSER_EMAIL
DJANGO_SUPERUSER_PASSWORD
SITE_URL
FORNTEND_SITE_URL
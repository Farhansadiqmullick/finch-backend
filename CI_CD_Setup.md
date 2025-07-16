# CI/CD Setup for Django with PostgreSQL

This document outlines the Continuous Integration and Continuous Deployment (CI/CD) setup for the Django application using GitHub Actions.

## Overview

The CI/CD pipeline performs the following actions:
1. Runs Django checks and unit tests on every push to `main` branch or pull request
2. Builds and pushes a Docker image to GitHub Container Registry when tests pass

## Prerequisites

- GitHub repository
- Docker installed for local testing (optional)
- PostgreSQL for testing environment
- Required secrets configured in GitHub repository

## GitHub Actions Workflow

### Trigger Events

The workflow runs on:
- Pushes to the `main` branch
- Tags starting with 'v*'
- Pull requests to the `main` branch

### Environment Variables

The following environment variables are used:


### Secrets Required

STRIPE_SECRET_KEY
STRIPE_PUBLIC_KEY
STRIPE_WEBHOOK_SECRET
DJANGO_SUPERUSER_USERNAME
DJANGO_SUPERUSER_EMAIL
DJANGO_SUPERUSER_PASSWORD
DOCKERHUB_USERNAME
DOCKERHUB_TOKEN

Jobs
1. Django Checks & Unit Tests

Runs on: Ubuntu latest

Services:

    PostgreSQL 15 container with:

        User: given_user

        Password: given_password

        Database: fleetdb

        Port: 5432

        Health checks configured

        Persistent volume for data

Steps:

    Checks out repository code

    Sets up Python 3.10

    Creates virtual environment and installs dependencies

    Waits for PostgreSQL to be ready

    Runs Django checks (python manage.py check)

2. Build and Push Docker Image

Runs on: Ubuntu latest
Depends on: Test job success

Steps:

    Checks out repository code

    Logs in to Docker Hub using credentials

    Sets up Docker Buildx

    Builds and pushes multi-architecture Docker image (linux/amd64, linux/arm64) to GitHub Container Registry

Docker Configuration

The workflow uses a Dockerfile in the repository root to build the image. The resulting image is tagged as:
ghcr.io/farhanmullick/finch-backend:latest
Deployment

While this workflow builds and pushes the Docker image, actual deployment to your production environment (presumably at http://93.127.195.189:5000) would need to be configured separately. This could be done through:

    A separate deployment workflow

    Manual pulling of the image on the production server

    Using a container orchestration system

Setup Instructions

    Configure all required secrets in GitHub repository settings

    Ensure your Dockerfile is properly configured for production

    Verify PostgreSQL connection settings match your production environment

    Push to main branch to trigger the workflow

Monitoring

Workflow runs can be monitored in the GitHub repository's "Actions" tab. Failed runs will be reported there with detailed logs.
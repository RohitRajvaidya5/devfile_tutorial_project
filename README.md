# Django Dev Environment Setup (Devfile & Codespaces)

This repository is a learning project to understand how modern
container-based development environments work.

The goal of this project is to:

-   Run a Django app inside a container
-   Understand Devfile-based environments
-   Understand GitHub Codespaces (.devcontainer)
-   Learn how cloud-based development differs from local virtualenv
    setup
-   Practice modern backend engineering workflows

------------------------------------------------------------------------

# What This Project Demonstrates

Instead of installing Python, virtualenv, and dependencies manually on a
local machine, we define the development environment as code.

This ensures: - Reproducibility - Faster onboarding - No "works on my
machine" issues - Alignment with modern enterprise backend workflows

------------------------------------------------------------------------

# Option 1: Using Devfile

This approach is typically used in Kubernetes-based cloud IDE platforms.

## Project Structure

    myproject/
    ├── manage.py
    ├── requirements.txt
    └── devfile.yaml

## devfile.yaml

``` yaml
schemaVersion: 2.2.0
metadata:
  name: django-dev-environment

components:
  - name: python
    container:
      image: python:3.11
      memoryLimit: 1Gi
      mountSources: true

commands:
  - id: install-deps
    exec:
      component: python
      commandLine: pip install -r requirements.txt
      workingDir: ${PROJECT_SOURCE}

  - id: run-server
    exec:
      component: python
      commandLine: python manage.py runserver 0.0.0.0:8000
      workingDir: ${PROJECT_SOURCE}
```

## What This Does

-   Pulls Python 3.11 container
-   Mounts project files inside container
-   Defines commands to:
    -   Install dependencies
    -   Run Django server

You must manually run: - install-deps - run-server

------------------------------------------------------------------------

# Option 2: Using GitHub Codespaces (.devcontainer)

This is the recommended and more practical approach for this project.

## Project Structure

    myproject/
    ├── manage.py
    ├── requirements.txt
    └── .devcontainer/
        └── devcontainer.json

## devcontainer.json

``` json
{
  "image": "python:3.11",
  "postCreateCommand": "pip install -r requirements.txt",
  "postStartCommand": "python manage.py runserver 0.0.0.0:8000",
  "forwardPorts": [8000]
}
```

## What Happens Automatically

When you create a Codespace:

1.  Python 3.11 container is pulled
2.  Project is mounted inside container
3.  Dependencies are installed automatically
4.  Django server starts automatically
5.  Port 8000 is forwarded
6.  A preview link is generated

------------------------------------------------------------------------

# Running Manually (If Needed)

If the server does not auto-start:

    python manage.py runserver 0.0.0.0:8000

Important: Always use `0.0.0.0` inside containers to allow port
forwarding.

------------------------------------------------------------------------

# What I Learned From This Project

-   Containers replace virtualenv in cloud environments
-   Dev environments can be defined as code
-   Ports must be explicitly exposed in container setups
-   0.0.0.0 is required for external access inside containers
-   Modern enterprises standardize development environments
-   Devfile is more Kubernetes-oriented
-   .devcontainer is GitHub-native
-   Cloud-based development is closer to real production workflows

------------------------------------------------------------------------

# Devfile vs .devcontainer

  Setup Type      Best For
  --------------- ----------------------------------------------
  Devfile         Kubernetes-based enterprise IDEs
  .devcontainer   GitHub Codespaces & modern cloud development

------------------------------------------------------------------------

# Why This Matters

Understanding this setup prepares you for:

-   Docker-based backend development
-   Kubernetes environments
-   Enterprise development workflows
-   Remote and cloud-based engineering teams
-   CI/CD and DevOps practices

This project is not about Django itself. It is about learning
environment standardization and container-based development.

------------------------------------------------------------------------

# Future Improvements

-   Add PostgreSQL container
-   Add Dockerfile
-   Add docker-compose setup
-   Connect to remote database
-   Add CI pipeline

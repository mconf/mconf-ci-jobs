# mconf-ci-jobs

Reusable GitHub Actions workflows for CI/CD pipelines across Mconf projects.

## Overview

This repository provides a centralized collection of reusable GitHub Actions workflows for building, testing, linting, and deploying applications across multiple programming languages and platforms. All workflows are optimized for self-hosted runners and follow security best practices.

## Available Workflows

### Go Workflows

* `lb-go-build.yml`
  Builds Go applications with support for private modules via SSH.
  **Usage:** See [`examples/lb-go-build.yml`](examples/lb-go-build.yml)
*  `lb-go-lint.yml`
  Runs golangci-lint on Go code.
  **Usage:** See [`examples/lb-go-lint.yml`](examples/lb-go-lint.yml)
* `lb-go-tests.yml`
  Runs Go tests with coverage reporting to pull requests. Requires `pull-requests: write` permission.
  **Usage:** See [`examples/lb-go-tests.yml`](examples/lb-go-tests.yml)

### Node.js Workflows

* `lb-node-build.yml`
  Builds Node.js applications.
  **Usage:** See [`examples/lb-node-build.yml`](examples/lb-node-build.yml)
* `lb-node-lint.yml`
  Runs ESLint on JavaScript/TypeScript code.
  **Usage:** See [`examples/lb-node-lint.yml`](examples/lb-node-lint.yml)
* `lb-node-tests.yml`
  Runs Node.js tests (only on modified JavaScript/TypeScript files).
  **Usage:** See [`examples/lb-node-tests.yml`](examples/lb-node-tests.yml)


### Python Workflows

* `lb-py-lint.yml`
  Runs Flake8, Black, and isort on Python code (pip-based projects).
  **Usage:** See [`examples/lb-py-lint.yml`](examples/lb-py-lint.yml)
* `lb-py-pdm-lint.yml`
  Runs Flake8, Black, and isort on Python code (PDM-based projects).
  **Usage:** See [`examples/lb-py-pdm-lint.yml`](examples/lb-py-pdm-lint.yml)
* `lb-py-tests.yml`
  Runs Python tests with coverage reporting (PDM-based projects). Requires `pull-requests: write` permission.
  **Usage:** See [`examples/lb-py-tests.yml`](examples/lb-py-tests.yml)

### Ruby Workflows

* `lb-ruby-lint.yml`
  Runs RuboCop linter on Ruby code.
  **Usage:** See [`examples/lb-ruby-lint.yml`](examples/lb-ruby-lint.yml)
* `portal-ruby-lint.yml`
  Runs RuboCop and HAML linting for Rails applications. Reads Ruby version from `.ruby-version` file.
  **Usage:** See [`examples/portal-ruby-lint.yml`](examples/portal-ruby-lint.yml)
* `portal-ruby-scan.yml`
  Scans Rails applications for security vulnerabilities using Brakeman. Reads Ruby version from `.ruby-version` file.
  **Usage:** See [`examples/portal-ruby-scan.yml`](examples/portal-ruby-scan.yml)
* `portal-ruby-tests.yml`
  Runs Rails tests with MySQL and Redis services. Reads Ruby version from `.ruby-version` file. Requires OAuth and Devise secrets.
  **Usage:** See [`examples/portal-ruby-tests.yml`](examples/portal-ruby-tests.yml)


### Security & Docker Workflows

* `all-build-push-image.yml`
  Builds and pushes Docker images to Harbor with automatic tagging and caching. Supports multi-platform builds and optional DockerHub login for base images.
  **Usage:** See [`examples/all-build-push-image.yml`](examples/all-build-push-image.yml)
* `lb-scan.yml`
  Scans repository filesystem for security vulnerabilities using Trivy. Requires `pull-requests: write` permission.
  **Usage:** See [`examples/lb-scan.yml`](examples/lb-scan.yml)
* `lb-push-scan-image.yml`
  Builds Docker image, pushes to registry (on tags), and scans with Trivy. Includes optional SSH support for private dependencies.
  **Usage:** See [`examples/lb-push-scan-image.yml`](examples/lb-push-scan-image.yml)

### Release & Changelog Workflows

* `all-gen-changelog-ai.yml`
  Generates changelog entries using Claude AI based on commits and pull requests between versions.
  **Usage:** See [`examples/all-gen-changelog-ai.yml`](examples/all-gen-changelog-ai.yml)
* `all-create-tag.yml`
  Creates an annotated git tag for a specified version.
  **Usage:** See [`examples/all-create-tag.yml`](examples/all-create-tag.yml)

## Security Features

All workflows include:
- **Minimal permissions**: Only required permissions are granted
- **Timeout protection**: All jobs have timeout limits (15-30 minutes)
- **Private module support**: SSH agent configured when `SSH_PRIVATE_KEY` secret is provided
- **Dependency caching**: Faster builds with intelligent caching
- **Vulnerability scanning**: Trivy integration for security checks

## Required Secrets

Workflows may require the following secrets (configured in your repository):

- `SSH_PRIVATE_KEY`: SSH key for accessing private Git repositories
- `HARBOR_USERNAME`: Harbor registry username
- `HARBOR_PASSWORD`: Harbor registry password
- `DOCKERHUB_PASSWORD`: DockerHub password
- `REGISTRY_ACCESS_TOKEN`: DockerHub access token for pushing images (will be deprecated in favor of `DOCKERHUB_PASSWORD`)
- `TRIVY_EXPLORER_AUTH_TOKEN`: Authentication for Trivy Explorer
- `TRIVY_EXPLORER_URL`: Trivy Explorer endpoint URL
- `GITHUB_TOKEN`: Automatically provided by GitHub Actions

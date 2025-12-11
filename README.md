# mconf-ci-jobs

Reusable GitHub Actions workflows for CI/CD pipelines across Mconf projects.

## Overview

This repository provides a centralized collection of reusable GitHub Actions workflows for building, testing, linting, and deploying applications across multiple programming languages and platforms. All workflows are optimized for self-hosted runners and follow security best practices.

## Available Workflows

### Go Workflows

#### `lb-go-build.yml`
Builds Go applications with support for private modules via SSH.

**Inputs:**
- `go-version` (required): Go version to use

**Usage:**
```yaml
jobs:
  build:
    uses: mconf/mconf-ci-jobs/.github/workflows/lb-go-build.yml@main
    with:
      go-version: "1.21"
    secrets: inherit
```

#### `lb-go-lint.yml`
Runs golangci-lint on Go code.

**Inputs:**
- `go-version` (required): Go version to use
- `golangci-lint-version` (required): golangci-lint version

**Usage:******
```yaml
jobs:
  lint:
    uses: mconf/mconf-ci-jobs/.github/workflows/lb-go-lint.yml@main
    with:
      go-version: "1.21"
      golangci-lint-version: "v1.55.2"
    secrets: inherit
```

#### `lb-go-tests.yml`
Runs Go tests with coverage reporting to pull requests.

**Inputs:**
- `go-version` (required): Go version to use

**Permissions:** Requires `pull-requests: write` for coverage reports.

**Usage:**
```yaml
jobs:
  test:
    uses: mconf/mconf-ci-jobs/.github/workflows/lb-go-tests.yml@main
    with:
      go-version: "1.21"
    secrets: inherit
```

---

### Node.js Workflows

#### `lb-node-build.yml`
Builds Node.js applications.

**Inputs:**
- `node-version` (required): Node.js version

**Usage:**
```yaml
jobs:
  build:
    uses: mconf/mconf-ci-jobs/.github/workflows/lb-node-build.yml@main
    with:
      node-version: "20"
```

#### `lb-node-lint.yml`
Runs ESLint on JavaScript/TypeScript code.

**Inputs:**
- `node-version` (required): Node.js version

**Usage:**
```yaml
jobs:
  lint:
    uses: mconf/mconf-ci-jobs/.github/workflows/lb-node-lint.yml@main
    with:
      node-version: "20"
```

#### `lb-node-tests.yml`
Runs Node.js tests (only on modified JavaScript/TypeScript files).

**Inputs:**
- `node-version` (required): Node.js version

**Usage:**
```yaml
jobs:
  test:
    uses: mconf/mconf-ci-jobs/.github/workflows/lb-node-tests.yml@main
    with:
      node-version: "20"
```

---

### Python Workflows

#### `lb-py-lint.yml`
Runs Flake8, Black, and isort on Python code (pip-based projects).

**Inputs:**
- `SRC_PATH` (required): Source code path
- `python-version` (required): Python version
- `black-version` (required): Black version

**Usage:******
```yaml
jobs:
  lint:
    uses: mconf/mconf-ci-jobs/.github/workflows/lb-py-lint.yml@main
    with:
      SRC_PATH: "src"
      python-version: "3.11"
      black-version: "24.8.0"
```

#### `lb-py-pdm-lint.yml`
Runs Flake8, Black, and isort on Python code (PDM-based projects).

**Inputs:**
- `SRC_PATH` (required): Source code path
- `python-version` (required): Python version
- `black-version` (required): Black version

**Usage:******
```yaml
jobs:
  lint:
    uses: mconf/mconf-ci-jobs/.github/workflows/lb-py-pdm-lint.yml@main
    with:
      SRC_PATH: "src"
      python-version: "3.11"
      black-version: "25.1.0"
```

#### `lb-py-tests.yml`
Runs Python tests with coverage reporting (PDM-based projects).

**Inputs:**
- `SRC_PATH` (required): Source code path
- `TESTS_PATH` (required): Tests directory path
- `python-version` (required): Python version
- `env-file` (required): Path to .env file for test environment

**Permissions:** Requires `pull-requests: write` for coverage reports.

**Usage:**
```yaml
jobs:
  test:
    uses: mconf/mconf-ci-jobs/.github/workflows/lb-py-tests.yml@main
    with:
      SRC_PATH: "src"
      TESTS_PATH: "tests"
      python-version: "3.11"
      env-file: ".env.test"
    secrets: inherit
```

---

### Ruby Workflows

#### `lb-ruby-lint.yml`
Runs RuboCop linter on Ruby code.

**Inputs:**
- `ruby-version` (required): Ruby version

**Usage:**
```yaml
jobs:
  lint:
    uses: mconf/mconf-ci-jobs/.github/workflows/lb-ruby-lint.yml@main
    with:
      ruby-version: "3.2"
```

---

### Security & Docker Workflows

#### `lb-scan.yml`
Scans repository filesystem for security vulnerabilities using Trivy.

**Permissions:** Requires `pull-requests: write` to post findings.

**Usage:**
```yaml
jobs:
  scan:
    uses: mconf/mconf-ci-jobs/.github/workflows/lb-scan.yml@main
    secrets: inherit
```

#### `lb-push-scan-image.yml`
Builds Docker image, pushes to registry (on tags), and scans with Trivy.

**Features:**
- Builds Docker images with optional SSH support for private dependencies
- Pushes to DockerHub only on tags (`refs/tags/*`)
- Scans images with Trivy for critical/high vulnerabilities
- Uploads results to Trivy Explorer

**Secrets Required:**
- `REGISTRY_ACCESS_TOKEN`: DockerHub access token
- `SSH_PRIVATE_KEY` (optional): For accessing private Git repositories during build
- `TRIVY_EXPLORER_AUTH_TOKEN`: Authentication token for Trivy Explorer
- `TRIVY_EXPLORER_URL`: URL endpoint for Trivy Explorer

**Usage:**
```yaml
jobs:
  docker:
    uses: mconf/mconf-ci-jobs/.github/workflows/lb-push-scan-image.yml@main
    secrets: inherit
```

---

## Security Features

All workflows include:
- **Minimal permissions**: Only required permissions are granted
- **Timeout protection**: All jobs have timeout limits (15-30 minutes)
- **Private module support**: SSH agent configured when SSH_PRIVATE_KEY secret is provided
- **Dependency caching**: Faster builds with intelligent caching
- **Vulnerability scanning**: Trivy integration for security checks

## Required Secrets

Workflows may require the following secrets (configured in your repository):

- `SSH_PRIVATE_KEY`: SSH key for accessing private Git repositories
- `REGISTRY_ACCESS_TOKEN`: DockerHub access token for pushing images
- `TRIVY_EXPLORER_AUTH_TOKEN`: Authentication for Trivy Explorer
- `TRIVY_EXPLORER_URL`: Trivy Explorer endpoint URL
- `GITHUB_TOKEN`: Automatically provided by GitHub Actions

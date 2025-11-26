# Build Poetry Workflow

## Overview

A reusable GitHub Actions workflow for building and validating Poetry-based Python projects. This workflow runs inside a containerized environment and provides automated quality checks, security scanning, artifact generation, and container image building.

## Features

- **Containerized Execution**: Runs in a custom Poetry build container
- **Quality Checks**: Executes linting, formatting, type checking, and unit tests
- **Security Scanning**: Performs security analysis on dependencies
- **Artifact Generation**: Builds Python wheel and sdist packages
- **Container Image Building**: Creates and publishes container images
- **Coverage Reporting**: Generates and uploads test coverage reports
- **PR Comments**: Automatically posts coverage summaries as PR comments

## Workflow Jobs

### 1. Quality and Security Checks
- Runs code quality validations (linting, formatting, type checking, tests)
- Performs security scanning of dependencies
- Requires `contents: read`, `packages: read`, and `pull-requests: write` permissions

### 2. Generate Artifacts
- Builds Python distribution packages (wheel and sdist)
- Uploads artifacts for later use or validation
- Runs after quality checks pass
- Requires `contents: read` and `packages: write` permissions

### 3. Generate Container Image
- Builds and pushes container image to GitHub Container Registry
- Uses artifacts from previous job
- Generates appropriate image tags based on branch/PR
- Runs after both quality checks and artifact generation
- Requires `contents: read` and `packages: write` permissions

## Usage

### Basic Example

```yaml
name: Build Poetry

on:
  pull_request:
    branches: [main]
  push:
    branches: [main]

permissions:
  contents: read
  packages: write
  pull-requests: write

jobs:
  build:
    uses: srekit/github-workflows/.github/workflows/build-poetry.yaml@main
    secrets: inherit
```

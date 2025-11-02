# Build Poetry Workflow

## Overview

A reusable GitHub Actions workflow for running quality checks on Poetry-based Python projects. This workflow runs inside a containerized environment and provides automated code quality validation for pull requests and pushes.

## Features

- **Containerized Execution**: Runs in a custom Poetry build container
- **Quality Checks**: Executes linting, formatting, type checking, and unit tests
- **Coverage Reporting**: Generates and uploads test coverage reports
- **PR Comments**: Automatically posts coverage summaries as PR comments

## Usage

### Basic Example

```yaml
name: Build Poetry Caller

on:
  pull_request:
    branches: [main]
  push:
    branches: [main]

permissions:
  contents: read
  packages: read
  pull-requests: write

jobs:
  build:
    uses: srekit/github-workflows/.github/workflows/build-poetry.yaml@feature/build-poetry-workflow
    secrets: inherit
```

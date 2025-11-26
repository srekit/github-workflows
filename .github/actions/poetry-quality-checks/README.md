# Poetry Quality Checks Action

A GitHub Action that runs code quality checks for Python projects using Poetry.

## Features

This action performs the following quality checks on your Python codebase:

- **Linting**: Code style validation using your preferred linter
- **Type Checking**: Static type analysis
- **Security Scanning**: Vulnerability detection
- **Dependency Auditing**: Check for known security issues in dependencies
- **Code Formatting**: Verify code formatting compliance

## Usage

```yaml
name: Quality Checks

on:
  pull_request:
  push:
    branches:
      - main

jobs:
  quality:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - uses: srekit/github-workflows/poetry-quality-checks@main
        with:
          python-version: '3.11'
```

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `python-version` | Python version to use | No | `3.11` |
| `working-directory` | Working directory for the action | No | `.` |
| `poetry-version` | Poetry version to install | No | `latest` |

## Outputs

This action does not produce any outputs. It fails if any quality checks do not pass.

## Requirements

Your project must:
- Use Poetry for dependency management
- Have a `pyproject.toml` file
- Include quality check tools in your development dependencies

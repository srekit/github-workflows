# GitHub Workflows

A collection of reusable GitHub Actions and workflow templates for streamlined CI/CD processes.

## Purpose

This repository serves as a centralized hub for custom GitHub Actions and workflow templates that can be reused across multiple projects. It aims to standardize common development practices and reduce duplication in CI/CD configurations.

## Actions

- [cleanup-pr-tags](.github/actions/cleanup-pr-tags/README.md): A GitHub Action to clean up tags associated with pull requests when they are closed or merged.
- [detect-files-changes](./.github/actions/detect-files-changes/README.md): A GitHub Action to detect changes in specific files or directories within a pull request.
- [generate-tag](./.github/actions/generate-tag/README.md): A GitHub Action to automatically generate version tags based on commit messages.


## Workflows

- [build-poetry](./.github/workflows/build-poetry.md): A workflow template for building Python projects using Poetry.
- [dynamic-build-and-push](./.github/workflows/dynamic-build-and-push.md): A workflow template for dynamically building and pushing Docker images based on changes in the repository.

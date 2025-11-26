# Container Build and Push Action

## Overview

A composite GitHub Action that builds Docker images and pushes them to GitHub Container Registry (GHCR). This action automatically downloads Python package distribution artifacts and includes them in the container build context.

## Features

- **Artifact Integration**: Automatically downloads Python package distributions
- **GHCR Publishing**: Pushes images to GitHub Container Registry
- **Flexible Configuration**: Supports custom Dockerfiles, build contexts, and build arguments
- **Docker Buildx**: Uses Docker Buildx for enhanced build capabilities

## Usage

### Basic Example

```yaml
- name: Build and push container
  uses: ./.github/actions/container-build
  with:
    github-token: ${{ secrets.GITHUB_TOKEN }}
    image-name: ghcr.io/myorg/myapp
    tags: latest
```

### Advanced Example

```yaml
- name: Build and push container
  uses: ./.github/actions/container-build
  with:
    github-token: ${{ secrets.GITHUB_TOKEN }}
    image-name: ghcr.io/myorg/myapp
    tags: |
      v1.0.0
      latest
    context-path: ./src
    dockerfile-path: ./docker/Dockerfile
    build-args: |
      PYTHON_VERSION=3.13
      APP_ENV=production

```
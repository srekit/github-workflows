# Dynamic Build and Push Docker Images Workflow

## Overview

This reusable workflow automatically detects changes in Docker image directories, builds the modified images, and pushes them to GitHub Container Registry (GHCR). It also handles tag cleanup for merged pull requests.

## Workflow Triggers

This is a reusable workflow that must be called from another workflow using `workflow_call`.

## Inputs

| Input | Description | Required | Type | Default |
|-------|-------------|----------|------|---------|
| `ignore-dirs` | Comma-separated list of directories to ignore during change detection | No | string | `.github/**` |
| `target-branch` | The target branch for tag generation (used for determining when to tag as `latest`) | No | string | `main` |

## Outputs

| Output | Description | Value |
|--------|-------------|-------|
| `matrix` | JSON array of changed directories containing Docker images | Output from `detect-images-changes` job |

## Jobs

### 1. detect-images-changes

**Purpose:** Detects which directories containing Docker images have been modified.

**Runs on:** `ubuntu-latest`

**Permissions:**
- `contents: read`

**Steps:**
1. Checks out the calling repository with full git history
2. Detects changed directories using a custom action

**Outputs:**
- `changed-dirs-matrix`: JSON array of changed directories

### 2. build-and-push

**Purpose:** Builds and pushes Docker images for all changed directories.

**Runs on:** `ubuntu-latest`

**Permissions:**
- `contents: read`
- `packages: write`

**Dependencies:** Requires `detect-images-changes` job to complete successfully

**Condition:** Only runs if there are changed directories

**Strategy:** Uses a matrix strategy to build images in parallel (fail-fast: false)

**Steps:**
1. Checks out the repository
2. Generates the image name in the format: `ghcr.io/{owner}/{repo}/{folder}`
3. Generates an appropriate image tag based on the branch/PR
4. Logs in to GitHub Container Registry
5. Sets up Docker Buildx
6. Builds and pushes the Docker image with the following tags:
   - Generated tag (e.g., PR number or branch-based tag)
   - `latest` tag (only when PR is merged into the target branch)

**Image Tagging Logic:**
- All builds receive a tag from the `generate-tag` action
- The `latest` tag is only applied when:
  - Event is a pull request
  - PR action is `closed`
  - PR was merged (not just closed)
  - PR base branch matches the `target-branch` input

### 3. cleanup-pr-tags

**Purpose:** Removes PR-specific tags from GHCR after a PR is merged.

**Runs on:** `ubuntu-latest`

**Permissions:**
- `contents: read`
- `packages: write`

**Condition:** Only runs when a PR is merged into the target branch

**Steps:**
1. Checks out the repository
2. Executes cleanup action to remove PR-specific tags

## Usage Example

```yaml
name: Build Docker Images

on:
  push:
    branches: [main]
  pull_request:
    types: [opened, synchronize, closed]

jobs:
  build:
    uses: srekit/github-workflows/.github/workflows/dynamic-build-and-push.yaml@main
    with:
      ignore-dirs: '.github/**,docs/**'
      target-branch: 'main'

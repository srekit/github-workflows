# Detect Changed Files Action

A GitHub composite action that detects changed files in a repository and outputs a matrix of affected directories for use in dynamic build workflows.

## Description

This action analyzes file changes in your repository and creates a JSON array of unique first-level directories that contain modified files. It's designed to optimize CI/CD workflows by building only the components that have actually changed.

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `ignore-dirs` | Comma-separated list of directories to ignore when detecting changes | No | `.github/**` |

## Outputs

| Output | Description |
|--------|-------------|
| `changed_dirs_matrix` | JSON array of changed directories that can be used in matrix builds |

## Usage

```yaml
- name: Detect changed directories
  id: detect-changes
  uses: ./.github/actions/detect-files-changes
  with:
    ignore-dirs: '.github/**,docs/**'

- name: Use the matrix output
  if: steps.detect-changes.outputs.changed_dirs_matrix != '[]'
  strategy:
    matrix:
      directory: ${{ fromJson(steps.detect-changes.outputs.changed_dirs_matrix) }}
  run: |
    echo "Building ${{ matrix.directory }}"

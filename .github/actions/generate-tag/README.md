# Generate Tag Action

A GitHub Action that generates standardized Docker image tags based on the triggering event and branch.


## Features

- **Pull Request Tags**: Generates `PR-{short-hash}` format for PR events
- **Main Branch Tags**: Generates `{YYYYMMDD}-{short-hash}` format for pushes to main branch
- **Configurable Target Branch**: Customize which branch uses the date-based format
- **Short SHA**: Uses 7-character commit hash for compact tags


## Inputs
- `target-branch` (optional): The branch name that should use the date-based tag format. Default is `main`.


## Outputs
- `tag`: The generated Docker image tag.


## Tag Formats

### Pull Request Events

Format: `PR-{7-char-hash}`
Example: `PR-a1b2c3d`
Triggered by: Any pull request event


### Target Branch Push Events

Format: `{YYYYMMDD}-{7-char-hash}`
Example: `20241201-a1b2c3d`
Triggered by: Push events to the target branch (default: main)


## Usage

### Basic Usage

```
- name: Generate Image Tag
  id: tag
  uses: ./.github/actions/generate-tag

- name: Use Generated Tag
  run: |
    echo "Generated tag: ${{ steps.tag.outputs.tag }}"
    docker build -t myapp:${{ steps.tag.outputs.tag }} .
```

### Custom Target Branch

```
- name: Generate Image Tag
  id: tag
  uses: ./.github/actions/generate-tag
  with:
    target-branch: 'production'  # Use date format for 'production' branch instead of 'main'
```


## Error Handling

The action will fail with an error message if triggered by unsupported events (anything other than push or pull_request).


# Requirements

### GitHub Actions environment

- `GITHUB_SHA` environment variable (automatically provided by GitHub Actions)
- `GITHUB_EVENT_NAME` environment variable (automatically provided by GitHub Actions)
- `GITHUB_REF` environment variable (automatically provided by GitHub Actions)

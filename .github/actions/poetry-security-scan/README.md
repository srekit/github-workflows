# Poetry Security Scan Action

A comprehensive GitHub Action that performs security scanning on Poetry-based Python projects using multiple security tools.

## Features

- **Safety Check**: Scans Python dependencies for known security vulnerabilities
- **Bandit**: Analyzes Python code for common security issues
- **pip-audit**: Audits Python packages for known vulnerabilities
- **PR Comments**: Automatically posts security scan results as comments on pull requests
- **Artifact Upload**: Saves detailed security reports for review
- **Configurable Severity**: Fail builds based on vulnerability severity levels

## Usage

### Basic Usage

```yaml
- name: Run Security Scan
  uses: srekit/github-workflows/.github/actions/poetry-security-scan@main
  with:
    github-token: ${{ secrets.GITHUB_TOKEN }}
```

### Advanced Usage

```yaml
- name: Run Security Scan
  uses: srekit/github-workflows/.github/actions/poetry-security-scan@main
  with:
    github-token: ${{ secrets.GITHUB_TOKEN }}
    python-version: '3.13'
    poetry-version: '1.8.0'
    fail-on-severity: 'high'
```

## Inputs

| Input               | Description                                                                      | Required | Default  |
|---------------------|----------------------------------------------------------------------------------|----------|----------|
| `github-token`      | GitHub token for posting PR comments                                             | Yes      | -        |
| `python-version`    | Python version to use                                                            | No       | `3.13`   |
| `poetry-version`    | Poetry version to install                                                        | No       | `1.8.0`  |
| `fail-on-severity`  | Fail on vulnerabilities of this severity or higher (low, medium, high, critical) | No       | high     |

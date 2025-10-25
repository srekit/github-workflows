# Cleanup PR Tags Action

This action is designed to delete all Docker image tags associated with a closed and merged pull request (PR).
It uses the GitHub API to identify and remove tags that match the naming pattern `PR-[PR Number]-*`.

## Inputs
- `pr-number` (required): The PR number for which tags should be cleaned up.
- `github-token` (required): A GitHub token with `packages:delete` permission to authenticate API requests.
 ## Outputs
- `deleted-tags`: A list of tags that were successfully deleted during the cleanup process.
 ## Usage
This action is typically used in workflows triggered by PR closure or merge events. It ensures that tags
associated with PRs are cleaned up to maintain a clean and organized container registry.
 ## Example Workflow
```yaml
cleanup-pr-tags:
  runs-on: ubuntu-latest
  permissions:
    contents: read
    packages: write
  if: github.event_name == 'pull_request' && github.event.action == 'closed' && github.event.pull_request.merged == true
  steps:
    - name: Checkout code
      uses: actions/checkout@v4
    - name: Cleanup PR Tags
      uses: ./.github/actions/cleanup-pr-tags
      with:
        pr-number: ${{ github.event.pull_request.number }}
        github-token: ${{ secrets.GITHUB_TOKEN }}
```

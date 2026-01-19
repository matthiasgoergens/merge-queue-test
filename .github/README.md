# GitHub Merge Queue Configuration

This directory contains the GitHub Actions workflows configured to support GitHub's merge queue functionality.

## Workflows

### 1. CI Workflow (`ci.yml`)
- Runs on pull requests and merge group events
- Performs basic validation (tests, build, lint)
- Required status checks for merge queue

### 2. Merge Queue Validation (`merge-queue-validation.yml`)
- Specifically runs when PRs enter the merge queue
- Validates the merged state before final merge
- Displays merge queue information

## How to Enable Merge Queue

To use the merge queue with these workflows:

1. Go to your repository **Settings** → **Branches**
2. Add or edit a branch protection rule for `main` (or your default branch)
3. Enable the following settings:
   - ✅ **Require status checks to pass before merging**
   - ✅ Select the workflows: `test` and `validate-merge`
   - ✅ **Require merge queue**
4. Choose your merge method (merge commit, squash, or rebase)
5. Optionally configure:
   - Maximum number of PRs to build at once (1-100)
   - Minimum number of PRs to merge at once (1-100)
   - Maximum time to wait for status checks (0-360 minutes)

## How It Works

1. When a PR is ready and approved, add it to the merge queue
2. GitHub creates a temporary branch that merges your PR with the latest main
3. The workflows run against this temporary merge
4. If all checks pass, the PR is automatically merged
5. If checks fail, the PR is removed from the queue and maintainers are notified

## Testing the Merge Queue

To test this setup:
1. Create a pull request with any changes
2. Once approved, add it to the merge queue (if enabled in settings)
3. Watch the workflows run on the `merge_group` event
4. The PR will be automatically merged when all checks pass

## Additional Resources

- [GitHub Docs: Managing a merge queue](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/configuring-pull-request-merges/managing-a-merge-queue)
- [GitHub Blog: Merge Queue Announcement](https://github.blog/news-insights/product-news/github-merge-queue-is-generally-available/)

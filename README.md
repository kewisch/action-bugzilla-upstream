action-bugzilla-upstream
========================

This small action will monitor bugzilla on a weekly basis to see if the referenced bug has been fixed.

Add `.github/workflows/upstream.yaml` with this content:


```yaml
name: Bugzilla Upstream Checker

on:
  workflow_dispatch:
  schedule:
    - cron: '0 0 * * 0'  # Runs at 00:00 every Sunday.

permissions:             # You need write permissions to create comments
  issues: write
  
jobs:
  check_upstream_issues:
    runs-on: ubuntu-latest

    steps:
      - name: Check and Comment on Upstream Issues
        uses: kewisch/action-bugzilla-upstream@v1
        with:
          bugzilla: bugzilla.mozilla.org              # default value
          label: upstream                             # default value
          github-token: ${{ secrets.GITHUB_TOKEN }}   # provide the secret

```


Then, when there is an issue you'd like to monitor, apply the `upstream` label, and comment the full
bugzilla link in your issue. When the workflow runs, it will find your comment and check the status
for the issue.

Note it will only accept comments by repository owners and members. If someone else notes the issue,
you'll have to repeat the link in your comment.

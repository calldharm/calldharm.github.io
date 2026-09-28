# Branch protection guidance for calldharm/calldharm.github.io

I added a CODEOWNERS file that assigns ownership of all files to @calldharm. To make this effective and prevent others from altering your main branch, enable branch protection for the `main` branch with these settings (requires repository admin access):

Recommended protection settings:
- Require pull request reviews before merging (set required approving review count to 1 or more)
- Require review from Code Owners
- Require status checks to pass before merging (optional — add your CI checks)
- Include administrators (enforce for admins)
- Restrict who can push to matching branches (set to specific users/teams)
- Do not allow force pushes

Example curl command (replace YOUR_TOKEN with a personal access token having admin:repo scope):

curl -X PUT \
  -H "Accept: application/vnd.github+json" \
  -H "Authorization: token YOUR_TOKEN" \
  https://api.github.com/repos/calldharm/calldharm.github.io/branches/main/protection \
  -d '{
    "required_status_checks": {"strict": true, "contexts": []},
    "enforce_admins": true,
    "required_pull_request_reviews": {"dismiss_stale_reviews": true, "require_code_owner_reviews": true, "required_approving_review_count": 1},
    "restrictions": {"users": ["calldharm"], "teams": []}
  }'

Or using the GitHub CLI (gh) to set branch protection:

gh api --method PUT \
  -H "Accept: application/vnd.github+json" \
  /repos/calldharm/calldharm.github.io/branches/main/protection \
  -f required_status_checks='{"strict": true, "contexts": []}' \
  -f enforce_admins=true \
  -f required_pull_request_reviews='{"dismiss_stale_reviews": true, "require_code_owner_reviews": true, "required_approving_review_count": 1}' \
  -f restrictions='{"users":["calldharm"],"teams":[]}'

Important:
- The API call must be run by a repository administrator.
- The CODEOWNERS file must be present on the default branch for "Require review from Code Owners" to have effect.

If you want, I can run the API call for you if you provide a personal access token with admin:repo scope — otherwise follow the steps above in your shell.

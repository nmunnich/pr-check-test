# GHA PR check test repo

A minimal repo to experiment with creating check runs on a pull request via
`github.rest.checks.create`, like the Netlify `deploy-preview` check or the
`docs-preview` check added to west's `docs-publish-pr.yml`.

## How to test

1. Push this repo to GitHub (any name, e.g. `pr-check-test`).
2. Open a PR: the `demo-change` branch has a commit ready to use as the PR head.
   Open a PR from `demo-change` into `main` and note its PR number.
3. Go to **Actions → Create PR check → Run workflow** (on `main` or any branch)
   and enter:
   - `pr_number`: the PR number from step 2
   - `preview_url`: any URL, e.g. `https://example.com/doc/`
4. Once the run finishes, look at the PR: a `docs-preview` check appears in
   the commit checks / PR conversation, with title "Documentation preview
   ready!" — clicking it follows `details_url` (the preview URL).

## Notes

- `checks: write` in the workflow `permissions` is what allows creating the
  check run; `pull-requests: read` lets the script look up the PR head SHA.
- The check is created directly on the PR head SHA, so it shows up on the
  exact commit the PR is at — same mechanism Netlify uses for its
  `netlify/zmk/deploy-preview` check.
- Re-running the workflow creates a new check run with the same name on the
  same SHA; GitHub shows the latest one.
- This test repo uses `actions/github-script@v7` unpinned for clarity; the
  west workflows pin action SHAs.

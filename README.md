# GHA PR check test repo

A minimal repo to experiment with creating check runs on a pull request via
`github.rest.checks.create`, like the Netlify `deploy-preview` check or the
`docs-preview` check added to west's `docs-publish-pr.yml`.

## How to test

1. Push this repo to GitHub (any name, e.g. `pr-check-test`).
2. Open a PR from `demo-change` into `main` (the branch already has a commit
   ready to use as the PR head).
3. The workflow runs automatically on `pull_request: opened` — no manual step.
4. Look at the PR: a `docs-preview` check appears in the commit checks / PR
   conversation, with title "Documentation preview ready!" — clicking it
   follows `details_url` (the preview URL).

You can also trigger it manually via **Actions → Create PR check → Run
workflow**; the check is then created on the commit of the selected branch.

## Notes

- The first step exports `PREVIEW_URL` (and the PR head SHA) via `GITHUB_ENV`;
  the check step reads them as plain environment variables. No workflow inputs.
- `HEAD_SHA` uses `github.event.pull_request.head.sha` on PR events so the
  check lands on the actual PR head commit (on a `pull_request` event
  `github.sha` is the merge commit). It falls back to `github.sha` for manual
  runs.
- `checks: write` in the workflow `permissions` is what allows creating the
  check run.
- The check is created directly on the head SHA of the PR, so it shows up in
  the PR's checks — same mechanism Netlify uses for its
  `netlify/zmk/deploy-preview` check.
- Re-running the workflow creates a new check run with the same name on the
  same SHA; GitHub shows the latest one.
- This test repo uses `actions/github-script@v7` unpinned for clarity; the
  west workflows pin action SHAs.

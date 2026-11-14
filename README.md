# GHA PR check test repo

A minimal repo to experiment with creating check runs on a pull request via
`github.rest.checks.create`, like the Netlify `deploy-preview` check or the
`docs-preview` check added to west's `docs-publish-pr.yml`.

## How to test

1. Push this repo to GitHub (any name, e.g. `pr-check-test`).
2. Open a PR: the `demo-change` branch has a commit ready to use as the PR head.
   Open a PR from `demo-change` into `main`.
3. Go to **Actions → Create PR check → Run workflow**, select the `demo-change`
   branch (the PR head — the check is created on the commit the workflow runs
   against, `context.sha`) and run it.
4. Once the run finishes, look at the PR: a `docs-preview` check appears in
   the commit checks / PR conversation, with title "Documentation preview
   ready!" — clicking it follows `details_url` (the preview URL).

## Notes

- The first step exports `PREVIEW_URL` via `GITHUB_ENV`; the check step reads
  it as a plain environment variable (`process.env.PREVIEW_URL`). No workflow
  inputs.
- `checks: write` in the workflow `permissions` is what allows creating the
  check run.
- The check is created directly on the head SHA of the PR, so it shows up in
  the PR's checks — same mechanism Netlify uses for its
  `netlify/zmk/deploy-preview` check.
- Re-running the workflow creates a new check run with the same name on the
  same SHA; GitHub shows the latest one.
- This test repo uses `actions/github-script@v7` unpinned for clarity; the
  west workflows pin action SHAs.

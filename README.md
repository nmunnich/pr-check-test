# GHA PR check test repo

A minimal repo to experiment with creating check runs on a pull request via
`github.rest.repos.createCommitStatus`, like the Netlify `deploy-preview` check..

## Notes

- The first step exports `PREVIEW_URL` (and the PR head SHA) via `GITHUB_ENV`;
  the check step reads them as plain environment variables, for ease of demonstration.
- `statuses: write` in the workflow `permissions` is what allows creating the
  check run.
- This test repo uses `actions/github-script@v7` unpinned for clarity; pin it 
  if you use this in practice.

DEMO CHANGE
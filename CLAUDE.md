# CLAUDE.md

## Git Workflow

- Always run `git pull` before making any changes to ensure you have the latest code.
- Always run `git pull` again before pushing to catch any changes made while you were working (e.g. automated CI commits).
- If there are merge conflicts, resolve them before continuing. Prefer keeping both sets of changes when possible.
- Never force push.
- When creating a PR for an issue, use branch name `issue-<number>` and include `Closes #<number>` in the commit message.

## Project Notes

- This is a single-file PWA (`index.html`) with a service worker (`sw.js`).
- `sw.js` uses a cache-first strategy. The `CACHE_NAME` in `sw.js` is automatically bumped by a GitHub Action on every push to `main` — do NOT manually edit `CACHE_NAME`.
- All CSS and JS lives inline in `index.html`. There are no build tools or bundlers.

## CI Workflows

- **bump-sw-cache.yml** — Automatically bumps `CACHE_NAME` in `sw.js` on every push to main.
- **claude.yml** — Triggers Claude on any issue or PR comment containing `@claude`. Only authorized users can trigger it (allowlist check). To add users, edit the `ALLOWED_USERS` env var (comma-separated) in the workflow file.

## Using Claude via Workflow

Mention `@claude` in any issue or PR comment to trigger Claude. Examples:

- `@claude implement this issue`
- `@claude review this PR`
- `@claude fix the failing tests`

Only users listed in `ALLOWED_USERS` in `claude.yml` can trigger Claude (currently: `mr-menno`).

## Deployment

The app is deployed to GitHub Pages at [mr-menno.github.io/run90](https://mr-menno.github.io/run90/) from the `main` branch.

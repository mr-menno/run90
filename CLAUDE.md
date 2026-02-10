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
- **claude-plan.yml** — When an issue is labeled `needs-plan`, Claude reads the codebase and posts an implementation plan as an issue comment.
- **claude-implement.yml** — When someone comments `@claude-implement` on an issue, Claude executes the plan: writes code, commits, and opens a PR.

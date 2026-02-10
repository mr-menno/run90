# CLAUDE.md

## Git Workflow

- Always run `git pull` before making any changes to ensure you have the latest code.
- Always run `git pull` again before pushing to catch any changes made while you were working (e.g. automated CI commits).
- If there are merge conflicts, resolve them before continuing. Prefer keeping both sets of changes when possible.
- Never force push.

## Project Notes

- This is a single-file PWA (`index.html`) with a service worker (`sw.js`).
- `sw.js` uses a cache-first strategy. The `CACHE_NAME` in `sw.js` is automatically bumped by a GitHub Action on every push to `main` — do NOT manually edit `CACHE_NAME`.

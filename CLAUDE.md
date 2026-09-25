# Melvani (મેળવણી)

Single-file PWA (`index.html`) for proofreading handwritten scans against the typed Google Doc on phone, iPad and Mac. See README.md for user-facing setup.

- Hosting: GitHub Pages from `main` root → https://manavk7-lab.github.io/melvani/ (repo manavk7-lab/melvani, public).
- `demo/` (notebook pages 10–11) and `tests/` are git-ignored on purpose: the repo is public.
- Google Cloud project: `skilful-rite-435413-t0` (same as ~/.google-multi-mcp). Docs + Drive APIs enabled; consent screen is External, In production.
- OAuth: Web client with origin `https://manavk7-lab.github.io` and redirect `https://manavk7-lab.github.io/melvani/`. Client "Melvani" (217460952409-n80mv52…) is set in `CONFIG.CLIENT_ID` in index.html (empty → demo mode; `?demo` forces demo).
- Saves: Docs API batchUpdate with minimal diffs + `targetRevisionId` retry. Progress syncs via Drive appDataFolder.
- Untested: the ગુ typing helper (unofficial Google input-tools) and iPad home-screen sign-in.
- Bump `VERSION` in sw.js when shipping changes to cached shell files.
- Scans source: a Drive folder, or a single PDF (`scanFile: true` on the project, `folderId` then holds the file id).

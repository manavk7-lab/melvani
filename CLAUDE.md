# Melvani (મેળવણી)

Single-file PWA (`index.html`) for proofreading handwritten scans against the typed Google Doc on phone, iPad and Mac. See README.md for user-facing setup.

- Hosting: GitHub Pages from `main` root → https://manavk7-lab.github.io/melvani/ (repo manavk7-lab/melvani, public).
- `demo/` (notebook pages 10–11) and `tests/` are git-ignored on purpose: the repo is public.
- Google Cloud project: `skilful-rite-435413-t0` (same as ~/.google-multi-mcp). Docs + Drive APIs enabled; consent screen is External, In production.
- OAuth: Web client with origin `https://manavk7-lab.github.io` and redirect `https://manavk7-lab.github.io/melvani/`. Client "Melvani" (217460952409-n80mv52…) is set in `CONFIG.CLIENT_ID` in index.html (empty → demo mode; `?demo` forces demo).
- Saves: Docs API batchUpdate with minimal diffs and `requiredRevisionId` (not target: after an outside edit, target made the *second* save land at stale indexes — verified 2026-09-25). A refused save reloads the Doc and re-applies via `findPara`. Progress syncs via Drive appDataFolder.
- Editing is autosave (user asked, 2026-09-25): one tap opens a paragraph, `Edit` saves ~1.2 s after typing stops, serially through `Edit.chain`; no save while composing or while a ગુ English-letter run is pending. A save with no answer sets `Edit.unsure` and the retry first reloads to check whether it went in (never type a fix twice). Corrections log: one entry per spot, merged while typing.
- The handwriting and the text move independently (user asked, 2026-09-25): no page↔text links, no pin, no "Show it" pill. `P().pos` keeps both places; old `links` data is left in the store, unused.
- The signed-in account needs edit rights on the Doc (`Doc.canEdit` from Drive capabilities; view-only shows a banner). manavk7@gmail.com was given Editor on the Yatharth Geeta Sar Doc (owner upnishad321@gmail.com) on 2026-09-25.
- Local check: `melvani` preview server in ~/.claude/launch.json → http://localhost:8123/?demo (demo pages are only local).
- Untested: the ગુ typing helper (unofficial Google input-tools) and iPad home-screen sign-in.
- Bump `VERSION` in sw.js when shipping changes to cached shell files.
- Scans source: a Drive folder, or a single PDF (`scanFile: true` on the project, `folderId` then holds the file id).

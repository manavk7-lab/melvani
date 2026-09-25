# Melvani (મેળવણી)

Proofread handwritten pages against the typed Google Doc — on the Samsung phone, the iPad and the MacBook.

The scanned page and the typed text sit together on one screen. Tap a paragraph, tap it again where the mistake is, fix it, press **Save**: the change goes straight into the Google Doc with its formatting kept. Each handwritten page is linked to the paragraph where it starts, so turning a page moves the text with it. Progress (checked pages, page links, the list of corrections) is saved in a private app folder in your Google Drive, so all three devices stay in step.

Until the setup below is done, the app runs as a **demo** with notebook pages 10–11 and a few paragraphs of *Yatharth Geeta Sar · Book 1*.

---

## One-time setup (about 20 minutes)

### 1. Put the app on GitHub Pages

1. Sign in at github.com and create a new repository named **`melvani`**. It must be **Public** (free GitHub Pages needs that). No secrets live in these files.
2. In the new repository: **Add file → Upload files**. Drag in everything from this folder: `index.html`, `manifest.webmanifest`, `sw.js`, `README.md`, and the `icons` and `demo` folders. Click **Commit changes**.
   - `demo/` holds two of your notebook pages (10 and 11). Because the repository is public, leave `demo/` out if you don't want those pages online — the app works without it.
3. **Settings → Pages**. Under *Build and deployment*, set Source to **Deploy from a branch**, Branch **main**, folder **/ (root)**, then **Save**.
4. After a minute or two the app is live at `https://YOUR-GITHUB-USERNAME.github.io/melvani/`. Opening it now shows the demo.

### 2. Let the app use your Google account

Use the same Google Cloud project as your gdocs-mcp server, or a new one — either works.

1. Open **console.cloud.google.com** and pick the project.
2. **APIs & Services → Library**: enable **Google Docs API** and **Google Drive API**.
3. **Google Auth Platform** (older consoles call it *OAuth consent screen*):
   - **Branding / Get started**: App name `Melvani`, your email as support and contact email. Audience: **External**. Create.
   - **Audience → Test users → Add users**: add the Gmail address that owns the Doc and the scans. Leave the app in **Testing** — there is no need to publish or verify it for your own use.
   - **Data access → Add or remove scopes** (optional but tidy): add `…/auth/documents`, `…/auth/drive.readonly`, `…/auth/drive.appdata` and `email`.
4. **Clients → Create client**:
   - Application type: **Web application**, name `Melvani`.
   - **Authorized JavaScript origins**: `https://YOUR-GITHUB-USERNAME.github.io`
   - **Authorized redirect URIs**: `https://YOUR-GITHUB-USERNAME.github.io/melvani/` (with the slash at the end)
   - **Create**, then copy the **Client ID** (it ends in `.apps.googleusercontent.com`).

### 3. Paste the Client ID into the app

1. On GitHub, open `index.html` and click the pencil (Edit).
2. Near the top of the script, find:
   ```js
   CLIENT_ID: '',        // e.g. '1234567890-abc123.apps.googleusercontent.com'
   ```
   and paste your Client ID between the quotes.
3. **Commit changes**. A minute later the live app asks you to sign in instead of showing the demo.
   (The demo is still reachable at `…/melvani/?demo`.)

### 4. Open it on each device

- **MacBook (Chrome)**: open the address, **Sign in with Google**, then **Add your first book**: paste the Google Doc link (from the address bar while the Doc is open) and the link of the Drive folder that holds the scans. Optional: click the install icon at the right end of Chrome's address bar to get a Melvani app window.
- **Samsung (Chrome)**: open the address, sign in. Then **⋮ → Add to home screen → Install**.
- **iPad (Safari)**: open the address, sign in. Then **Share → Add to Home Screen**.
  If sign-in doesn't return to the home-screen app on the iPad, use Melvani in a normal Safari tab instead — sign-in always works there.

The first sign-in shows *"Google hasn't verified this app"*. It's your own copy: choose **Continue**. Google gives browser apps a one-hour sign-in, so Melvani asks you to sign in again every hour; anything you were typing is kept.

---

## The scans

- Put the scanned pages in **one Drive folder** as JPG, PNG or PDF (a PDF can hold many pages).
- Pages are taken in file-name order with numbers sorted properly: `10.jpg` comes before `11.jpg` and `100.jpg`. If a file name ends in a number of up to four digits, that number is shown as the page number.
- iPhone HEIC photos can't be shown by browsers — export them as JPG. Samsung camera photos are already JPG.
- A sideways photo can be turned with the rotate button; the app remembers the turn for that page.

## Daily use

| | |
|---|---|
| **Fix a word** | Tap the paragraph, tap again at the mistake, fix it, press **Save** (or Enter). **Shift+Enter** adds a line break inside the paragraph. |
| **Undo** | In the message after saving, or in **⋯ → Corrections made**. |
| **Finish a page** | **Page done** marks it checked, goes to the next page, and links that page to where you are in the text. |
| **Fix a page link** | Tap the paragraph where the page really starts, then the **pin**. The rose *p.* label in the margin shows where each page begins. |
| **Find** | **⋯ → Find**, or `/` on the Mac: a verse (`૧/૧૧` or `1/11`), a page (`p 12`), or any words. Finding a verse also turns to its page. |
| **Handwriting** | Pinch or ⌘+scroll to zoom, drag to move, double-tap to zoom in or back. The **ruler** dims all but the line you're reading; `↑`/`↓` move it one line on the Mac. |
| **Gujarati typing** | Use the Gujarati keyboard on the phone and iPad (Gboard can turn English letters into Gujarati). While editing, the **ગુ** button does the same inside Melvani: type `bruh`, pick `બૃહ`. |

Mac keys: `←` `→` pages · `E` edit · `D` page done · `P` pin · `R` ruler · `+` `−` `0` zoom · `Esc` cancel.

## What it keeps, and where

- **Your Doc**: each Save sends only the changed letters to Google Docs (formatting stays). Nothing else in the Doc is touched.
- **Progress** (checked pages, page links, corrections list): a hidden file in your Drive's app-data area, readable only by Melvani. To erase it: Google Drive → Settings → **Manage apps** → Melvani → **Delete hidden app data**.
- **On each device**: the one-hour sign-in and view settings (text size, zoom, pane sizes).
- **Other services**: fonts come from Google Fonts; a PDF reader loads from cdnjs only when the folder has PDFs; the **ગુ** typing helper sends the English letters you type to Google's input-tools service. That service is unofficial and may stop working someday — the device keyboard always works.

## Limits

- Splitting or joining paragraphs, and anything touching images or tables of contents, is done in Google Docs.
- If someone edits the Doc at the same time, Melvani reapplies your fix on the latest version. If that paragraph itself changed, it reloads the text and asks you to redo the fix.
- Suggestion mode isn't used; saves are direct edits.

## Updating the app later

Replace `index.html` with a newer version and paste your Client ID again (or copy your `CLIENT_ID` line across). Everything else stays.

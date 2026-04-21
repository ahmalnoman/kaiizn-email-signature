# Kaiizn Email Signature

Internal tool for the Kaiizn team to generate a consistent, on-brand email signature that renders reliably in Gmail, Outlook, Apple Mail, and beyond.

**Live tool:** https://ahmalnoman.github.io/kaiizn-email-signature/

---

## For team members

1. Open the link above.
2. Fill in your name, role, office, and optionally phone/LinkedIn.
3. Click **Copy signature**.
4. Paste it into your email client's signature settings. Step-by-step for each client is built in — click **Install guide**.

Keyboard shortcut: `⌘⏎` (macOS) / `Ctrl+Enter` (Windows/Linux) to copy.

---

## For admins — deploying to GitHub Pages

The tool is a single static `index.html` + a few image assets. GitHub Pages is perfect.

```bash
# From the repo root, after cloning / initializing:
git add .
git commit -m "Initial signature generator"
git branch -M main
git remote add origin git@github.com:ahmalnoman/kaiizn-email-signature.git
git push -u origin main
```

Then in the repo on GitHub:

1. **Settings → Pages**
2. **Source:** `Deploy from a branch`
3. **Branch:** `main` · `/ (root)` → **Save**
4. Wait ~30 seconds. Your site is at `https://ahmalnoman.github.io/kaiizn-email-signature/`.

---

## How it's built

Single-file vanilla HTML/CSS/JS — no build step, no dependencies.

- **`index.html`** — the whole app (form, previews, install guide, clipboard logic).
- **`kaiizn-icon-email.png`** — 512×512 source icon used inside the generated signature HTML.
- **`kaiizn-icon.png`, `kaiizn-logo-*.png/svg`** — brand assets referenced by the UI/preview.

### Why hosted images (not base64)?

Gmail silently truncates signatures over ~10KB, and Outlook sometimes strips `data:` URIs. Hosting the logo as a public `https://` URL means:

- Gmail's image proxy fetches and caches it.
- The same tiny image is reused across every email you send (recipients' clients cache it too).
- Signatures stay lightweight.

### Updating the logo

1. Replace `kaiizn-icon-email.png` with a new 512×512 PNG (transparent background).
2. Commit and push. GitHub Pages auto-redeploys in seconds.
3. Already-sent emails still show the old image (they cache by URL). New emails show the new one.

### Changing the hosted base URL (if you ever move the repo)

Edit the one constant near the top of the `<script>` block in `index.html`:

```js
const DEFAULT_ASSETS_BASE = 'https://ahmalnoman.github.io/kaiizn-email-signature';
```

---

## Cross-client compatibility

Tested patterns in use:

- **Table-based layout** with inline styles (Gmail strips `<style>` blocks).
- **Explicit `width`/`height` attributes** on the `<img>` tag (Outlook requirement).
- **`role="presentation"`** on layout tables for screen readers.
- **Neutral colors** that read on both light and dark reading panes.
- **Gradient K** on transparent PNG — reads on any background without a separate dark-mode asset.

---

## Pre-filled links

Share a pre-filled link with a teammate — form state is in the URL:

```
https://ahmalnoman.github.io/kaiizn-email-signature/?name=Ahmed%20Noman&role=Software%20Engineer&office=Cairo
```

Supported keys: `name`, `role`, `office`, `phone`, `linkedin`.

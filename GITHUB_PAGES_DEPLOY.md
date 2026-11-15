# Publish the website with GitHub Pages (no Render)

For GitHub Free, GitHub Pages requires a public repository. The website will be public, so never upload confidential SQL seed data or private keys.

Upload these website files/folders to the repository root:
- `index.html`
- `styles.css`
- `app.js`
- `cases.json`
- `config.js` (contains only the Supabase URL and publishable/anon key)
- `assets/`

Do NOT upload `supabase/seed_cases.sql`. It contains the private case solutions and prepared suspect answers. Keep it on your device and run it only in the Supabase SQL Editor.

Then open repository Settings → Pages → Deploy from a branch → `main` → `/(root)` → Save.

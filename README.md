# SEY GAMERZ CRIME CITY — Full source package

## Architecture — no Render
- GitHub stores the source.
- GitHub Pages serves the static front-end.
- Supabase provides authentication, Postgres, RLS, RPCs and Realtime.
- Supabase's GitHub integration/CLI can deploy database migrations from `supabase/migrations/`.
- Render is not used.

**Important:** Supabase is the backend, not the static website host. GitHub Pages is the front-end host.

## Package contents
- `index.html`, `styles.css`, `app.js`
- `cases.json`: public case catalog with 100 distinct cases, five suspects per case and question prompts; no answer keys or culprit solutions.
- `assets/`: cinematic city artwork and five suspect portrait assets.
- `config.example.js`: Supabase URL and publishable/anon key template.
- `supabase/schema.sql`: tables, RLS policies and secure RPC functions.
- `supabase/migrations/20261115000100_sgcc_core.sql`: migration copy for Supabase CLI/GitHub integration.
- `supabase/seed_cases.sql`: confidential case answer keys, suspect answers, evidence details and solutions. **Do not commit this file to a public GitHub repository.** Keep it on your device and run its contents in Supabase SQL Editor.

## Case library
The package contains 100 distinct case records, 500 suspect dossiers, 2,500 interrogation question/answer pairs and 500 evidence entries. The five bundled suspect portrait assets are reused as visual portrait assets across the case library; the case text and data are distinct, but this package does not contain 500 individually generated headshots.

## Supabase setup
1. Create a Supabase project.
2. In SQL Editor, run `supabase/schema.sql` (or apply the migration with Supabase CLI).
3. On your device, open `supabase/seed_cases.sql`, copy its full contents, and run it in Supabase SQL Editor. Do not put this confidential seed file in a public repository.
4. Enable Email authentication.
5. Register the host account in the app.
6. Promote the host account using SQL Editor, replacing the email:
   `insert into public.profiles(user_id,display_name,role) select id,'Crime City Host','host' from auth.users where email='YOUR_HOST_EMAIL' on conflict(user_id) do update set role='host';`
7. Copy `config.example.js` to `config.js`; enter your Supabase Project URL and publishable (anon) key. Never use a service_role or secret key in browser code.
8. Upload the public website files to GitHub: `index.html`, `styles.css`, `app.js`, `cases.json`, `config.js`, `assets/`, and (optionally) README. Do **not** upload `supabase/seed_cases.sql`.
9. For a GitHub Free account, the Pages source repository must be public. The app's public case catalog is designed not to contain culprit solutions or prepared answers. Keep the confidential seed SQL out of the repository.
10. In GitHub repository Settings → Pages, choose Deploy from a branch, branch `main`, folder `/(root)`, Save.

## Security design
- The full culprit, solution explanation, interrogation answers and private evidence details live in `case_files.private_content` in Supabase. Direct table access is revoked.
- Secure SECURITY DEFINER RPCs perform room creation, joining, request creation, host approval, coin deductions, accusation submission and result release.
- Profile creation defaults to player. A user cannot promote their own role through the app; the project owner promotes the host in SQL.
- The browser uses only the Supabase publishable/anon key. Never expose service_role or secret keys.
- Public suspect cards are shuffled; the public catalog does not include the culprit answer key.

## Operational behaviour
- Maximum 20 players per room; registration closes when the host starts the case.
- Each player starts with 100 Crime Coins.
- Public evidence costs 10 CC; private evidence costs 15 CC; interrogation answers cost 3 CC.
- Every purchase request requires host approval. Coins are deducted only on approval.
- Investigation is 30 minutes, followed by a 10-minute accusation phase.
- Keep the host dashboard open during the session: it monitors the phase deadline and advances the phase through the authenticated host RPC.
- Completed cases are locked in the Supabase `used_cases` table.

## Important pre-launch checks
This source package has not been connected to your Supabase project or tested with your live credentials. Before using it for a real tournament, test with two separate devices/accounts:
- host-only actions are rejected for player accounts;
- private evidence and interrogation answers cannot be read by other players;
- public evidence is visible to all room members;
- deductions happen exactly once after approval;
- the timer transitions correctly when the host dashboard remains open;
- a completed case cannot be selected again;
- the final culprit is unavailable until the case is completed.

Free Supabase projects may pause after inactivity. Keep a secure backup of the database and case seed.

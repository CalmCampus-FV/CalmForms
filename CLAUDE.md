# CalmForms

The CalmCampus forms platform: public form pages, the ingest Worker, the database and the private admin. Moved here from CalmWeb with its history. The website is [CalmCampus-FV/CalmWeb](https://github.com/CalmCampus-FV/CalmWeb); university demos [CalmUni](https://github.com/CalmCampus-FV/CalmUni), company demos [CalmBiz](https://github.com/CalmCampus-FV/CalmBiz), the iOS app [CalmApp](https://github.com/CalmCampus-FV/CalmApp), the research and design archive [CalmCampus](https://github.com/CalmCampus-FV/CalmCampus). Architecture, privacy model and go-live steps: `FORMS.md`.

## Where it runs today (do not break)

- Public forms: `https://cc.braunf.com/f/<slug>` (for example `/f/case-study`). The static files are CalmWeb's GitHub Pages copies of `docs/f/`, `docs/forms/` and `docs/tokens.css` from this repo; the Cloudflare Worker `calmweb-forms` (`worker/`) answers `cc.braunf.com/api/*`, `/f/*`, `/forms/*`, `/tokens.css`.
- Admin: `https://ccadmin.braunf.com`, the Cloudflare static site `calmweb-admin` (`admin/wrangler.toml`), deployed with `bash scripts/deploy-admin.sh`.
- Data: Supabase (EU). The Worker deploy, the admin deploy and Supabase settings are done by hand by Florian (they need Cloudflare and Supabase logins); CI never deploys them.
- After changing `docs/f/`, `docs/forms/` or `docs/tokens.css` here, run `bash scripts/sync-forms.sh` in CalmWeb and merge that PR; CalmWeb's Source sync workflow fails until the copies match.

Planned (prepared, not live): forms at `form.calmcampus.app` (this repo's own Pages site, `docs/CNAME`, `.github/workflows/pages.yml`; GitHub ignores the CNAME file for Actions deploys until the domain is set in Settings > Pages) and the admin at `admin.calmcampus.app`. Steps in `FORMS.md`, "Moving to form.calmcampus.app".

## Layout

- `docs/` - the public files: `f/` (the form page shell, `/f/<slug>`), `forms/` (shared modules: `core.js` logic, `render.js`, `templates.js`, `api.js`, `pow.js`, `forms.css`), `tokens.css` (design tokens, shared with the website), `assets/` (brand, fonts, screens, og-image), and a small `index.html` for the root.
- `admin/` - private admin (builder, settings, share, results), plain ES modules, assembled by `scripts/assemble-admin.sh`.
- `worker/` - Cloudflare Worker: the only way respondents reach the database. `supabase/` - migrations and SQL tests. `tests/` - `node:test` suites. `scripts/` - dev server, in-memory Supabase fake, seed, site check, admin assemble and deploy.

## Commands

```bash
node scripts/dev-server.mjs                    # forms + admin (/admin/, any email and password) + Worker + fake Supabase
node scripts/seed-dev.mjs                      # publish the templates into the running dev server
python3 scripts/check-site.py                  # links resolve, nothing loads from third parties
node --test tests/*.test.mjs                   # core, proof-of-work, Worker
bash scripts/assemble-admin.sh                 # admin bundle
bash supabase/tests/run.sh                     # migration + RLS + anonymity tests (needs PostgreSQL binaries)
```

CI (`.github/workflows/ci.yml`) runs all of the checks above. `.github/workflows/claude.yml` is the Claude Code action (write access, triggered by `@claude`); it needs the `CLAUDE_CODE_OAUTH_TOKEN` repository secret.

## Principles

- `docs/forms/core.js` is shared by the browser and the Worker, so keep it pure (no DOM, no network). Anonymous tables must never gain an identifying or fine-grained column; `supabase/tests/forms.test.sql` asserts the column lists.
- No third-party scripts, fonts or embeds anywhere, including the admin; CSP is `'self'`-only, so no inline scripts or style attributes (set styles through CSSOM).
- Never commit secrets: the Supabase service key and `CHALLENGE_SECRET` live only in Worker secrets. The anon key in `admin/config.js` is public by design.

## Wording rules (all CalmCampus copy)

- Never claim pilots, results, partnerships or detection. No "early signs", no "stop burnout", no claim that CalmCampus finds burnout or gets students to seek help. Wellness wording only ("supports").
- Full disclaimer: we are students, not medical professionals; CalmCampus does not diagnose or treat; we strongly suggest a doctor or student support.
- The check-in takes less than a minute. Data stays on the phone. Where a form stores data on a server, it says so.
- No em dashes in user-facing copy. Names: Florian Braùn (with the ù) and Hélène David.

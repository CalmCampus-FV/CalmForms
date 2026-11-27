# CalmForms

[![CI](https://github.com/CalmCampus-FV/CalmForms/actions/workflows/ci.yml/badge.svg)](https://github.com/CalmCampus-FV/CalmForms/actions/workflows/ci.yml)
[![License: MIT](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)

The forms platform of **CalmCampus**: a form builder and public form pages in the CalmCampus design system, with an anonymous mode that the system enforces rather than just promises. No trackers, no third-party scripts. Moved here from [CalmWeb](https://github.com/CalmCampus-FV/CalmWeb) with its history.

> We are students, not medical professionals. CalmCampus does not diagnose or treat. If you need help, we strongly suggest a doctor or your student support service. If you are in crisis, contact your local emergency number or a crisis line.

## What's here

| Path | What it is |
|---|---|
| [`docs/`](docs/) | Public form files: `f/` (form page), `forms/` (shared logic, renderer, templates), `tokens.css`, `assets/` |
| [`admin/`](admin/) | The private admin: builder, settings, sharing, results |
| [`worker/`](worker/) | Cloudflare Worker that receives submissions, so respondents never talk to the database directly |
| [`supabase/`](supabase/) | Database schema, row-level security and its tests |
| [`tests/`](tests/), [`scripts/`](scripts/) | Unit tests, dev server, site check, admin assemble and deploy |

Architecture, privacy model and going live: **[FORMS.md](FORMS.md)**. Rules for contributors and agents: [AGENTS.md](AGENTS.md).

## Where it runs

- Forms: `https://form.calmcampus.app/f/<slug>` (this repo's Pages site plus the Worker). Old links on `https://cc.braunf.com/f/<slug>` keep working (CalmWeb's copies plus the same Worker).
- Admin: `https://admin.calmcampus.app`, with `https://ccadmin.braunf.com` as a fallback (both on the `calmweb-admin` Worker).

## Team

Florian Braùn and Hélène David.

## License

[MIT](LICENSE) © 2026 Florian Braùn, Hélène David.

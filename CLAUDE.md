# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Git rules (strict)

**Only the user commits code.** Never run `git commit`, `git push`, `git merge`, `git rebase`, `git tag`, or open/merge pull requests — not even when a task feels "done". Make the file changes, then stop and let the user review and commit. Read-only git commands (`status`, `diff`, `log`, `show`, `branch`) are fine. Don't stage changes either (`git add`) unless asked.

## What this is

Marketing site for **The 1 Stop Solution** (Lucknow-based agency: websites, apps, SEO, ads). It is a single static page — no build step, no package manager, no tests, no linter.

- `index.html` — the entire site: inline `<style>`, markup, and one inline `<script>` at the bottom. No external JS; only Google Fonts (Plus Jakarta Sans) is loaded externally.
- `CNAME` — custom domain for GitHub Pages (`the1stopsolution.in`).
- `netlify.toml` — config for the dev site only (see Deployment).

To preview locally, open `index.html` in a browser or serve the folder, e.g. `python3 -m http.server 8000`.

## Deployment

- **Prod**: `main` branch → GitHub Pages → https://the1stopsolution.in. GitHub Pages ignores `netlify.toml`.
- **Dev**: `develop` branch → Netlify → dev.the1stopsolution.in. `netlify.toml` publishes the repo root and sends `X-Robots-Tag: noindex, nofollow` so the dev site doesn't compete with prod in search. Keep that header.

## Page structure and behaviour

Sections in `<main>` are anchored by id and linked from both the desktop nav and the `#mobile-menu`: `#services`, `#pricing`, `#how`, `#team`, `#faq`, `#contact`. When adding/renaming a section, update both navs.

Things that span multiple places and must stay in sync:

- **Contact details** (phone, email, address) appear in the JSON-LD `<script type="application/ld+json">` in `<head>`, in visible markup (contact section/footer), and in the JS constants `WHATSAPP_NUMBER` / `EMAIL` at the top of the inline script. Change all of them together.
- **Pricing → contact form**: buttons with `data-plan="..."` pre-select the `#f-plan` `<select>`. A `data-plan` value must match an `<option>` value in `#f-plan`. The JSON-LD `priceRange` should reflect the lowest plan price.
- **Lead form (`#lead-form`)** has no backend. On submit it builds a plain-text message from the fields and opens either WhatsApp (`wa.me`) or a `mailto:` link, chosen by the submit button's `data-via` attribute (`whatsapp` default, or `email`). Field `name` attributes (`fullname`, `phone`, `business`, `need`, `plan`, `message`) are read directly by the script, so renaming them requires updating the handler. Only name and phone are required (custom validity messages).
- **Legal dialog (`<dialog id="legal">`)**: Privacy / Terms / Refund / Disclaimer live as `.legal-tab` sections with ids `legal-<tab>`. Footer links call `openLegal('<tab>')`; tab buttons use `data-tab="<tab>"`. A new tab needs a matching section id and button.

## Styling conventions

Colors and shared values are CSS custom properties on `:root` (`--navy`, `--blue`, `--cyan`, `--ink`, `--text`, `--muted`, `--line`, `--bg`, `--bg-2`, …). Use these variables rather than hardcoding colors. SEO/meta tags (`description`, canonical, Open Graph, `theme-color`) live in `<head>` and should be updated if the site's positioning or copy changes significantly.

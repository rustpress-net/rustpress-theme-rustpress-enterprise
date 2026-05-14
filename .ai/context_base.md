# rustpress-theme-rustpress-enterprise — AI Context

> **Purpose**: Orient an AI agent to this repo without reading the whole tree. Pair with the RustPress organisation context in `rustpress-core-base/.ai/context/CONTEXT_BASE.md`.

## Project

`rustpress-theme-rustpress-enterprise` is the **flagship official theme** for RustPress CMS — a dark, animated, rust-orange landing-page theme designed to showcase what a modern CMS can look like. It's the only theme of the three in the org that's actually complete (the other two — `rustpress-theme-developer-developer` and `rustpress-theme-business-elite` — are stubs and won't ship at v1.0).

Theme name (per `theme.json`): **RustPress Enterprise**, v2.0.0. Engine: **Tera** (Rust-native templating, similar to Jinja2). The theme is feature-rich enough that the audit calls it production-ready with only minor README polish to do.

> Note: this theme is **not** a WordPress-lookalike. It's a modern dark-themed marketing-page system. The Plan-D narrative around "WP-lookalike themes" is being softened to "polished default theme + community theme SDK" — see `AUDIT.md`.

## Tech stack

- **Template engine**: Tera (HTML-based — no other directive system used)
- **Styling**: Custom modular CSS (NOT Tailwind) — 8 files by concern
- **Scripting**: Vanilla JS modules — 8 files, no framework
- **Manifest**: `theme.json` v2.0.0, declares templates, partials, assets, customizable colors/fonts
- **Engine requirement**: `rustpress >= 1.0.0`
- **License**: MIT
- **Build / packaging**: none required — theme ships as-is and is packaged via `rustpress-net/rustpress-core-devops/actions/build-theme@main`

## Directory layout

```
rustpress-theme-rustpress-enterprise/
├── theme.json              # manifest: templates, partials, assets, colors, fonts
├── README.md               # basic install + feature list (minimalist, accurate)
├── CONTRIBUTING.md
├── LICENSE                 # MIT
├── production-release.yml  # release config used by CI
├── templates/
│   ├── base.html           # layout shell extended by every page
│   ├── index.html / home.html / front-page.html
│   ├── page.html / single.html
│   ├── archive.html / category.html / tag.html / author.html / search.html
│   ├── 404.html
│   ├── about.html / contact.html / docs.html / features.html
│   ├── pricing.html / plugins.html / themes.html / roadmap.html
│   ├── ide.html / admin-preview.html
│   ├── checkout.html / thank-you.html / download.html
│   └── partials/
│       ├── header.html / footer.html
│       ├── sidebar.html
│       └── mobile-menu.html
└── assets/
    ├── css/                # 8 files: variables, base, typography, layout,
    │                       #          components, animations, sections, responsive
    ├── js/                 # 8 files: animations, navigation, particles,
    │                       #          counters, forms, gallery, stripe, main
    └── images/
```

## Public API / what this repo exposes

The theme's "API" is the set of templates RustPress can render against. From `theme.json`:

**Templates** (page-level):
`home`, `page`, `post`, `blog`, `archive`, `contact`, `404` (and additional in-tree variants: index, single, search, category, tag, author, front-page, about, docs, features, pricing, plugins, themes, roadmap, ide, admin-preview, checkout, thank-you, download).

**Partials** (reusable fragments):
`header`, `footer`, `sidebar`, `mobile-menu`, `hero`, `features`, `ai-showcase`, `ecosystem`, `comparison`, `gallery`, `donations`, `contact-form`, `stats`.

**Customizable** (declared in `theme.json`):
- Colors: `primary` (#CE422B rust orange), `secondary` (#B87333), `accent` (#E97451), `background` (#0D0D0D dark), `surface`, `text`, `text-muted`
- Fonts: heading (Space Grotesk), body (Inter), code (JetBrains Mono)

Plus 8 JS modules (`animations.js`, `navigation.js`, `particles.js`, `counters.js`, `forms.js`, `gallery.js`, `stripe.js`, `main.js`) wired by the base template.

## How to build / test

```bash
# No build step. Smoke test locally:
# 1. Drop this directory into a running RustPress install's `themes/` folder.
# 2. Activate via the admin UI (rustpress-core-admin-ui) or CLI:
#       rustpress theme activate rustpress-enterprise
# 3. Visit /, /about, /docs, etc.

# Package for release (CI workflow uses this):
#   rustpress-net/rustpress-core-devops/actions/build-theme@main
```

CI: `rustpress-net/rustpress-core-devops/actions/build-theme@main` (theme packager).

## Cross-repo dependencies

- **Depends on**: `rustpress-core-base` ≥1.0 (its theme manager, Tera engine, asset pipeline)
- **Depended on by**: nothing at build time. End users install it into a running RustPress site.
- **Companion**: `rustpress-ai-prompts` `theme/` category — the AI prompts reference theme conventions that this theme exemplifies.

## Conventions

- **License**: MIT (LICENSE file at repo root + declared in `theme.json` and CSS headers)
- **Commits**: Conventional Commits — recent example: `Trigger deployment - updated contact text`
- **Templating**: Tera only — no inline `<script>` blocks (use `assets/js/`)
- **CSS**: modular — never add `<style>` tags inside templates; pick the right concern file (`variables.css` for tokens, `components.css` for reusable components, `sections.css` for page sections, `animations.css` for keyframes, `responsive.css` for breakpoints)
- **CSS variables**: centralised in `assets/css/variables.css`; everything else references them
- **Naming**: BEM-ish (block__element--modifier) where applicable; no Tailwind utility chains
- **Assets**: compiled / minified outputs are committed (legitimate for a theme that ships as-is)

## Status

- Release readiness: **🟢 READY** for v1.0 (see `AUDIT-themes.md` Theme 1 and master `AUDIT.md`)
- Verdict: "polished, feature-rich flagship theme with proper structure, comprehensive template coverage, and modular styling ready for GA release."
- Manifest: theme.json v2.0.0; first MAJOR release matches v1.0 of the CMS.
- This is the **only theme** of the three in the org that ships at v1.0 — the other two are stubs to be archived or removed.

## Known issues / TODOs

From `AUDIT-themes.md` (Theme 1 section):

- **P0**: None.
- **P1**: Add screenshot to README for visual preview (theme is animated; a static screenshot is the lowest-effort marketing win).
- **P1**: Expand README install section with concrete theme-directory path examples per host (Docker, systemd, manual).
- **P1**: Document the customizer (colors / fonts in `theme.json`) in README — today it's only discoverable by reading `theme.json` directly.
- **P1**: One `home.html` image references an Unsplash URL — context-appropriate for a marketing template but should be swapped for a self-hosted asset in production deployments. Document this in README.

## When working in this repo

- The theme **renders the public face of RustPress**. Visual changes are launch-impacting — get design eyes on them before merging.
- Add CSS to the correct concern file. Don't create new top-level CSS files unless absolutely necessary — the 8 are the contract.
- New templates need to be registered in `theme.json#templates`; new partials in `theme.json#partials`. Forgotten registrations result in 404s at runtime.
- Tera syntax pitfalls: `{% if x %}` vs `{{ x }}`, no Python-style filters chained with `|` mid-expression without space. When in doubt, check existing templates.
- Keep the theme **self-contained**. Don't depend on plugins for rendering the public site (a fresh install without `rustcommerce` or `visual-queue` should still render `home.html` cleanly). Plugin-aware partials should degrade gracefully.
- Customizer values (`theme.json#customizable`) are user-editable at runtime. Don't hardcode the defaults into CSS — always read from `var(--primary)` etc.
- Performance budget: home page should stay under 200KB JS + 80KB CSS gzipped. The particles/gallery scripts are easy to bloat — audit before adding features.

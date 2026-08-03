# Handoff: CANShift documentation site restyle

## Overview

A full restyle of `docs.canshift.tmbk.ch` — the Starlight (Astro) documentation site for CANShift —
onto the Modernist design language already used by the Tuner web app, the dash firmware UI and the
mobile app. The restyle covers site chrome (header, primary nav, sidebar, table of contents), the
article page, dense reference tables, code presentation, the search palette, the 404, a contact page,
a hardware/BOM page with regional pricing, and a set of per-repository icons.

It also introduces a **real two-level navigation**: a primary nav bar with a dropdown per section, and
a numbered sidebar whose groups collapse and whose pages can carry sub-pages, filtered by audience
(DRIVER / TUNER / DEV).

## About the design files

The files in this bundle are **design references written in HTML** — prototypes that show the intended
look, geometry and behaviour. They are not production code to copy. The work is to reproduce them in
the real site's own environment: Astro + Starlight, styled through `src/styles/canshift.css` and a
handful of Starlight component overrides. `canshift-docs-theme/` is the closest thing to shippable
code in this bundle: it is real CSS and real `.astro` components, written against Starlight 0.41.x,
and is meant to be moved into the repo and then verified selector by selector against the installed
version.

## Fidelity

**High fidelity.** Colours, type sizes, weights, letter-spacing, rule weights and layout geometry are
final and should be reproduced exactly. Copy in the mockup is real editorial copy for the four pages
that are written; every other page is deliberately marked "NOT WRITTEN YET" (see *Content state*).

## Files in this bundle

| File | What it is |
| --- | --- |
| `CANShift Docs.dc.html` | the mockup — nine screens: home, doc page, dense reference, code/architecture, ⌘K search, 404, contact, hardware & BOM, plus the mockup screen-switcher strip at the very top (dev chrome, **not** part of the site) |
| `CANShift Repo Icons.dc.html` | the five repo marks, every variant, on light / dark / accent grounds |
| `canshift-docs-theme/` | the Starlight theme: `canshift.css`, `SiteTitle.astro`, `PageTitle.astro`, `Pinout.astro`, `Glance.astro`, config snippets, and its own README with the file→destination mapping |
| `canshift-repo-icons/` | 45 SVGs — five repos × variants, including `-onaccent` versions for red grounds |
| `NAV_MODEL.json` | the full navigation tree (5 groups, 41 pages), machine-readable |
| `docs-content.js` | the drafted content model for the pages that are not yet written |
| `ISSUES.md` | the work broken into issues, in dependency order, with acceptance criteria |
| `CLAUDE_CODE_PROMPT.md` | the prompt to paste into Claude Code to open those issues and then work through them |
| `support.js` | runtime needed only to open the `.dc.html` mockups in a browser |

To view a mockup: open the `.dc.html` file directly in a browser (`support.js` must sit next to it).

## Design tokens

Two themes ship. The accent changes between them on purpose: `#ec3013` fails contrast on the dark
ground, so the dark theme uses the device red `#ff4747`.

### Light

| Token | Value | Use |
| --- | --- | --- |
| `--cs-accent` | `#ec3013` | primary action, active nav, small emphasis |
| `--cs-accent-soft` | `#fff2ef` | hover tint, DANGER callout ground |
| `--cs-bg` | `#f3f2f2` | page ground |
| `--cs-panel` | `#eae9e9` | hover ground, TIP callout |
| `--cs-sunk` | `#e2e0e0` | inline `code` ground |
| `--cs-ink` | `#201e1d` | body text, 2 px rules |
| `--cs-muted` | `#605d5d` | secondary text, nav idle |
| `--cs-faint` | `#7d7979` | mono labels, ToC idle |
| `--cs-line` | `rgba(32,30,29,0.16)` | 1 px separators |
| `--cs-rule` | `#201e1d` | 2 px separators |

### Dark

| Token | Value |
| --- | --- |
| `--cs-accent` | `#ff4747` |
| `--cs-accent-soft` | `rgba(255,71,71,0.10)` |
| `--cs-bg` | `#121212` |
| `--cs-panel` | `#1a1a19` |
| `--cs-sunk` | `#202020` |
| `--cs-ink` | `#f3f2f2` |
| `--cs-muted` | `#a3a09e` |
| `--cs-faint` | `#7d7979` |
| `--cs-line` | `rgba(243,242,242,0.15)` |
| `--cs-rule` | `#f3f2f2` |

### Code blocks — the same in both themes

`--cs-code-bg #0d0d0d`, `--cs-code-ink #e8e6e3`, `--cs-code-dim #6f6b68` (comments), keywords/types
`#ff8f7a`, identifiers `#ffc4b8`, language tag `#ff4747`, filename `#8f8b88`, header rule
`rgba(255,255,255,0.12)`. In the light theme the code block is the only black field on the page — that
is intentional and load-bearing for the page rhythm. Do not lighten it.

### Type

- Headings and UI: **Archivo** 400 / 500 / 600 / 700 / 800.
- Values, labels, code, numbering: **JetBrains Mono** 400 / 500 / 700.
- Scale: h1 52 px / 800 / -0.035em / line-height 1 · dek 19 px / 1.5 · h2 27 px / 800 / -0.02em ·
  h3 21 px · body 16.5 px / 1.78 · sidebar item 14 px · sub-page 13.5 px · nav bar 12.5 px / 700 /
  0.04em · mono labels 10–11 px / 700 / 0.12–0.16em uppercase · inline code 14 px · code block
  13.5 px / 1.7.
- Every numeric readout is mono and `font-variant-numeric: tabular-nums`.
- Radius is **0** everywhere. Starlight sets many radii by default — override globally.

## Layout geometry

| Element | Size |
| --- | --- |
| Header | 64 px, sticky top 0, 1 px bottom line |
| Primary nav bar | 46 px, sticky top 64, 2 px bottom rule |
| Sidebar | 286 px, 1 px right line, sticky top 110, `max-height: calc(100vh - 110px)`, own scroll |
| Article | max 860 px measure, padding 64 px 72 px 128 px |
| Table of contents | 248 px, sticky top 110, 1 px left line, padding 64 px 30 px |
| Search dialog | 640 px wide, 1 px line, 2 px accent top border |

Breakpoints (already implemented in the mockup, keep them): ToC hides at 1180 px, the trailing nav-bar
items (HARDWARE & BOM, CONTACT) hide at 1240 px, the header search collapses to an icon at 1080 px,
header badges and link labels collapse before that, sidebar hides at 900 px and the nav bar wraps to
two rows and stops being sticky.

## Navigation (the core of this restyle)

Source of truth: `NAV_MODEL.json`. 5 groups, 41 pages, two levels below the group.

### Primary nav bar

`HOME` · `GUIDE 8` · `FIRMWARE 7` · `REFERENCE 9` · `TUNER & APP 4` · `CONTRIBUTING 5` · spacer ·
`HARDWARE & BOM` · `CONTACT`

- Labels 12.5 px / 700 / 0.04em, `--cs-muted`, 1 px separator between items, page count in mono 10 px
  at 55 % opacity. No icons in the nav bar.
- The current section carries `box-shadow: inset 0 -3px 0 var(--cs-accent)` and ink-coloured text.
  Exactly one item is ever marked current: the section marks only on documentation screens, so HOME
  stands alone on the home screen and neither marks on contact/404.
- Hovering a section opens its dropdown (level 2): a panel anchored under the item, min-width 330 px,
  1 px line, **2 px accent top border**, listing every page of the group as `num` + label, plus `+3`
  when that page has sub-pages. The two right-most sections anchor their panel to the right edge so it
  cannot overflow the viewport.
- Clicking a section goes to its first page; clicking a panel row goes to that page, expands its group
  in the sidebar, and switches the audience tab if the group is not visible in the current one.

### Sidebar

1. **Audience tabs** — DRIVER / TUNER / DEV, three equal cells, 11 px / 700 / 0.08em. The active tab
   is filled ink-on-ground. They genuinely filter: DRIVER sees groups 1 and 4, TUNER sees 1, 3 and 4,
   DEV sees 1, 2, 3 and 5.
2. **Count row** — `41 PAGES · DEV` in mono 10.5 px, with an `EXPAND ALL` / `COLLAPSE ALL` button
   (1 px line, mono).
3. **Groups** — a button per group: repo mark (15 px, two paths, the second in accent), `1 — USER
   GUIDE` in mono 11 px / 0.14em, page count, and `+` / `−`. Clicking toggles the group.
4. **Pages** — `1.1` in mono 12 px at 60 % opacity + label at 14 px, padding 10 px 20 px 10 px 22 px.
   A page with sub-pages shows its own `+` / `−` and expands them in place.
5. **Sub-pages** — indented 34 px behind a 1 px left line, 13.5 px, `--cs-faint`.
6. **Footer** — `Tuner (web) ↗` and `GitHub organisation ↗` above a 1 px rule.

Active state: accent fill, white text, weight 600. Hover: `--cs-accent-soft`. Keyboard focus:
`outline: 2px solid var(--cs-accent); outline-offset: 2px` — never the browser default.

Numbering (`1`, `1.1`, `1.4.2`) is generated with CSS counters in `canshift.css`, not written into the
Starlight config, so renaming or reordering a group cannot desynchronise it.

## Article page

- **Breadcrumb** — mono 11 px / 0.1em: group, `/`, page in accent, uppercase.
- **Title** 52 px, **dek** 19 px `--cs-muted`, max 60ch.
- **Badge row** — `FW ≥ 1.6.0` (accent fill, white), `WIRE v2` and `UPD 23.07.2026` (1 px outline),
  mono 11 px, above a 2 px rule with 46 px of air under it. Fed by frontmatter `fw:` / `wire:` /
  `source:` (see `content.config.snippet.ts`).
- **H2** 27 px / 800 with a 1 px top line and 56 px of space above — the rule does the sectioning.
- **Callouts** — no icons. 1 px box, 3 px left bar, mono label above the text.
  DANGER: accent bar + `--cs-accent-soft` ground, label in accent. NOTE: ink bar, page ground.
  TIP: ink bar, `--cs-panel` ground, accent label.
- **Code block** — 1 px box; header 9 px 14 px on `#0d0d0d` with the file path in mono 11.5 px
  `#8f8b88`, the language in mono 10 px accent, separated by a 1 px white-12 % line; body padding
  18 px 16 px, 13.5 px / 1.7.
- **Inline code** — mono 14 px, `--cs-sunk` ground, 1 px line, padding 2 px 5 px, no radius.
- **Prev / next** — two equal cells in a 1 px box separated by a 1 px gap; mono kicker
  `← PREVIOUS` / `NEXT →` over the page title at 15 px / 700. They walk the flattened nav tree, so
  they always agree with the sidebar order (including sub-pages).
- **ToC** — `ON THIS PAGE` in mono 11 px over a 1 px line, then heading links at 13.5 px with a 2 px
  left border (accent when current, line otherwise), and `Edit this page ↗` / `Report an issue ↗`
  under a 1 px rule.

## Other screens

- **Home** — split hero: left, a mono kicker with a dashed bus rule, a 3-line 800-weight headline and
  the primary actions (accent fill / 1 px outline); right, a `HARDWARE TARGET` panel with a 2×2 stat
  grid (mono labels above values) on `--cs-panel`, plus the Tuner capture. Below: a 3-up card grid
  (1 px gaps, 372 px min height, mono kicker, 30 px title, accent call to action), a stat band, and a
  red poster block for the "write it down" close — the only place red runs as a field.
- **Dense reference** — full-width table, mono 11 px uppercase header on `--cs-panel` over a 2 px
  rule, 1 px row lines, mono values, accent for identifiers, row hover `--cs-panel`. No zebra.
- **Architecture / code** — long code blocks plus ASCII trees in mono; the same black field.
- **Search (⌘K)** — 640 px dialog, 2 px accent top border, mono `⌘K` hint, grouped results with the
  group name in mono over each cluster, first result pre-selected in accent, footer with
  `↑↓ navigate · ↵ open · esc close`. Opens on ⌘K/Ctrl-K and from the header search.
- **404** — mono `404` kicker, 52 px headline, three suggested destinations as outlined rows, and the
  search entry point.
- **Contact** — four fields (type, email, message, send) at a 880 px measure, with an optional
  `DETAILS` section that only appears for bug reports (component, firmware version, reproduction).
  Segmented controls, no radii, accent submit.
- **Hardware & BOM** — region selector US / EU / UK / CH; every row re-prices in local currency and
  re-points at a regional supplier; core parts and optional extras tables with per-region totals and a
  note per region. Data lives in the mockup's `BOM` table and should move to a data file.
- **Repo icons** — five marks (firmware bits, core frame fields, tuner selection handles, mobile RSSI
  bars, docs blueprint cartouche), drawn on a 116 × 100 viewBox with 15-unit strokes, two paths each,
  the second in the accent. `currentColor` for the ink path so they inherit. `-onaccent` variants for
  red grounds. Used at three points: sidebar group headers, the header DOCS tag, and repo links.

## Interactions & behaviour

| Interaction | Behaviour |
| --- | --- |
| Nav bar hover | dropdown opens immediately, closes on mouse leave; no delay, no animation |
| Section click | first page of the section, group expanded, audience switched if needed |
| Sidebar group click | collapse / expand, state kept per group |
| Sidebar page with sub-pages | navigates **and** toggles its children |
| Audience tab | filters groups; navigating into a hidden group switches the tab automatically |
| EXPAND / COLLAPSE ALL | applies to every group, label reflects the current state |
| ⌘K / Ctrl-K | opens the search dialog from anywhere; Esc closes |
| Theme toggle | header button, persisted, `data-theme` on `<html>` (Starlight already does this) |
| Prev / next | previous / next node in the flattened nav tree |

State needed: current page id, current audience, per-group expanded flag, per-page expanded flag, open
dropdown id, theme, search-dialog open. In Starlight most of this is URL-driven; only the audience
filter, the group/page expansion and the dropdown need client state.

## Content state — read this before opening issues

Only **four** pages have written bodies in the mockup: `1.6 Cruise control` (the reference for tone,
callouts and code blocks), `3.1 Hardware & BOM`, `3.2 Pinout — CrowPanel 2.8″`, `2.2 Boot sequence`.

Every other page in `NAV_MODEL.json` has a final title, number, position and one-line dek, and renders
a deliberate **NOT WRITTEN YET** block: mono badge, one paragraph explaining the page is outlined but
not drafted, a link to 1.6 as the reference, and a "write it on GitHub" link. Its ToC shows
`Outline only — no headings yet`.

Do not invent prose for those pages. Either keep the placeholder in the shipped site, or draft the
page from `docs-content.js` in a separate content issue — never as part of a styling issue.

## Starlight integration notes

- `canshift-docs-theme/README.md` holds the file → destination table and the config snippets.
- Written against Starlight **0.41.x**. Every selector that reaches into Starlight internals must be
  re-verified against the installed version before the CSS is trusted; the risky ones are the sidebar
  tree, the ToC, `expressive-code` and the mobile nav.
- Starlight's own sidebar does not do audience filtering or two-level collapse — that needs a component
  override, not CSS.
- `editLink.baseUrl` → `https://github.com/CANShift/canshift-docs/edit/main/`; "open an issue" links
  target the repo the page is about (firmware for architecture pages, tuner for configuration pages),
  never the org.
- The home page shows `npm i @canshift/core`, not a `git clone`.

## Assets

- Repo icons: `canshift-repo-icons/` (45 SVGs, `README.md` inside lists the variants).
- Logo, favicon, app icons, social image: already delivered in `design_handoff_canshift/` — reuse those,
  do not redraw.
- `shots/tuner-wide.png` (the Tuner capture in the hero) lives in the design project; re-export from the
  real Tuner at 2× rather than shipping the mockup crop.
- Fonts: Archivo and JetBrains Mono, Google Fonts. Self-host if the site has a font budget.

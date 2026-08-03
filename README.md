# CANShift brand

Brand assets and design references for the [CANShift organization](https://github.com/CANShift).

- `logo/brand/` — monogram, lockups, favicon set ("Bracket" identity)
- `icons/<repo>/` — the five repo marks, every variant (currentcolor, onaccent, dark, small, avatar, app-icon, maskable)
- `docs/design/restyle-handoff.md` — the visual grammar every surface follows
- `docs/design/docs-restyle-handoff.md` — the docs-site restyle spec (tokens, type, navigation, every screen)
- `docs/design/tuner-flows.md` — the tuner UX flows reference
- `docs/design/*.dc.html` — brand book and surface mockups (open next to `support.js`)

The firmware dash is the canonical rendering of the identity; the tuner preview mirrors it.
Vector paths used at runtime live in [`@canshift/core`](https://github.com/CANShift/canshift-core) (`src/brand.ts`) — update both when the identity changes.

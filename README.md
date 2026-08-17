# CANShift brand

Brand assets and design references for the [CANShift organization](https://github.com/CANShift).

- `logo/brand/` — monogram, lockups, favicon set ("Bracket" identity)
- `logo/icons/<repo>/` — the repo marks, every variant (currentcolor, onaccent, dark, small, avatar)
- `design/*.dc.html` — brand book and surface mockups (open next to `support.js`)
- `design/PROMPT_*.md` — the correction prompts that go with the mockups

The firmware dash is the canonical rendering of the identity; the tuner preview mirrors it.
Vector paths used at runtime live in [`@canshift/core`](https://github.com/CANShift/canshift-core) (`src/brand.ts`) — update both when the identity changes.

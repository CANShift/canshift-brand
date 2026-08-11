# Prompt — CANShift mobile app: the correct design

Paste this into Claude Code in the `canshift-mobile` repo. The reference mockup is `CANShift Mobile.dc.html`
(nine screens at 390 × 844); the brand rules come from `CANShift Brand Book.dc.html` §05.

---

You are correcting the CANShift mobile app (iOS + Android) to match its design spec. The current
implementation drifted. Below is the complete, binding spec — where the code differs, the code is wrong.
Audit the app screen by screen against it, list every deviation, then fix them.

## The one-line brief

The phone app is the dash's grammar on a phone: black ground, mono numerals, hairline rules, one red
kept for danger and the active state. Nothing is rounded, nothing is decorated, no shadows, no gradients,
no icons in the chrome.

## Tokens

| Token | Value | Use |
| --- | --- | --- |
| Ground | `#121212` | every screen, edge to edge |
| Ink | `#FFFFFF` | primary text and values |
| Muted | `#BABABA` | secondary text, kickers, units |
| Faint | `#8F8F8F` | tertiary, inactive tabs, list chevrons |
| Rule strong | `#333333` (2 px) | under the header, above the tab bar |
| Rule hair | `#262323` (1 px) | between list rows and graph blocks |
| Track | `#222222` | progress-bar and gauge tracks |
| Accent | `#FF4747` | active tab, selected row, engaged/primary state, CAN IDs |
| Alarm ground | `#FF4444` | the critical-alert takeover only |
| OK | `#00CC2A` | bus ACTIVE status only |

Radius **0** everywhere. No other colors. `#FF4747` and `#FF4444` are not interchangeable: 47 is the
interface accent, 44 is the alarm field.

## Type

- **Archivo 800** for everything that speaks: screen titles (22 px, -0.02em), buttons and tab labels
  (11–15 px, letter-spacing 0.08–0.1em, UPPERCASE), section kickers (10 px, 0.18–0.2em, UPPERCASE).
- **Monospace** (JetBrains Mono or the platform mono) for everything that measures: all numerals, units,
  device names, versions, timestamps, CAN frames, body copy. The app's default font IS the mono — Archivo
  is the exception, reserved for headings/labels/buttons.
- Big values: graph readouts 34 px, OTA percent 96 px, alert value 132 px, all mono, line-height ≤ 1.
- Units set smaller (13–14 px) in muted, inline after the value: `1.42 bar`.

## Chrome (every screen)

- Status row 54 px: time · "CANShift" · battery, mono 13 px muted. No logo anywhere in-app after Connect.
- Header: title Archivo 800 22 px, baseline-aligned meta on the right (mono 12 px faint), 2 px `#333333`
  rule below. On connected screens the header carries the device name and live bus state
  (`CAN 842 Hz · MAP 1`) — never a logo.
- Tab bar 84 px, 2 px `#333333` rule above, four tabs only: DASH · GRAPH · CONSOLE · DEVICE, Archivo 800
  11 px 0.1em, active in `#FF4747`, inactive `#8F8F8F`. Nothing else at root level.
- Screen padding: 22 px horizontal, list rows 16–18 px vertical with 1 px `#262323` separators.

## Components

- **Primary action**: 64 px bar, `#FF4747` fill, white Archivo 800 15 px 0.09em label, **flush left**
  (padding-left 20 px, never centered). One per screen at most, at the bottom of the content.
- **Secondary action**: 56 px, 2 px `#FFFFFF` outline, transparent, flush-left label. Destructive
  variants (DISCONNECT) outline + text in `#FF4747`.
- **Selected list row**: `inset 3px 0 0 #FF4747` left bar + `#1A1717` ground. Never a full red fill.
- **Segmented tabs** (Console CAN/LOG/SEND): active = `inset 0 -3px 0 #FF4747` underline, no fill.
- **Choice cards** (theme NIGHT/DAY/RALLY): 68 px, 2 px outline — `#FF4747` when active, `#333333` idle.
- **Progress**: 12 px bar, `#222222` track, `#FF4747` fill, square ends.
- **Graphs**: 2 px white polylines on the bare ground, no axes, no grid, no fills; the signal in alarm
  draws in `#FF4444`. Kicker above, big mono value below.
- **Console**: CAN IDs in `#FF4747`, payload bytes muted mono 12 px, line-height 2.1.
- **Callout notes**: 2 px left border (`#FF4747` for the important one, `#333333` for the rest),
  12 px left padding, mono 14 px muted.

## The nine screens

01 Connect (logo + SCAN, the only screen with the mark) · 02 Devices (scan list, paired device selected)
· 03 Dashboard (mirrors the dash's 12-column grid so panel and phone read as one product; SIM badge
outlined when simulating) · 04 Pages (numbered dash pages, active row selected) · 05 Graph (stacked
signal strips, last 30 s) · 06 Console (CAN/LOG/SEND, PAUSE + FILTER ID) · 07 Critical alert ·
08 Device (info table, theme cards, settings rows, red DISCONNECT, fw update row shows `0.1.1 ›` in red)
· 09 OTA update (percent, `0.1.0 → 0.1.1`, progress, BLE notes, changelog, CANCEL UPDATE).

## Critical alert (07) — full takeover

Whole screen `#FF4444`, pulsing 0.9 s ease-in-out (100 % → 35 % opacity); signal name Archivo 800 26 px
0.16em; value mono 132 px; threshold line under it; context table (RPM / OIL TEMP / SINCE) over a 2 px
white-50 % rule; "STOP THE ENGINE" pinned above the actions; ACKNOWLEDGE = white fill with `#FF4444`
text, MUTE FOR THIS SESSION = white-70 % outline. No tab bar while the alert holds.

## Behaviour rules

- Values snap, never tween. Bars catch up in 120 ms linear.
- A signal stale for 500 ms goes grey and reads `- -`.
- Alerts are read-only mirrors of the dash's state — acknowledging on the phone acknowledges on the dash.
- Everything flush left, including button labels. No centered copy except tab labels.

## What to do

1. Audit every screen against this spec; produce the deviation list first (screen, element, expected,
   actual).
2. Fix in this order: tokens/fonts → chrome (status row, header, tab bar) → components → per-screen
   layouts → alert takeover → motion rules.
3. Do not introduce icons, shadows, radii, gradients, or a third red. When the platform fights the spec
   (safe areas, system fonts), keep the geometry and say what you adapted.

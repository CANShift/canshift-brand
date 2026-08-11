# Prompt — CANShift dash (firmware UI): the correct design

Paste this into Claude Code in the `canshift-firmware` repo. The reference is `CANShift Brand Book.dc.html`
§04 (dash, 320 × 240 CrowPanel 2.8″, LVGL).

---

You are correcting the CANShift dash UI (LVGL on a 320 × 240 CrowPanel 2.8″) to match its design spec.
The current implementation drifted. Below is the complete, binding spec — where the code differs, the
code is wrong. Audit the rendered pages against it, list every deviation, then fix them.

## The one-line brief

The dash is an instrument, not an app: pure black ground, white mono numerals, red only for danger and
engaged states, no icons, no decoration, no animation except the ones listed. Everything is flat and
square.

## Tokens (device)

| Token | Value | Use |
| --- | --- | --- |
| Ground | `#121212` | the panel, edge to edge |
| Ink | `#FFFFFF` | values, primary rules, outlines |
| Muted | `#BABABA` | kickers, units |
| Track | `#222222` | bar tracks, secondary 1 px rules |
| Accent / engaged | `#FF4747` | engaged buttons, selected state, progress fill |
| Danger | `#FF4444` | danger values, shift-light top end, alarm takeover ground |
| Warning | `#FF8800` | the DO NOT UNPLUG line only |

Radius 0. No greys other than the four above. No gradients, no shadows, no anti-aliased decoration.

## Type

Two faces, converted to LVGL bitmap fonts with `lv_font_conv`:

- **Archivo 800** — labels, kickers, button text. Kickers 10 px at 0.18em tracking UPPERCASE; button
  labels ~28 px value / 10 px kicker. Never below 9 px equivalent.
- **Mono (JetBrains Mono)** — every number the car produces. Primary value 72 px (-0.045em, line-height
  0.92), secondary 34 px, danger 46 px, units 14–15 px in muted inline after the value.

Tabular numerals mandatory — a value must not shift width as digits change.

## Layout grammar

- **12-column grid, 12 px gutter, 22 px outer padding.** Widgets span columns, never fixed pixels.
- Every widget opens with a top rule + kicker: primary values a **2 px white** rule, secondaries a
  **1 px `#222222`** rule, danger a **2 px `#FF4444`** rule with kicker and value in `#FF4444`.
- Kicker sits above the value: label 10 px muted, value huge mono under it, flush left.
- Secondary widgets may carry a 3 px bar gauge under the value: `#222222` track, white fill.
- **Buttons**: outlined 2 px white when off, filled `#FF4747` (border + ground) when engaged; kicker +
  state word (e.g. `MAP 1`, `ARMED`), flush left; touch targets never below 48 × 50 px real.
- **Shift light**: a row of square cells, 3 px gaps — white through the ramp, `#FF4444` at the top end,
  `#222222` unlit.
- Anything that does not fit 320 × 240 is flagged in the Tuner at layout time — the dash never silently
  scales or clips.

## The six default pages

01 Street (speed, gear, rpm, water, fuel, batt, odo) · 02 Track (shift light, rpm, gear, speed, oil
press, four secondaries) · 03 Engine (water, oil temp, oil press, fuel press, gearbox, diff, batt) ·
04 Tuning (boost, target, lambda, throttle, IAT, EGT, knock) · 05 Timing (lap, delta, best, last, laps,
timer + reset buttons) · 06 Controls (launch rpm, clutch, launch, anti-lag, map, pit limit, cruise).

## Motion — exhaustive list, nothing else animates

- Rev limit: hard on-off blink at 6 Hz, no easing.
- Armed states: 1 s ease-in-out pulse, 100 % → 35 % opacity.
- Values **snap** — never tween a numeral. Bar gauges catch up in 120 ms linear.
- Stale signal (500 ms without a frame): value goes grey and renders `- -`.
- Page change is an instant cut. No slide, no fade, nothing for the eye to track.

## System screens

- **Boot**: mark centered on `#121212` — CS monogram (ink white, right half `#FF4747`) over the skewed
  CANSHIFT wordmark — then straight into self-test. No progress bar at boot.
- **Firmware update**: kicker `UPDATING FIRMWARE`, mono percent 56 px, 10 px progress bar (`#222222`
  track / `#FF4747` fill) pinned at the bottom, `DO NOT UNPLUG` in `#FF8800` under it. Then complete or
  failed — no intermediate states.
- **Critical alert**: full takeover on `#FF4444`, pulsing; signal name Archivo 800 tracked wide; value
  mono huge; `STOP THE ENGINE` pinned at the bottom. It preempts every page and holds until
  acknowledged (dash button or phone).

## What to do

1. Audit each of the six pages plus boot/update/alert against this spec; produce the deviation list
   first (page, widget, expected, actual).
2. Fix in this order: fonts (regenerate the LVGL bitmap fonts if sizes are wrong) → tokens → the widget
   grammar (rules, kickers, value sizes) → grid spans → buttons and shift light → motion rules → system
   screens.
3. Respect the LVGL ownership rules in the docs (`2.3 LVGL ownership`, `2.4 Page lifecycle`): widgets
   are rebuilt on page change, never mutated across pages; no `lv_obj_clean` outside page teardown.
4. Do not add easing, icons, radii, greys, or a third red. If a constraint (memory, font size budget)
   forces a deviation, keep the grammar and document what changed and why.

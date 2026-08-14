# Prompt — CANShift states, alerts and control feedback

For Claude Code. This one document covers three repositories: `canshift-firmware` (the dash),
`canshift-mobile` and `canshift-tuner`. Visual reference: `CANShift States and Alerts.dc.html` — open it
in a browser, `support.js` and `_ds/` must sit beside it. The dash's full design system is
`DASH_DESIGN_SYSTEM.md` in the same folder; this document is the states layer on top of it.

Implement the section for the repo you are working in. Audit first, list every deviation, then fix.

---

## 0. The severity grammar — all three products

Four levels. The level is carried by **the rule and the ground**, never by an icon and never by a badge.

| Level | Token | Reads as | Where |
| --- | --- | --- | --- |
| **Information** | none | exactly like a healthy value — no colour, no badge. If it needs a badge it is not information. | all |
| **Warning** | `#FF8800` | one reading out of range: rule, kicker and value amber, unit stays dim, the rest of the page untouched, nothing blinks. | all |
| **Critical** | `#FF4444` | the engine is at risk: full pulsing takeover, preempts everything, holds until acknowledged. Physical danger only. | dash, mobile |
| **Cut** | amber or red | the ECU is protecting itself: a persistent band, never a takeover (§3b). | dash, mobile mirror, Tuner log |
| **Failure** | `#FF4444` | the system cannot continue: danger rule on the normal ground, the reason in mono, one stated way out. Never a takeover. | all |

`#FF8800` is the amber already used for the dash's DO NOT UNPLUG line; it is now the warning role
everywhere. `#FF4747` (engaged) is a **fourth** red and only ever fills an engaged control — never a
severity. Never two severity colours on one element, never a fifth colour.

`- -` means **the sensor went quiet** (500 ms with no frame) and nothing else. It is not a "not
connected" placeholder, not an empty state and never appears in a demo or a preview.

---

## 1. Dash — `canshift-firmware`

Eight system states, all 320 × 240 (the reference renders at 2×, so halve every pixel you measure off it).

| # | State | Design |
| --- | --- | --- |
| D01 | **Boot** | The only centred screen on the dash. CS monogram (ink white, right half `#FF4747`) over the skewed CANSHIFT wordmark, on `#121212`, ~400 ms. No progress bar. |
| D02 | **Self-test** | 2 px ink rule + `SELF-TEST` kicker, then one row per check in mono 20 px device-scaled, 1 px rules between: DISPLAY, TOUCH, FLASH, CONFIG, CAN BUS. Result right-aligned in dim; a failed check is amber (`CAN BUS — NO FRAMES`). `CONTINUING IN 2 s` pinned at the bottom. |
| D03 | **Firmware update** | `UPDATING FIRMWARE` kicker, percent in huge mono, `0.1.0 → 0.1.1` under it, 10 px progress bar (`#222` track / `#FF4747` fill) pinned at the bottom, `DO NOT UNPLUG` in `#FF8800` below. |
| D04 | **Signals lost** | The page keeps its layout. Every stale value drops to dim and reads `- -` (unit kept); the bus-rate field in the status row goes amber (`CAN — Hz`); a live signal (BATT) stays white. One amber line at the bottom: `BUS SILENT 4 s — CHECK WIRING`. |
| D05 | **Critical alert** | Full `#FF4444`, pulsing 1 s. Signal name in Archivo 800 tracked wide, value in huge mono, threshold and context under it (`MIN 1.8 bar · AT 5200 rpm`), `STOP THE ENGINE` over a 2 px white-50 % rule at the bottom. No status row, no page nav. |
| D06 | **Config rejected** | Failure. 2 px `#FF4444` rule + `CONFIG REJECTED`, the reason in mono 44 px (`PAGE 4 EXCEEDS 240 PX`), the detail under it (`WIDGET EGT AT Y 208, H 52 — OVER BY 20`), and the way out at the bottom: `RUNNING LAST GOOD CONFIG — FIX IN THE TUNER`. |
| D07 | **No config (first boot)** | `NO CONFIG` kicker, `NOTHING TO DISPLAY YET`, then where to go (`CONNECT USB-C AND OPEN DOCS.CANSHIFT.TMBK.CH/TUNER`), and two outlined cells at the bottom: USB READY, FIRMWARE 0.1.0. |
| D08 | **Rev limit** | The whole shift-light row blinks on/off at 6 Hz (`steps(1)`, no easing). The rpm widget turns danger (rule, kicker, value); every other value keeps reading normally. Status row shows `LIMIT 7200`. |

Rules: a stale value keeps its unit; the dash never clears a page to show a message except D05; page
changes and state entries are instant cuts.

## 2. Controls — `canshift-firmware`

Five controls, one vocabulary: anti-lag, traction control, launch control, pit limiter, cruise.

### Four states per control

| State | Design |
| --- | --- |
| **Off** | 2 px `#FFFFFF` border, page ground, kicker dim, state word ink. |
| **Armed** | Identical to off, **pulsing 1 s** ease-in-out 100 % → 35 %. The condition is met but the function has not fired: `LAUNCH · 4200 rpm — ARMED`, `CRUISE · SET 110 — ARMED`, `PIT LIMIT · 60 km/h — READY`. |
| **Active** | Border **and** ground `#FF4747`, kicker white 75 %, state word white. **Steady** — an active control never pulses. A control that is intervening says so in its kicker (`TRACTION · CUTTING`); it does not move. |
| **Unavailable** | Border `#333333`, text `#6B6B6B`, and **the kicker states why** — never a bare grey-out: `ANTI-LAG · EGT HIGH`, `LAUNCH · MOVING`, `TRACTION · NO WHEEL SPEED`, `PIT LIMIT · GEAR 4`, `CRUISE · BRAKE CUT`. State word `LOCKED`, `N/A` or `CANCELLED`. |

The pulse belongs to armed and to armed only. No icons on any control: a traction-control glyph is an OEM
convention and unreadable at 320 px — the word plus the state carries it, and the word is what the driver
would say out loud.

### Two kinds of button

- **Toggle** — anti-lag, launch, pit limit, cruise. One tap engages, the next disengages. The state word
  is a word: `ON` / `OFF`, `ARMED`, `READY`.
- **Stepper** — traction control and the ECU map. **Each tap raises the level by one** (1, 2, 3 … 6), and
  the tap past the top **wraps back to `OFF`**, so the whole range is reachable with one finger and no
  second button. A **600 ms long press returns to level 1**. Under the state word, a row of segment cells
  is the only readout of the current level: 2 px device high, lit in ink (white at 30 % over an engaged
  fill), unlit in `#222222`.

## 3. Control splash screens — `canshift-firmware`

When a control changes state the dash takes the whole screen, so the driver reads the change without
hunting for a small button. Six cases, one fixed grammar.

**Grammar:** 2 px rule in the state's colour → control name as kicker (Archivo 800, 12 px device-scaled,
0.2em) → the new state at 128 px canvas / 64 px device in mono → the parameter that matters underneath.

| # | Splash | Colour | Content |
| --- | --- | --- | --- |
| S01 | Anti-lag engaged | accent | `ON`, with EGT and the cut limit in two mono cells at the bottom |
| S02 | Anti-lag off | ink | `OFF`, `EGT FALLING · 906 °C` |
| S03 | Launch armed | accent | `4200 rpm` as the hero, `ARMED` under it, `CLUTCH IN · RELEASE TO GO` at the bottom |
| S04 | Traction level changed | accent | the level digit as the hero, `OF 6 · MORE SLIP`, and the six segments at splash scale (22 px canvas tall) pinned at the bottom |
| S05 | ECU map changed | accent | the map number as the hero, the map name from the profile (`E85 · HIGH BOOST`), boost target and rpm limit at the bottom |
| S06 | Refused | danger | `LOCKED` as the hero, the condition in plain mono (`CAR IS MOVING — 24 km/h`), and what to do: `STOP TO ARM` |

Rules:

- **Engaging is accent, disengaging is ink, refusing is danger.** No red field — that stays reserved for
  the critical alert.
- **800 ms** for a change, **1.2 s** for a refusal. Both cut in and out with no transition.
- A new change **replaces** the splash and restarts the timer; splashes never queue.
- A critical alert preempts a splash immediately.
- Only a state the driver asked for earns a splash: engaging, disengaging, changing a level or a map, and
  a refusal. **A value crossing a threshold is a warning on its widget, never a splash.**
- The splash is a receipt, not a dialog: no pulse, no icon, no progress bar, and it never asks for a tap.
  The page underneath keeps running and returns untouched.

## 3b. Protection cuts — `canshift-firmware`

A cut is the ECU protecting itself, **not** something the driver asked for — so it never earns a splash and
never pulses. It gets **the cut band**: a persistent full-width strip directly under the shift light,
present for exactly as long as the cut is. 26 px canvas / 13 px device tall, flush left, one line.

**Grammar:** 2 px rule in the severity colour → CUT NAME (Archivo 800, 0.16em, uppercase) → the measured
value against its limit in mono → elapsed time right-aligned.

    ──────────────────────────────────────────────  2 px #FF8800
    BOOST CUT   OVERBOOST 1.94 · LIMIT 1.80              2.4 s

**Amber or red:**

- **Amber #FF8800** when the cut holds a target: overboost, rev limit, traction cut, pit limit cut. The
  detail line stays dim.
- **Red #FF4444** when it protects the engine: low oil pressure, overheat, limp mode. The detail line turns
  **ink** so it reads first.
- The value that caused the cut takes the same colour on its own widget (rule, kicker, value).

**The cut vocabulary** — one name per cut, taken from the ECU profile, never invented at render time:
BOOST CUT, FUEL CUT, IGNITION CUT, IGNITION RETARD, REV LIMIT, TRACTION CUT, PIT LIMIT CUT,
OVERHEAT PROTECT, LIMP MODE.

**Four reference cases** (F01–F04 in the plank):

| # | Case | Notes |
| --- | --- | --- |
| F01 | Boost cut | amber band + the BOOST widget in warning, TARGET beside it in ink so the pair reads as measured-vs-asked |
| F02 | Knock / ignition retard | the **retard** is the hero value (−4.5 °), not the event; the band names the cylinder and the event count; right field reads HOLDING |
| F03 | Fuel cut, engine protect | red band, detail in ink, OIL PRESS widget in danger. The critical alert (D05) follows on its own terms about 2 s later — the band never becomes one |
| F04 | Limp mode | red band reading LATCHED instead of an elapsed time, the imposed RPM limit as the hero, and how it clears: ignition off, or clear the fault from the Tuner |

**Timing:**

- The band appears within **one frame** of the cut flag and stays a **1.5 s minimum**, so a 60 ms cut is
  still readable.
- The elapsed counter runs while cutting; a latched cut reads LATCHED.
- Two concurrent cuts **stack**, most severe on top, **three maximum**; beyond that the third row reads
  "+2 MORE".
- The band never animates in or out — it cuts, like everything else on the dash.

**Mobile mirrors the band** at the top of the Dashboard screen, same colours and same names, read-only.
**The Tuner** lists cut events in Live data as timestamped rows using the same names and colours, so the
three products speak one cut vocabulary.

## 4. Tuner — `canshift-tuner`

Six states. **The editor never blocks on a modal** — every state is inline, in the panel it belongs to,
and editing stays possible.

| # | State | Design |
| --- | --- | --- |
| T01 | **Connecting** | 3 px ink left bar on `--color-neutral-100`: mono kicker `USB CDC`, title "Opening the port…", one line of explanation. No spinner. |
| T02 | **Burn failed** (failure) | 3 px accent bar on `--color-accent-100`: kicker `PUT_CONFIG · E_CRC` in `--color-accent-700`, title "The dash rejected the write", the detail (which chunk), the reassurance that the dash kept its previous config, then `RETRY BURN` (primary) + `Read the error codes` (secondary). |
| T03 | **Bus silent** (warning) | 3 px `#FF8800` bar: kicker `CAN · 0 FRAMES IN 6 s`, title "The dash is connected, the bus is not", and what to check (bus rate, 120 Ω termination). Editing keeps working. |
| T04 | **No ECU profile** (empty) | 1 px box, no bar: title "No signals to bind yet", why widgets cannot read anything, then `PICK A PROFILE` (primary) + `Capture the bus` (secondary). |
| T05 | **Out of bounds** (failure) | 3 px accent bar: kicker `PAGE 4 · WIDGET EGT`, title "This layout will not fit 320 × 240", the overflow in numbers, and the consequence — burn stays disabled until it fits, because the dash never scales a layout. |
| T06 | **Live data loading** | Panel header `LIVE DATA · LISTENING…`, then **skeleton rows, no spinner**: the row count equals the profile's signal count, so the panel does not resize when data arrives. |

## 5. Mobile — `canshift-mobile`

Five states, 390 × 844.

| # | State | Design |
| --- | --- | --- |
| M01 | **Pairing** | Device rows separated by 1 px `#262323`; the connecting device shows `CONNECTING` in `#FF4747`, the others their RSSI in dim. At the bottom: "The dash shows a four-digit code. Confirm it there." |
| M02 | **Out of range** (warning) | Header keeps the screen title, with `LINK LOST 8 s AGO` in `#FF8800` under it over the 2 px rule. Values drop to dim `- -`. Footer states the policy: last values held 30 s, then cleared, reconnecting automatically. |
| M03 | **Critical alert** | Full `#FF4444`, pulsing. Signal name Archivo 800 tracked wide, value ~92 px mono, threshold, then a context table (RPM / OIL TEMP / SINCE) over a 2 px white-50 % rule, `STOP THE ENGINE`, and two actions: `ACKNOWLEDGE` (white fill, `#FF4444` text) and `MUTE FOR THIS SESSION` (white-70 % outline). No tab bar while it holds. |
| M04 | **Empty log** | Console with its CAN/LOG/SEND tabs intact; the empty message in mono uppercase over three lines saying **why** it is empty ("THE BUS IS QUIET OR THE FILTER IS TOO NARROW"), one outlined `CLEAR FILTER` action, and the active filter stated at the bottom. |
| M05 | **Low battery** (warning) | Status-row battery turns `#FF8800`; a 3 px amber left bar states the consequence — logging stops below 5 % to protect the recording, the dash keeps running on its own — then the normal rows, and `STOP LOGGING NOW` outlined in `#FF4747`. |

Mobile alerts are **read-only mirrors of the dash's state**: acknowledging on the phone acknowledges on
the dash, and the phone never originates an alert.

## 6. Non-negotiable, all three repos

- Radius 0, no shadow, no gradient, no icon in any state or alert.
- Everything flush left, including button labels and alert copy. Only the dash boot screen is centred.
- Never render or fake a value the app is not receiving. `- -` is the stale-signal state, nothing else.
- Warning never blinks. Only armed pulses. Active is steady.
- Every failure states the reason **and** one way out. Every unavailable control states its condition in
  the kicker.
- The critical takeover is the only full red field in the entire system.

## 7. Order of work

1. Tokens: add the warning `#FF8800` and, on the dash, the locked pair `#333333` / `#6B6B6B`.
2. The severity grammar as a shared primitive (one rule + kicker + colour), then rewrite the existing
   ad-hoc error displays on top of it.
3. Dash: D01–D08, then the four control states, then the stepper behaviour, then the splashes, then the cut
   band (§3b) — the band is a page-level element, so wire it before the individual cut cases.
4. Tuner: T01–T06, replacing any modal with its inline equivalent.
5. Mobile: M01–M05.
6. Timing pass: 800 ms / 1.2 s splashes, 1 s armed pulse, 6 Hz rev-limit blink, 500 ms stale, 30 s hold on
   mobile. Verify each with a stopwatch or a test, not by eye.

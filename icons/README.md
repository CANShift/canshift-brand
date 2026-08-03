# CANShift — repository icons

Five marks, one per repo under [github.com/CANShift](https://github.com/CANShift).
They are instrument readings, not object drawings: same 116 × 100 frame and 11° shear as the CS monogram,
three stroke weights only (13 u for signal, 7 u for rules and frames, 18 u for handles — 15 and 22 u on the small variants), exactly one red element each.

| Repo | The mark is |
| --- | --- |
| `canshift-firmware` | a bit sampled on the wire — recessive, then dominant; the red half is the edge the firmware acts on |
| `canshift-core` | a CAN frame cut into fields; the red bar in the last one is what the parser hands back as a decoded signal |
| `canshift-tuner` | a widget selected on the canvas: dashed bounds, four handles, one grabbed |
| `canshift-mobile` | four RSSI bars on their axis, the paired one in red — the BLE link the app lives on |
| `canshift-docs` | a drawing sheet with its title block; the red rule is the revision line |

## Files per repo

| File | Use |
| --- | --- |
| `<name>.svg` | ink #201E1D on light grounds, accent #EC3013 |
| `<name>-dark.svg` | #F3F2F2 on dark grounds, accent #FF4747 |
| `<name>-currentcolor.svg` | ink follows the surrounding text colour |
| `<name>-small.svg` | 16–24 px variant: no shear, weights at 15 u, secondary rules dropped |
| `<name>-small-dark.svg` | same, on dark |
| `<name>-onaccent.svg` | all-white on the #EC3013 ground — a red element on red would vanish |
| `<name>-avatar-460.svg` | 460 × 460 repo avatar on #151313 — what GitHub shows |
| `<name>-appicon-1024.svg` | 1024 × 1024 app icon, mark at 54 % — iOS applies its own mask, the artwork stays square |
| `<name>-maskable-1024.svg` | 1024 × 1024 maskable / Android adaptive, mark at 42 % so a circular crop takes nothing |

## Rules

- One accent element per mark. Never two, never a red outline.
- On dark grounds the accent becomes #FF4747 — the same red as the dash.
- Below 24 px use the `-small` variant: the shear closes up, weights step to 15 u, secondary rules are cut.
- Do not rotate, do not recolour the ink to the accent, do not add a container shape. The square plate is the only container.
- The CS monogram stays the umbrella mark — org avatar, splash, the icon on the dash. A repo mark identifies one piece inside the system; it never replaces it.
- A sixth repo gets the same treatment: one reading, one red element, and it must name what the repo does.

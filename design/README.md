# CANShift — maquettes mobile & dash

Pour Claude Code, avec les prompts de correction.

| Fichier | Contenu |
| --- | --- |
| `CANShift Dash Pages.dc.html` | les six pages du dash rendues à 2× — la référence visuelle du firmware |
| `CANShift Mobile.dc.html` | la maquette de l'app mobile — 9 écrans à 390 × 844 |
| `CANShift Brand Book.dc.html` | le brand book — §04 est la référence du dash (grammaire des widgets, 6 pages, boot/OTA/alerte), §05 le résumé mobile |
| `PROMPT_MOBILE_DESIGN.md` | prompt de correction de l'app mobile (spec auto-suffisante) |
| `PROMPT_DASH_DESIGN.md` | **périmé** — remplacé par `docs/design/DASH_DESIGN_SYSTEM.md` dans canshift-firmware |
| `support.js`, `_ds/` | runtime + design system — nécessaires pour ouvrir les `.dc.html` dans un navigateur |

Ouvrir les maquettes directement dans un navigateur, tout le dossier doit rester ensemble.

Le dash a sa spec contraignante dans le firmware, pas ici : `canshift-firmware/docs/design/DASH_DESIGN_SYSTEM.md`
(tokens, type, grille, grammaire, motion) et `DASH_PAGES.json` (les six pages en données, en pixels device).
Les maquettes de ce dossier en sont la référence visuelle — elles rendent à 2×, donc diviser par deux pour
comparer à une capture de l'appareil.

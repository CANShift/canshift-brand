# CANShift — maquettes tuner, mobile & dash

Pour Claude Code, avec les prompts de correction.

| Fichier | Contenu |
| --- | --- |
| `CANShift Tuner v2.dc.html` | la maquette de l'app web — coquille à cinq onglets (HOME · DASH · SIGNALS · LIVE · DEVICE), deux thèmes, cliquable |
| `PROMPT_TUNER_APP.md` | prompt du tuner v2 (spec auto-suffisante : tokens, coquille, les cinq écrans, gestes, stockage, clavier, non-négociables) |
| `CANShift Dash Pages.dc.html` | les six pages du dash rendues à 2× — la référence visuelle du firmware |
| `CANShift Mobile.dc.html` | la maquette de l'app mobile — 9 écrans à 390 × 844 |
| `CANShift States and Alerts.dc.html` | tous les états et alertes des trois produits — 4 niveaux de gravité, 8 états dash, la matrice des commandes, 6 splashscreens, 4 bandeaux de coupure, 6 états tuner, 5 états mobile |
| `CANShift Brand Book.dc.html` | le brand book — §04 est la référence du dash (grammaire des widgets, 6 pages, boot/OTA/alerte), §05 le résumé mobile |
| `PROMPT_STATES_AND_ALERTS.md` | prompt des états et alertes, couvre les trois dépôts (firmware §1–3, tuner §4, mobile §5) |
| `DASH_DESIGN_SYSTEM.md` | le design system du dash, révision avec la couche états (4 niveaux, tokens `CS_WARN` / `CS_LOCK_LINE` / `CS_LOCK_INK`, les quatre états de bouton) |
| `PROMPT_MOBILE_DESIGN.md` | prompt de correction de l'app mobile (spec auto-suffisante) |
| `PROMPT_DASH_DESIGN.md` | **périmé** — remplacé par `docs/design/DASH_DESIGN_SYSTEM.md` dans canshift-firmware |
| `support.js`, `_ds/` | runtime + design system — nécessaires pour ouvrir les `.dc.html` dans un navigateur |

Ouvrir les maquettes directement dans un navigateur, tout le dossier doit rester ensemble.

Le tuner a sa copie du prompt dans `canshift-tuner/docs/design/PROMPT_TUNER_APP.md` — c'est la même,
et c'est celle que lit Claude Code dans ce dépôt. La maquette ne sert qu'à confirmer les proportions.

Le dash a sa spec contraignante dans le firmware, pas ici : `canshift-firmware/docs/design/DASH_DESIGN_SYSTEM.md`
(tokens, type, grille, grammaire, motion) et `DASH_PAGES.json` (les six pages en données, en pixels device).
Les maquettes de ce dossier en sont la référence visuelle — elles rendent à 2×, donc diviser par deux pour
comparer à une capture de l'appareil.

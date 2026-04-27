# Prehistoric Encyclopedia — Repository Architecture

This repository now follows a clearer, scalable folder tree.

## Current folder tree

```text
.
├── index.html
├── data/
│   └── animals.json
├── pages/
│   └── species/
│       ├── ammonite.html
│       ├── tyrannosaurus-rex.html
│       └── ...
├── assets/
│   └── images/
│       ├── ammonite.png
│       ├── tyrannosaurus-rex.png
│       └── ...
└── docs/
    └── ARCHITECTURE-BLUEPRINT.md
```

## Design rules

- `index.html` is the entry point and catalog UI.
- All species pages live under `pages/species/`.
- All images are centralized under `assets/images/`.
- `data/animals.json` is the catalog source-of-truth for homepage links.
- A species `id` must match both:
  - page filename: `pages/species/<id>.html`
  - image filename: `assets/images/<id>.png`

## Why this is easier to use

- Cleaner separation between UI (`index.html`), data (`data/`), content pages (`pages/`), and static assets (`assets/`).
- New species onboarding is predictable: add one HTML page, one image, one JSON entry.
- Safer future migration to templates/SSG because content and assets are already grouped.

## Next recommended step

Move from manual species HTML pages to a shared template system so repeated nav/sections are no longer duplicated.

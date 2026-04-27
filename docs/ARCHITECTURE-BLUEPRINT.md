# Prehistoric Encyclopedia — Architecture Blueprint

## Why this blueprint
The current site already has rich content and visual quality, but scaling it to dozens/hundreds of species requires a data-first architecture. This blueprint defines a safe migration path that keeps GitHub Pages compatibility while improving maintainability, scientific traceability, and performance.

---

## Target architecture (v2)

### 1) Content layer (single source of truth)
- `data/animals.json` — lightweight catalog for homepage and navigation.
- `content/animals/<id>.md` (future phase) — full research entries with structured frontmatter.
- `data/eras.json` (future phase) — era metadata and UI labels.

### 2) Presentation layer
- Home page reads from `data/animals.json`.
- Species pages generated from templates (future: SSG), not manually duplicated HTML.
- Shared design tokens and reusable blocks (hero, dossier, timeline, references).

### 3) Scientific metadata layer
Each species should include:
- `taxonomy`
- `time_range_ma` (start/end)
- `measurements` (length, mass, etc.)
- `claims[]` with:
  - `statement`
  - `source` (DOI / museum / peer-reviewed reference)
  - `confidence` (`consensus`, `debated`, `legacy`)
  - `last_verified`

### 4) Quality gates (CI)
- JSON schema validation for catalog/content.
- Link checker (internal links + image paths).
- HTML validation (when static pages remain).
- Optional scientific lint: warn if claim has no source.

---

## Recommended repository structure

```text
.
├── index.html
├── data/
│   ├── animals.json
│   └── eras.json                # future
├── content/
│   └── animals/                 # future markdown entries
├── templates/                   # future shared layouts/components
├── images/
├── docs/
│   └── ARCHITECTURE-BLUEPRINT.md
└── .github/
    └── workflows/
        ├── validate-content.yml # future
        └── deploy.yml           # future
```

---

## Migration plan (practical)

### Phase 1 — now (completed in this PR)
- Create central `data/animals.json` catalog.
- Make `index.html` load animals from JSON instead of hardcoded array.
- Keep existing species HTML pages unchanged for zero-risk rollout.

### Phase 2 — standardization
- Remove accidental markdown fences from all affected HTML files.
- Normalize repeated blocks (nav/footer/modal) into shared partials or templates.
- Add `data/eras.json` and move era labels/colors there.

### Phase 3 — scientific rigor
- Add per-species source sections with machine-readable references.
- Introduce confidence tags per claim.
- Add "last updated" and "verified by" metadata.

### Phase 4 — full generation
- Move to a static-site generator (Astro/Eleventy/Next static export).
- Generate species pages automatically from content collection.
- Keep GitHub Pages deployment via CI.

---

## Non-breaking standards to adopt immediately
- Every new species entry must be added to `data/animals.json`.
- `id` must match image filename in `images/<id>.png`.
- `path` must be explicit even when `ready: false` (future-proofing).
- No direct hardcoded species arrays in UI code.

---

## Suggested next task
Convert one species page (e.g., `tyrannosaurus-rex`) into a template-driven page with structured metadata and references as a pilot.

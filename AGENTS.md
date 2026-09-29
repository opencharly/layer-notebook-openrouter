# AGENTS.md — layer-notebook-openrouter

Standalone candy repo for the `notebook-openrouter` data layer — 3 Jupyter
notebooks demonstrating the OpenRouter API (basics, model discovery, practical
inference), seeded into the workspace volume of a Jupyter image at deploy time.
The candy lives in `charly.yml` at the repo root: the `env_require:`
declaration, the `data:` mapping into the `workspace` volume, the `plan:`
`check:` assertions, and the embedded `skill:` entity projected into the
marketplace corpus as `/charly-jupyter:notebook-openrouter`.

Canonical files:

- `charly.yml` — the `notebook-openrouter:` candy entity and the
  `notebook-openrouter-skill:` skill entity.
- `data/openrouter/` — the notebooks and `notebooks.yaml`.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-jupyter:notebook-openrouter` — the owning skill. The notebook catalog,
  the `env_require` feature, and the free-tier rate-limit gotcha. Load before
  editing or troubleshooting the layer.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:`, the `data:` and `env_require:` fields, and
  service declarations). Load before editing any entity field or plan step.

## Build / validate / test

- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no
  per-repo candy gate** and ships only `.github/workflows/tag-on-merge.yml`.
- The candy's `plan:` `check:` steps are the functional evidence — they assert
  the data subdirectory is provisioned, the producer notebook is staged, and the
  key declaration is honoured. A change to the collection must keep those checks
  honest.

## Modify this repo

- Edit the `notebook-openrouter:` candy entity AND the
  `notebook-openrouter-skill:` skill entity in `charly.yml` together. The skill
  is the projected usage source, so a data, env, path, or behaviour change not
  mirrored in the skill leaves the corpus stale.
- `env_require:` declares a runtime credential (`OPENROUTER_API_KEY`) as an OCI
  label; keep it in step with what the notebooks actually read.
- New behaviour claims belong in the `plan:` as an observable `check:` step, and
  in the skill body.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.

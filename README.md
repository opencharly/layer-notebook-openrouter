# notebook-openrouter

The OpenRouter API integration notebook collection as a charly *data layer* — 3
Jupyter notebooks demonstrating the OpenRouter API, seeded into the workspace
volume of a Jupyter image at deploy time.

The `notebook-openrouter` candy ships no packages, no services and no
dependencies. Its whole job is to stage `data/openrouter/` into the `workspace`
volume's `openrouter/` subdirectory. It also declares `env_require:
OPENROUTER_API_KEY`, so `charly config` warns at deploy time when the key is
unset — it was the first candy in the project to use the `env_require` feature.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `notebook-openrouter` |
| Type | Data-only — no packages, no services, no dependencies |
| Volume | `workspace` → `/workspace` (supplied by the jupyter base) |
| Data | `data/openrouter` → `workspace` volume, dest `openrouter` |
| Requires at runtime | `OPENROUTER_API_KEY` (declared via `env_require`) |
| Notebooks | 3 `.ipynb` files + `notebooks.yaml` |

## Notebook contents

| Notebook | Topic | Features |
|---|---|---|
| `00_OpenRouter_Basics.ipynb` | API basics | Authentication, simple chat, multi-turn, error handling, usage/cost tracking |
| `01_OpenRouter_Models.ipynb` | Model selection | List models, filter free models, inspect details (pricing, context, modality), compare responses |
| `02_OpenRouter_Inference.ipynb` | Practical inference | Structured JSON output, summarization, code generation, reasoning with thinking tokens, translation |

The notebooks default to the free model `qwen/qwen3.6-plus:free`, which has
strict free-tier rate limits (~1 request/minute). Each notebook ships a retry
helper that handles HTTP 429 with exponential backoff and transient 502s from the
Qwen provider.

## How to use it

Provide the API key and deploy:

```bash
# Via charly config (persisted in the quadlet)
charly config jupyter-ml-notebook -e OPENROUTER_API_KEY=sk-or-v1-...
charly start jupyter-ml-notebook
# Open http://localhost:8888 -> navigate to openrouter/
```

If `OPENROUTER_API_KEY` is absent, `charly config` prints a warning:

```
Warning: jupyter-ml-notebook requires OPENROUTER_API_KEY (API key for OpenRouter LLM inference) — not set
```

Compose the layer in a box's `candy:` list:

```yaml
jupyter-ml-notebook:
  candy:
    base: fedora-nonfree
    candy:
      - '@github.com/opencharly/layer-notebook-openrouter:v2026.239.1600'
      # ... other notebook data layers
```

## Layout

- `charly.yml` — the `notebook-openrouter:` candy entity (the `env_require:`
  declaration, the `data:` mapping, and the `plan:` checks) plus the embedded
  `skill:` entity.
- `data/openrouter/` — the 3 notebooks and `notebooks.yaml`.
- `README.md` — this user overview.

## Related

- Owning skill: `/charly-jupyter:notebook-openrouter` — the notebooks, the
  `env_require` feature, and the free-tier rate-limit gotcha.
- Sibling data layers: `/charly-jupyter:notebook-ollama`,
  `/charly-jupyter:notebook-llm-on-supercomputers`,
  `/charly-jupyter:notebook-templates`.
- Consuming box: `/charly-jupyter:jupyter-ml-notebook`.
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella.

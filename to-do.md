# Improvement Roadmap

Ordered by impact for a senior AI/Software Engineer portfolio.

---

## Phase 1 — Code Quality (refactor notebook → proper Python package)

- [ ] Convert notebook into a proper project structure:
  ```
  gen-ai-music/
  ├── app/
  │   ├── __init__.py
  │   ├── generator.py      # MusicGen wrapper class
  │   ├── config.py         # generation params as dataclass/pydantic
  │   └── utils.py          # audio save, resample helpers
  ├── notebooks/
  │   └── exploration.ipynb # keep notebook for demo only
  ├── outputs/              # gitignored
  └── main.py               # CLI entrypoint
  ```
- [ ] Wrap model in a class with proper `__init__`, lazy loading, device detection
- [ ] Use `pydantic` or `dataclasses` for generation config instead of loose params
- [ ] Add type hints everywhere

---

## Phase 2 — CLI / API Interface

- [ ] Build a CLI with `argparse` or `typer`:
  ```bash
  python main.py --prompt "calm jazz" --duration 15 --output outputs/jazz.wav
  ```
- [ ] Build a REST API with FastAPI:
  - `POST /generate` — accepts prompt + params, returns audio file
  - `GET /models` — list available MusicGen model sizes
- [ ] Add async generation (non-blocking endpoint with job ID + polling)

---

## Phase 3 — Experiment Tracking

- [ ] Log every generation to a local SQLite or JSON log:
  - prompt, model size, duration, temperature, cfg_coef, generation time, output path
- [ ] Add MLflow or Weights & Biases run tracking for parameter experiments
- [ ] Build a simple comparison: same prompt across `musicgen-small` vs `musicgen-medium`

---

## Phase 4 — Evaluation & Quality

- [ ] Add objective audio metrics (CLAP score — measures text-audio alignment)
- [ ] Add generation time benchmarks (CPU vs GPU comparison table in README)
- [ ] Prompt engineering experiments: document which prompt styles produce best results

---

## Phase 5 — Gradio / Streamlit Demo (most visible for portfolio)

- [ ] Build a Gradio UI:
  - Text input for prompt
  - Sliders for duration, temperature, cfg_coef
  - Audio player for output
  - Download button
- [ ] Deploy to Hugging Face Spaces (free, public, shareable link for Upwork)

---

## Phase 6 — README (hiring managers read this first)

- [ ] Write a proper README.md:
  - What it does + demo GIF or audio sample embed
  - Architecture diagram (T5 → MusicGen → EnCodec pipeline)
  - Quickstart in 3 commands
  - Link to live Hugging Face Space demo
  - Results table (prompt → generated audio description)
- [ ] Add a `samples/` folder with pre-generated `.wav` examples committed to repo

---

## Phase 7 — Advanced (differentiates from juniors)

- [ ] Fine-tune MusicGen on a custom genre dataset (even 10-20 samples shows you know training)
- [ ] Add melody conditioning (`MusicGen.get_pretrained('facebook/musicgen-melody')`)
- [ ] Implement prompt chaining: generate music that evolves across sections (intro → verse → chorus)
- [ ] Add `stereo` model variant and compare mono vs stereo output quality

# Mental-health Bias (MHB) utilities

This repository contains two small scripts to prepare a counterfactual dataset and evaluate bias in generated mental-wellbeing suggestions using Google Generative AI (Gemini).

**Files**
- [prepare_counterfactual_dataset.py](prepare_counterfactual_dataset.py): extracts base prompts from `facebook/empathetic_dialogues`, injects demographic templates (gender, religion, culture) and writes `ed_augmented.jsonl`.
- [evaluate_bias_pipeline.py](evaluate_bias_pipeline.py): generates short supportive suggestions with Gemini, inspects them for bias with a second Gemini call, computes average bias per example, and writes incremental results to `ed_results.jsonl` and deltas to `ed_deltas.jsonl`.

**Quick Setup**
1. Create and activate a virtual environment (optional but recommended):

```powershell
python -m venv .venv
& .venv\Scripts\Activate.ps1
pip install --upgrade pip
```

2. Install dependencies used by the scripts:

```powershell
pip install python-dotenv datasets google-generative-ai
```

3. Add your Gemini API key to a `.env` file in the repo root:

```text
GEMINI_API_KEY=your_actual_api_key_here
```

Note: `.env` and `.venv` are already ignored in `.gitignore`.

**Usage**
- Prepare the counterfactual dataset (produces `ed_augmented.jsonl`):

```powershell
python prepare_counterfactual_dataset.py --n 3000 --out ed_augmented.jsonl
```

- Run the bias evaluation pipeline (incremental; resumes if `ed_results.jsonl` exists):

```powershell
python evaluate_bias_pipeline.py --input ed_augmented.jsonl --output ed_results.jsonl --deltas ed_deltas.jsonl --n 1000
```

**Notes & Behavior**
- `prepare_counterfactual_dataset.py` selects prompts from the `context` (or falls back to `prompt`) column of the Empathetic Dialogues dataset, enforces a minimum character length, and creates grouped variants using a shared `group_id` so counterfactuals align.
- `evaluate_bias_pipeline.py`:
  - Requires `GEMINI_API_KEY` in `.env`.
  - Uses two Gemini models: one to generate suggestions and one to inspect for bias.
  - Writes results incrementally and supports resuming by checking existing `row_id` values in the output file.
  - Computes per-group deltas with `ed_deltas.jsonl`.

**Next steps you might want**
- Add a `requirements.txt` or `pyproject.toml` for reproducible installs.
- Add lightweight tests or a small example `ed_augmented.jsonl` for quick smoke runs.

If you want, I can add `requirements.txt` and example commands or run a small end-to-end smoke test locally.

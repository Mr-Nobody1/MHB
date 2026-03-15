# Mental-health Bias (MHB) utilities

This repository contains dataset prep, evaluation scripts, and notebooks for bias analysis of generated mental-wellbeing suggestions across Gemini, LM Studio, and OpenAI workflows.

## Project layout

- `scripts/`
  - `prepare_counterfactual_dataset.py`: builds the augmented Empathetic Dialogues dataset.
  - `evaluate_bias_pipeline.py`: runs the Gemini baseline evaluation pipeline.
- `notebooks/`
  - `openai_bias_pipeline.ipynb`: OpenAI-based bias pipeline.
  - `local/`: LM Studio and local-model notebooks.
  - `experiments/`: exploratory notebooks such as the iterative mitigation loop.
- `data/input/`
  - `ed_augmented.jsonl`: counterfactual input dataset.
- `data/output/gemini/`
  - Gemini pipeline outputs.
- `data/output/lmstudio/`
  - LM Studio pipeline outputs.
- `data/output/openai/`
  - OpenAI pipeline outputs.
- `docs/`
  - `paper.md`: project notes and write-up material.

## Quick setup

1. Create and activate a virtual environment:

```powershell
python -m venv .venv
& .venv\Scripts\Activate.ps1
pip install --upgrade pip
```

2. Install the main dependencies:

```powershell
pip install python-dotenv datasets google-generative-ai openai pandas matplotlib seaborn
```

3. Add the required keys to `.env` in the repo root:

```text
GEMINI_API_KEY=your_gemini_key_here
OPENAI_API_KEY=your_openai_key_here
OPENAI_MODEL=gpt-5-mini
```

## Usage

Prepare the counterfactual dataset:

```powershell
python scripts/prepare_counterfactual_dataset.py --n 3000
```

Run the Gemini evaluation pipeline:

```powershell
python scripts/evaluate_bias_pipeline.py --n 1000
```

Open the notebooks from `notebooks/` and run them from the top. They now resolve the project root automatically and read/write from `data/input/` and `data/output/...`.

## Notes

- `scripts/prepare_counterfactual_dataset.py` writes to `data/input/ed_augmented.jsonl` by default.
- `scripts/evaluate_bias_pipeline.py` writes to `data/output/gemini/` by default.
- The OpenAI notebook writes to `data/output/openai/`.
- The LM Studio notebooks write to `data/output/lmstudio/`.

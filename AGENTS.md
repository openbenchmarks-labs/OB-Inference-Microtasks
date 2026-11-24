# AGENTS.md — OB-Inference (Microtasks)

Quick navigation guide for coding agents working in `OB-Inference-Microtasks`.

## Structure (Zero Overlap)

| Folder | Owns | Files |
|---|---|---|
| **`data/`** | Dataset format and task prompts (the question set is private) | `data/README.md`, `data/prompt_templates/*.system.txt` |
| **`harness/`** | Execution only: run settings, CLI, provider requests, streaming, scheduling, run locks, checkpoints | `harness/spec.yaml`, `harness/cli.py`, `harness/providers.py`, `harness/transport.py`, `harness/runner.py`, `harness/lifecycle.py`, `harness/schedule.py`, `harness/dataset.py`, `harness/storage.py`, `harness/watcher.py`, `harness/config.py` |
| **`tools/`** | Provider setups & pricing (one file per tool for clean PRs) | `tools/<tool>.yaml` (10 tools) |
| **`judge/`** | Grading only: answer checks, metrics, report | `judge/evaluation.py`, `judge/metrics.py`, `judge/reporting.py` |

Tests sit next to the code they cover in `harness/tests/` and `judge/tests/`. Benchmark order & win condition live at the root in `benchmark.yaml`.

## Commands

```sh
# Offline tests (no API keys)
uv venv && uv pip install -e ".[dev]"
.venv/bin/pytest
```

## Guidelines

- **Keep folders strictly separated:** Dataset format and prompts in `data/`; execution in `harness/`; per-tool configs in `tools/<tool>.yaml`; grading in `judge/`.
- **Adding or tuning a tool:** Edit `tools/<tool>.yaml` (a new tool also goes in `TOOLS` in `harness/providers.py`), then run `pytest`.
- **No secrets, private questions, or raw run logs in git:** Keep API keys in `.env`, the dataset in `data/micro-single-task/`, and run outputs in `runs/` (all gitignored).
- **Consistent terminology:** Use `tool`, `setup`, `question`, `task family`, `ground truth`, `judge`, `smoke run`, `full run`, `board`.

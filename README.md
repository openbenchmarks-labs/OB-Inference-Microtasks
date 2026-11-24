# OB-Inference-Microtasks

Open, independent LLM inference provider benchmark: serverless endpoints measured on end-to-end latency, p99 tail latency, streaming tokens per second and exact task accuracy. Every provider serves the same model (GLM 5.3 Flash) on the same paired micro-tasks, with the same request settings, at the same moment. Providers measured: Baseten, DeepInfra, Fireworks AI, Modal, Nebius, Novita AI, Parasail, Telnyx, Together AI and Z.AI.

**Live leaderboard:** [openbenchmarks.com/inference](https://openbenchmarks.com/inference) · **Public dataset:** [`openbenchmarks/OB-Inference-Microtasks`](https://huggingface.co/datasets/openbenchmarks/OB-Inference-Microtasks)

Published and maintained by **[OpenBenchmarks Labs](https://openbenchmarks.com)**.

## Methodology

The benchmark compares single-user serverless inference endpoints across three short, deterministic task families:

- **Meeting-notes lookup** (`meeting-notes-lookup`)
- **Ticket triage** (`ticket-triage`)
- **Contract-terms extraction** (`contract-terms-extraction`)

Every provider receives paired streaming requests with fixed parameters (`harness/spec.yaml`), and `judge/` grades exact task correctness deterministically alongside latency (median and p99), throughput (tokens/s), and failure rates. It does not measure model quality across different models, high-concurrency load testing, dedicated endpoints, or long-form generation.

## Tools evaluated

10 serverless inference providers configured in [`tools/`](tools/README.md):

`baseten`, `deepinfra`, `fireworks`, `modal`, `nebius`, `novita`, `parasail`, `telnyx`, `together`, `zai`

## Dataset

System prompt templates for all three task families are committed in `data/prompt_templates/`.

- **Public split:** Published on Hugging Face at [`openbenchmarks/OB-Inference-Microtasks`](https://huggingface.co/datasets/openbenchmarks/OB-Inference-Microtasks).
- **Held-out split:** The frozen evaluation set used for the published leaderboard stays private. Place local dataset files under `data/micro-single-task/` (gitignored) before running live benchmarks.

See [`data/README.md`](data/README.md) for the directory layout, manifest format, and JSONL schema.

## Repository structure

- [`data/`](data/README.md): System prompt templates (`prompt_templates/`), dataset layout, and schema documentation.
- [`harness/`](harness/README.md): CLI runner (`openbench-inference`), run spec (`harness/spec.yaml`), and provider adapters.
- [`tools/`](tools/README.md): One `<tool>.yaml` config per inference provider (10 tools).
- [`judge/`](judge/README.md): Deterministic answer evaluator, latency/throughput/failure metrics, and report generator.
- [`benchmark.yaml`](benchmark.yaml): Benchmark contract, scope, and refresh cadence.
- [`AGENTS.md`](AGENTS.md): Entry guide for coding agents.

## Quick start

Run the offline test suite (no API keys required):

```sh
uv venv && uv pip install -e ".[dev]"
.venv/bin/pytest
```

Use one virtual environment per `OB-*` repository (all `OB-*` repos install top-level `harness`, `judge`, and `tools` packages).

To run against live providers, copy `.env.example` to `.env`, add your keys, place the dataset as described in [`data/README.md`](data/README.md), and inspect the CLI:

```sh
cp .env.example .env
.venv/bin/openbench-inference --help
```

## Adding or updating a tool

1. Add or edit `tools/<tool>.yaml` (and `.env.example` if adding a new API key). For a new provider, also add its filename to `TOOLS` in `harness/providers.py` and update the expected `PROVIDER_COUNT` and request totals.
2. Run `.venv/bin/pytest`.
3. Open a pull request.

## Links

- [Live leaderboard](https://openbenchmarks.com/inference)
- [JSON API](https://openbenchmarks.com/api/benchmarks/inference)
- [Hugging Face dataset](https://huggingface.co/datasets/openbenchmarks/OB-Inference-Microtasks)
- [`llms.txt`](https://openbenchmarks.com/llms.txt)

## Related benchmarks

- [OB-Voice-Latency](https://github.com/openbenchmarks-labs/OB-Voice-Latency): Real PSTN voice-agent latency ([leaderboard](https://openbenchmarks.com/voice-agent-latency))
- [OB-LegalQA-Document-Parsing](https://github.com/openbenchmarks-labs/OB-LegalQA-Document-Parsing): Document parsers on redlined legal contracts ([leaderboard](https://openbenchmarks.com/document-processing))
- [OB-Code-WebSearch](https://github.com/openbenchmarks-labs/OB-Code-WebSearch): Web search APIs for coding agents ([leaderboard](https://openbenchmarks.com/web-search-for-coding-agents))
- [All OpenBenchmarks leaderboards](https://openbenchmarks.com)

## License

MIT. See [LICENSE](LICENSE).

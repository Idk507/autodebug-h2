# AutoDebug-H²

A runnable, **deterministic** Python debugging demonstration and optional adapter
that calls the **actual upstream Harness²** recursion scheduler. The local agent
is intentionally rule-based: this is a framework experiment, not a general LLM
code repair product. `harness2` mode needs the upstream dependency installed.

## Requirements

Python 3.12 or 3.13 and pytest. For the upstream integration, follow the
[Harness² Linux installation instructions](https://github.com/google-research/harness2).
Use WSL2 Ubuntu if developing on Windows. External model credentials are **not**
needed for either deterministic mode.

## Quick start

```bash
python -m venv .venv
source .venv/bin/activate  # Windows PowerShell: .venv\Scripts\Activate.ps1
python -m pip install -e '.[dev]'
pytest -q
python -m autodebug.main demo --output runs
```

The demo prints results for two Python fixes and writes `runs/results.json`.
The test suite verifies that each baseline fails and the refined candidate
passes. It modifies only temporary copies, not `tasks/`.

## Actual Harness² integration

Install the official repo alongside this project using its documented Linux
steps. With its Python package available in the same environment, run:

```bash
python -m autodebug.main harness2 --output runs --k 1
```

The adapter maps the two Python debugging fixtures to a custom Harness² domain
and calls `harness2.core.schedule.run_domain`. A deterministic proposer changes
`ENABLE_EDGE_CASES` in an editable harness script, so this is an authentic
**orchestration** demo without model-backed improvement or general coding ability.
It is not the official Harness² built-in demo. The integration depends on the
upstream version; check compatibility if the public API changes.

## Security

The project executes **trusted local** pytest fixtures in a temporary directory.
A timeout does not provide a secure sandbox. Never pass untrusted code to this
runner or the upstream unconfined Codex executor. Real deployments must add
container/microVM isolation, separate credentials, egress controls, and policy
review. Candidate source files are never executed directly except through
pytest in the trusted local fixtures.

## Limitations

Only two narrow bug patterns are recognized. All correctness checking uses
local pytest and is not the paper's answer-free selection. The local demo was
intended to be executable without external services. No promise is made that
an unrelated Python bug can be repaired.

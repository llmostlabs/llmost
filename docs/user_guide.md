# User Guide

Who this is for: first-time local-LLM users who want to experiment quickly without setup confusion.

What you will finish with: a working local model chat, plus a clear understanding of why each llmost feature exists.

## 1) Start Fast (First 2 Minutes)

Run:

```bash
./bin/llmost
```

On `Home`:
- `Enter` starts quick-start (installs runtime if needed, downloads starter model, starts gateway)
- `Enter` again jumps to `Chat`

Why this exists:
- quick-start removes the “which runtime/model do I pick first?” problem.
- you can test value first, then tune details later.

## 2) Simple Mental Model

- `Model`: the weights that generate answers.
- `Runtime`: the engine that runs that model (mlx-lm, ollama-mlx, llama.cpp, etc).
- `Gateway`: one stable OpenAI-compatible local API in front of your runtime.
- `Tuning`: behavior knobs for chat/serve/logging.

Why this exists:
- you can switch models/runtimes while keeping one predictable API endpoint for apps and scripts.

## 3) Why Each Tab Exists

- `Home`: shortest path to first response and system summary.
- `Models`: scan/import/download/register model assets.
- `Serve`: lifecycle controls (`start`, `stop`, `restart`, `unload`) for safe runtime transitions.
- `Setup`: host/port/python/backends configuration in one place.
- `Tuning`: scoped settings (`serve.*`, `chat.*`, `runtime.*`, `logs.*`) so intent is explicit.
- `Advisor`: optional remote planning guidance; local operation still works without it.
- `Chat`: terminal chat loop for quick verification and iterative prompting.
- `Use`: copyable API examples once service is running.
- `Logs`: install/runtime/gateway troubleshooting without leaving the TUI.

### Setup tab behavior for Python-backed runtimes

- For `mlx-lm`, `mlx-openai-server`, and `vllm-mlx`, llmost checks host Python first.
- If no compatible host Python is found, Setup asks for consent (`Y/N`) before creating a managed `.venv` with `uv`.
- Setup runtime discovery is shared across all runtimes and now shows the resolved source and hint when something is stale.
- Setup shortcuts:
  - `S`: status-only runtime rescan
  - `Shift+S`: deep runtime verification

Why this exists:
- host-first avoids surprising local environment changes.
- managed fallback keeps first-run reliable when host Python is missing/incompatible.

## 4) API Modes: Optimized vs Raw

llmost offers two API modes:

- Optimized chat endpoints:
  - `/v1/chat/completions`
  - `/api/chat`
  - include llmost chat reliability improvements (model/runtime-friendly defaults and response cleanup).

- Raw passthrough endpoints:
  - `/v1/chat/completions/raw`
  - `/api/chat/raw`
  - forward request/response unchanged to upstream runtime (no llmost chat modifiers).

Why this exists:
- optimized mode helps first-time users get usable chat replies quickly.
- raw mode helps advanced users benchmark and integrate with exact upstream behavior.

## 5) Advisor and Advanced Controls

Advisor:
- configure endpoint/token in `Advisor` tab
- validate status and inspect failures in `Logs`
- local serving/chat continue even if advisor is disabled or unreachable

Advanced operations:

```bash
./bin/llmost runtime-check
./bin/llmost cleanup-ghosts
./bin/llmost tune show
./bin/llmost python status
./bin/llmost python check
```

What these commands validate:
- host Python candidates and compatibility across all Python-dependent subjects
- whether fallback is needed
- readiness expectations for runtime installers and Hugging Face helper flow

## 6) Local-First Security

Default:
- bind host is `127.0.0.1`
- local usage works without external auth setup

When exposed beyond loopback:
- bearer token auth is required by default for protected endpoints
- optional private-subnet override exists for trusted LAN scenarios

See [Security](security.md) for exact policy details.

## 7) Recovery Playbooks

Service mismatch or stuck state:

```bash
./bin/llmost doctor
./bin/llmost status
./bin/llmost cleanup-ghosts
```

Port issues:
- use auto-port reassignment when prompted
- verify active URLs in `Home`/`Use`

Toolchain issues:
- Python resolution/compatibility: `python status`, `python check`
- Metal/Xcode issues: see [Troubleshooting](troubleshooting.md)

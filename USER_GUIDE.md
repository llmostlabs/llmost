# llmost User Guide (Binary Bundle)

This guide documents workflows supported by the `./bin/llmost` binary in this repository.

## 1) First Run

```bash
./bin/llmost
```

From `Home`:
1. Press `Enter` once to run quick-start (runtime install if needed + starter model pull + gateway start).
2. Press `Enter` again to jump to `Chat`.

If quick-start cannot proceed, use:

```bash
./bin/llmost doctor
```

## 2) Core TUI Workflow

- `Setup`: host/port, models root, backend install status, Python checks
- `Models`: scan/import/pull/register models
- `Serve`: start/stop/restart/unload runtime + gateway lifecycle
- `Tuning`: scoped settings grouped by serve/chat/runtime/logs
- `Chat`: terminal chat against running gateway model
- `Use`: API snippets and usage helpers
- `Logs`: backend/install/gateway logs
- `Advisor`: optional remote advisor configuration/status

## 3) Runtime and Model Lifecycle

### List and inspect

```bash
./bin/llmost status
./bin/llmost ports
./bin/llmost list-models
./bin/llmost list-backends
```

### Start and stop

```bash
./bin/llmost serve --model-id <id> --runtime mlx-lm
./bin/llmost stop
```

### Clean up stale process records

```bash
./bin/llmost cleanup-ghosts
```

## 4) Python Reliability (Host-First)

llmost resolves Python with host-first behavior, then managed fallback if required.

### Check interpreter compatibility

```bash
./bin/llmost python status
./bin/llmost python check
```

### Pin a preferred interpreter

```bash
./bin/llmost python use /opt/homebrew/bin/python3.12
```

### Clear preference

```bash
./bin/llmost python clear-preference
```

## 5) Tuning (Scoped Keys)

Use scoped keys so intent is explicit:
- `serve.*` affects runtime serving behavior
- `chat.*` affects chat payload behavior
- `runtime.*` affects runtime policy/selection behavior
- `logs.*` affects log viewer behavior

Examples:

```bash
./bin/llmost tune show
./bin/llmost tune set serve.context_length 8192
./bin/llmost tune set chat.temperature 0.2
./bin/llmost tune set chat.top_p 0.95
./bin/llmost tune set chat.thinking_mode off
./bin/llmost tune reset all
```

## 6) API Usage

When gateway is running, use OpenAI-compatible endpoints on your configured host/port.

Example:

```bash
curl -s http://127.0.0.1:8787/v1/chat/completions \
  -H 'Content-Type: application/json' \
  -d '{"model":"local-model","messages":[{"role":"user","content":"Reply with one short sentence."}]}'
```

For non-loopback exposure, include bearer auth header.

## 7) Troubleshooting

### Gateway connection refused

```bash
./bin/llmost status
./bin/llmost ports
./bin/llmost logs
```

### Runtime appears running but TUI disagrees

```bash
./bin/llmost runtime-check
./bin/llmost cleanup-ghosts
./bin/llmost status
```

### Backend install issues

```bash
./bin/llmost logs
./bin/llmost doctor
```

### Python/backend mismatch

```bash
./bin/llmost python status
./bin/llmost python check
```

## 8) Advisor and Advanced Features

### Advisor tab

- configure optional remote advisor endpoint/token
- check advisor connection status from within TUI
- inspect advisor failures in `Logs` without leaving the app

Advisor is optional. Local model serving and chat continue to work without it.

### Advanced operations

```bash
./bin/llmost runtime-check
./bin/llmost cleanup-ghosts
./bin/llmost tune show
./bin/llmost tune set serve.context_length 8192
./bin/llmost tune set chat.thinking_mode off
```

Use these when you need stronger runtime verification, stale-state cleanup, or more controlled model behavior.

## 9) Safety Notes

- Prefer loopback host for local-only usage.
- Use bearer token when binding to LAN/public interfaces.
- Stop one runtime before starting another runtime for the same model flow.

# llmost (Binary Bundle)

Run local models from a terminal UI with a local OpenAI-compatible gateway.

This repository is a binary distribution bundle centered on `./bin/llmost`.

## Quick Start

```bash
./bin/llmost
```

On `Home`:
1. Press `Enter` to install runtime (if needed), pull the starter model, and start the gateway.
2. Press `Enter` again to jump to `Chat`.

## What This Binary Offers

- Terminal UI tabs: `Home`, `Models`, `Serve`, `Setup`, `Tuning`, `Advisor`, `Chat`, `Use`, `Logs`
- Local OpenAI-compatible API gateway
- Runtime install/start/stop from TUI and CLI
- Model scan/import/pull/register lifecycle
- Scoped tuning via `llmost tune ...`
- Python host-first resolution with managed fallback support
- Diagnostics: doctor/status/ports/runtime checks/logs

<img width="1097" height="613" alt="Screenshot 2026-04-11 at 11 57 05 PM" src="https://github.com/user-attachments/assets/32f6fb81-18a2-4e70-b51e-48b26ec7fd5b" />

## Included Docs

- User guide: `docs/user_guide.md` [link](docs/user_guide.md)
- Troubleshooting: `docs/troubleshooting.md` [link](docs/troubleshooting.md)
- Security: `docs/security.md` [link](docs/security.md)


## Essential Commands

```bash
./bin/llmost
./bin/llmost --help
./bin/llmost doctor
./bin/llmost status
./bin/llmost ports
./bin/llmost stop
./bin/llmost cleanup-ghosts
./bin/llmost logs
```

## Python Reliability Commands

llmost prefers host Python and only falls back to managed setup when needed.

```bash
./bin/llmost python status
./bin/llmost python check
./bin/llmost python use /absolute/path/to/python3
./bin/llmost python clear-preference
```

## Tuning Commands (Scoped)

```bash
./bin/llmost tune show
./bin/llmost tune set serve.context_length 8192
./bin/llmost tune set chat.temperature 0.2
./bin/llmost tune set chat.thinking_mode off
./bin/llmost tune reset chat.temperature
```

## Security Defaults

- default bind host is loopback (`127.0.0.1`)
- local-first workflow requires no cloud account
- when exposing outside loopback, use bearer auth

Example:

```bash
./bin/llmost serve --model-id <id> --host 0.0.0.0 --port 8787 --bearer-token '<strong-token>'
```

## Bundle Notes

- Binary path: `./bin/llmost`
- Runtime/model state lives under `./config` and `./var`
- Bundled vendor components are under `./vendor`

## License

Apache-2.0. See `LICENSE`.

# Troubleshooting

Who this is for: users when startup, chat, runtime health, or security config does not behave as expected.

What you will finish with: concrete diagnosis and recovery steps.

## First Diagnostic Pass

```bash
./bin/llmost doctor
./bin/llmost status
./bin/llmost ports
```

## Gateway Health Failures

Symptoms:
- `connection refused`
- health endpoint unavailable

Actions:
1. `./bin/llmost stop`
2. `./bin/llmost cleanup-ghosts`
3. relaunch from Home `Enter`

## Chat Replies Are Empty Or Placeholder-Only

Symptoms:
- short placeholder response like `**`
- response visible as metadata instead of answer text

Actions:
1. verify gateway is healthy (`/health`)
2. run a quick API check:

```bash
./bin/llmost test-api --plain --prompt "Reply with one short sentence."
```

3. in `Tuning`, set `chat.thinking_mode` to `off` for problematic models
4. retry on a starter model to isolate model-specific behavior

## 401 Unauthorized On LAN/Public

If host is non-loopback (`0.0.0.0`), protected APIs require bearer by default.

Check:
- request includes `Authorization: Bearer <token>`
- token matches configured llmost token
- `security_allow_private_subnet_without_bearer` was not enabled unintentionally

## Port Ownership Mismatch

```bash
./bin/llmost ports
ps -Ao pid=,command=
```

Only stop processes owned by your current llmost instance unless intentionally doing broader cleanup.

## Python Issues

If backend install fails due to Python mismatch:
1. run:

```bash
./bin/llmost python status
./bin/llmost python check
```

2. for Python-dependent runtime/tool flows:
- install a compatible host Python (3.11+, with backend-specific caps where applicable), or
- approve managed fallback when prompted so llmost can create `.venv` via `uv`.

3. for Hugging Face pulls:
- llmost auto-bootstraps helper tooling into `.llmost-tools/hf` (no `--break-system-packages` needed).

4. in Setup tab, use:
- `S` for runtime status rescan
- `Shift+S` for deep runtime verify

5. if `vllm-metal` fails with stale embedded venv paths:
- remove `vendor/vllm-metal/.venv-vllm-metal`
- reinstall `vllm-metal` from Setup or:

```bash
./bin/llmost install-backend vllm-metal
```

## Xcode and Metal Toolchain Issues (SwiftLM)

If `metal` is missing or not executable:

```bash
xcode-select -p
xcodebuild -runFirstLaunch
xcodebuild -downloadComponent MetalToolchain
xcrun -sdk macosx metal -v
```

If asset download fails, retry MetalToolchain download.

## Logs

Use `Logs` tab for latest install/action logs.

CLI:

```bash
./bin/llmost logs all
```

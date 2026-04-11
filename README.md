# llmost

## `llmost` helps you run local models without needing to learn the usual stack of runtimes, ports, and launch commands first.

This release bundle is meant for the simplest path:
- open the TUI
- press `Enter`
- let `llmost` install what it needs
- press `Enter` again to start chatting

Service model:
- one installed copy of `llmost` manages one active local service at a time
- closing the TUI does not stop that service
- a separate install in another directory can run its own service if it uses different ports

## Why Try It

If you have been curious about local models but keep running into:
- “I do not know which runtime to install”
- “I do not know what command starts the server”
- “I do not know what port to use”
- “I do not want to hand-wire an OpenAI-compatible endpoint”

that is the problem `llmost` is trying to remove.

## What You Get

<img width="785" height="565" alt="Screenshot 2026-04-08 at 8 43 18 PM" src="https://github.com/user-attachments/assets/4ea3b8c4-f4c3-419d-8fb4-571b3385d970" />


- a terminal UI for setup, models, serve, tuning, chat, use, and logs
- a guided first-run path with a suggested starter model
- automatic runtime install when the recommended backend is missing
- a local gateway you can point other tools at
- terminal chat built in
- browser chat built in on its own local URL
- `doctor`, `status`, and cleanup commands for recovery

## Install

If you just want to try it, use one of these two paths.

Python requirement:
- Python 3.11 or newer is recommended for runtime installation
- if your machine only has an older Python 3 install, upgrade Python first before expecting backend installs to work cleanly

If you plan to build the bundled SwiftLM from source (`vendor/SwiftLM/build.sh`):
- full Xcode is required (Command Line Tools alone are not enough)
- verify with `xcrun --find metal`
- if needed, switch with `sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer`
- run Xcode first-launch setup: `sudo xcodebuild -runFirstLaunch`
- if `metal` is found but not runnable, run `sudo xcodebuild -downloadComponent MetalToolchain`
- if Metal Toolchain download fails with catalog fetch errors, retry `xcodebuild -downloadComponent MetalToolchain`

### Option 1: Download The ZIP

Download:

- [llmost release ZIP](https://github.com/llmostlabs/llmost/archive/refs/heads/llmost-initial-release.zip)

For future release branches, GitHub ZIP downloads follow this pattern:

```text
https://github.com/llmostlabs/llmost/archive/refs/heads/<branch-name>.zip
```

So if a later release branch is named `llmost-v0.2.0`, the ZIP would be:

```text
https://github.com/llmostlabs/llmost/archive/refs/heads/llmost-v0.2.0.zip
```

Then in Terminal:

```bash
cd ~/Downloads
unzip llmost-initial-release.zip
cd llmost-llmost-initial-release
./bin/llmost
```

### Option 2: Clone The Repo

If you already use Git:

```bash
cd ~/src/github.com
git clone --branch llmost-initial-release https://github.com/llmostlabs/llmost.git
cd llmost
./bin/llmost
```

## First Run

On first launch, `llmost` is designed to keep the next step simple:

- open the app
- press `Enter`
- let it install the recommended runtime if needed
- let it download the suggested starter model if needed
- press `Enter` again to start chatting

You do not need to manually:
- choose a port first
- learn a backend launch command
- wire up an OpenAI-compatible endpoint yourself

## Fastest First Run

Run:

```bash
./bin/llmost
```

Then:

1. Press `Enter` on the `Home` tab.
2. `llmost` installs the recommended runtime if needed.
3. `llmost` downloads the suggested starter model if needed.
4. `llmost` starts the local gateway and browser chat.
5. Press `Enter` again to jump straight into terminal chat.

That is the main onboarding path:
- one `Enter` to get the service running
- one more `Enter` to start chatting

## Browser Chat Too

When the service is running, `llmost` also exposes a small browser chat UI on a local port.

You can use that to:
- sanity-check the model in a browser
- try prompts without staying in the terminal chat tab
- reset the conversation easily and start over

The TUI shows the browser chat URL on:
- `Home`
- `Serve`
- `Use`

## Supported CLI Commands In This Bundle

These commands are intended to work in this binary release:

```bash
./bin/llmost
./bin/llmost doctor
./bin/llmost init
./bin/llmost scan-models
./bin/llmost pull-model --list-samples
./bin/llmost status
./bin/llmost stop
./bin/llmost cleanup-ghosts
./bin/llmost cleanup-ghosts --all-current-root
./bin/llmost tune show
./bin/llmost tune set serve.context_length 8192
./bin/llmost tune set chat.temperature 0.2
./bin/llmost tune reset chat.temperature
./bin/llmost tune reset all
./bin/llmost tune bench compare --models 2,5 --runtime-idx 1,6 --rounds 1 --strict
```

Useful recovery commands:
- `./bin/llmost doctor`
  - shows Python/runtime readiness, `instance_id`, runtime state, and managed processes for this install root
- `./bin/llmost status`
  - prints machine-readable runtime state
- `./bin/llmost cleanup-ghosts`
  - removes orphaned managed backend processes
- `./bin/llmost cleanup-ghosts --all-current-root`
  - one-time sweep of all `llmost`-managed processes for this install root
- tuning is hard-switched to `./bin/llmost tune ...` scoped keys
  - legacy `compare-runtimes` and `tune-mlx` entrypoints are removed

## One Service Per Install

`llmost` tracks its running service by install instance.

That means:
- one copy of `llmost` keeps track of one active gateway/backend pair
- starting a different model from that same copy replaces the existing one cleanly
- the app does not need to own the whole machine to work correctly

If you intentionally keep a second `llmost` install elsewhere, it can run separately as long as:
- it has its own ports
- it has its own config/runtime state

So the rule is:
- one active service per installed `llmost` instance
- not necessarily one active service for the whole computer

Operational notes:
- each install has its own `instance_id`
- older same-root daemons are still detected for compatibility
- if several managed daemons exist for the same install root, `llmost` will warn rather than guess

## Good Fit

Best fit right now:
- Apple Silicon macOS
- users who want the quickest path to a working local model
- users who want a local OpenAI-compatible base URL without manual setup

## Included In This Bundle

- `bin/llmost`
- `config/app_settings.json`
- `LICENSE`

This branch is a runnable binary release bundle, not the full source repository.

## Notes

- `llmost` writes runtime state and logs locally as it runs
- the default gateway is local-only on `127.0.0.1`
- first run may download a runtime and a starter model
- this branch is a binary release bundle, so source-only repo scripts are not part of the supported flow here

## License

Apache-2.0. See `LICENSE`.

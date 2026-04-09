# llmost

## `llmost` helps you run local models without needing to learn the usual stack of runtimes, ports, and launch commands first.

This release bundle is meant for the simplest path:
- open the TUI
- press `Enter`
- let `llmost` install what it needs
- press `Enter` again to start chatting

## Why Try It

If you have been curious about local models but keep running into:
- “I do not know which runtime to install”
- “I do not know what command starts the server”
- “I do not know what port to use”
- “I do not want to hand-wire an OpenAI-compatible endpoint”

that is the problem `llmost` is trying to remove.

## What You Get

<img width="782" height="551" alt="Screenshot 2026-04-08 at 8 25 41 PM" src="https://github.com/user-attachments/assets/a15438a5-d554-4b17-94f4-4fdd0e7c8578" />

- a terminal UI for setup, models, serving, chat, and logs
- a guided first-run path with a suggested starter model
- automatic runtime install when the recommended backend is missing
- a local gateway you can point other tools at
- terminal chat built in
- browser chat built in on its own local URL

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

## License

Apache-2.0. See `LICENSE`.

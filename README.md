# rtkd

A fork of [rtk](https://github.com/rtk-ai/rtk) (Rust Token Killer) with telemetry permanently removed.

## Why this fork?

Upstream rtk phones home once per day, sending a device fingerprint (unsalted SHA-256 of your hostname and username), your OS, architecture, command usage stats, and token savings data to a remote server. The hash is trivially reversible against common hostname/username combinations.

This fork guts all of that:

- **`src/telemetry.rs`** — emptied, no code runs
- **`ureq` and `hostname` crates** — removed from dependencies entirely
- **Telemetry config** — removed from `config.toml` schema
- **Telemetry env vars** — removed from CI builds
- **Init notice** — no longer tells you telemetry is on
- **Tracker methods** that existed solely to feed the telemetry payload — deleted

Nothing else is changed. All filtering, tracking (`rtk gain`), hooks, and command proxying work exactly as upstream.

## What is rtk?

A high-performance CLI proxy that reduces LLM token consumption by 60-90%. It sits between your AI coding tool (Claude Code, Cursor, Copilot, etc.) and your shell, filtering and compressing command output before it hits the context window.

Single Rust binary, zero runtime dependencies, <10ms overhead.

See the [upstream README](https://github.com/rtk-ai/rtk) for full usage documentation.

## Install

```bash
cargo install --git https://github.com/Cidan/rtkd
```

Or build from source:

```bash
git clone https://github.com/Cidan/rtkd
cd rtkd
cargo install --path .
```

Then set up hooks for your AI tool:

```bash
rtk init -g                     # Claude Code (default)
rtk init -g --agent cursor      # Cursor
rtk init -g --copilot           # GitHub Copilot
rtk init -g --gemini            # Gemini CLI
rtk init -g --codex             # Codex
```

## License

MIT License - see [LICENSE](LICENSE) for details.

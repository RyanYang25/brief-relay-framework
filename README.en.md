# BRIEF Relay Framework (BRF) v1.1

A **cross-agent, cross-platform, portable** method for relaying project context. Its core is a single BRIEF that acts as both the **entry point and the evolution engine** of a project. Combined with `@context` contract headers, it makes the "who-produces / who-consumes / what's-next" relationship explicit—so any new AI agent (WorkBuddy / Trae / Qoderwork / etc.) can pick up the project folder and take over with zero cost.

## Why this exists

- Agent platforms are switched frequently; their built-in memory doesn't interoperate.
- When a conversation is interrupted, the agent can't act on its own—context is easily lost.
- Project knowledge scattered in chats is hard to retain and reuse.

## Core mechanisms

1. **BRIEF = entry point + evolution engine**: lets a new agent onboard fast, and keeps evolving as the project progresses.
2. **`@context` contract headers**: each context file declares its role / maintainer / reader / update trigger / upstream / downstream relay.
3. **Session Resume Protocol**: whenever an agent re-takes the project, it first reads BRIEF + current, compares timestamps, then reconciles.
4. **Incremental checkpoint discipline**: write to files the moment a step finishes; worst case on interruption is losing only the last in-flight step.

## Files

| File | Description |
|------|-------------|
| `简报接力-v1.1.md` | Full method specification (Chinese, current version) |
| `简报接力-v1.0.md` | Full method specification (Chinese, historical version) |
| `Brief-Relay-Framework-v1.1-en.md` | Full method specification (English, current, synced with Chinese) |
| `Brief-Relay-Framework-v1.0-en.md` | Full method specification (English, historical version) |
| `template/` | Copy-paste ready starter files (Chinese) |
| `template/使用说明.md` | How to use the template (Chinese) |
| `template/en/` | English starter files (kept in sync with the Chinese template) |
| `template/en/README.md` | How to use the English template |
| `README.md` | This README's Chinese version |
| `CONTRIBUTING.md` | Contribution guide |
| `LICENSE` | MIT License |

> The specification is now available in **both Chinese and English**, in two versions: **v1.1 (current)** — `简报接力-v1.1.md` (Chinese) / `Brief-Relay-Framework-v1.1-en.md` (English); **v1.0 (historical)** — `简报接力-v1.0.md` (Chinese) / `Brief-Relay-Framework-v1.0-en.md` (English). Chinese is authoritative; the English spec is kept in sync.

## Quick start

1. Copy `template/` into your project root.
2. Open `BRIEF.md` and replace the `{placeholders}` with your real content.
3. Whenever state changes, follow the "Update SOP" at the bottom of `BRIEF.md` and update linked files together—**don't edit BRIEF.md alone**.

See `template/使用说明.md` for details.

## Design principles

- **Pure Markdown + HTML comments**—no dependency on any platform-specific capability (no YAML frontmatter, no proprietary memory, no "interruption callback").
- Copy the whole project folder to another agent platform; the new agent reads the contract header at the top of BRIEF and takes over. That is the hard requirement of this framework.

## License

MIT — see `LICENSE`.

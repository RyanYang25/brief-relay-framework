# Template Usage (English)

This folder provides a **ready-to-use template** for the BRIEF Relay Framework (BRF). Copy it into any project root and fill the placeholders—no need to read the full spec first.

## One-minute start

1. Copy the whole `template/en/` folder into your project root (or just its files):

   ```
   your-project/
   ├── BRIEF.md
   ├── project-context/
   │   ├── context/
   │   │   ├── current.md
   │   │   └── standards.md
   │   ├── references/
   │   ├── handoffs/
   │   ├── .working/
   │   └── changelog.md
   └── {meeting-notes-dir}/
   ```

2. Open `BRIEF.md` and replace `{project name}` etc. with your real content.
3. Fill your meeting-notes directory name into `{meeting-notes-dir}`.
4. On state changes, follow the "Update SOP" at the bottom of `BRIEF.md` and update linked files together—**don't edit BRIEF.md alone**.

## File roles

- `BRIEF.md`: single entry + evolution engine. A new agent / platform switch reads only this to take over.
- `context/current.md`: current state, tasks, recent decisions (with rationale). Updated most often.
- `context/standards.md`: output specs, workflow, past pitfalls. Append-only.

## The three-piece set (relay guarantee)

- **@context contract header**: an HTML comment at the top of each file declaring role & relay (who produces, who consumes, what's next).
- **Relay baton (downstream)**: the `downstream` field tells the agent which file to touch next.
- **Session Resume Protocol**: on platform switch / after interruption, read BRIEF + current, compare times, then work—no context lost.

## Go deeper

- Full spec: `../简报接力-v1.0.md` (Chinese)
- English intro: `../README.en.md`
- License: `../LICENSE` (MIT)

# Template Usage (English)

This folder provides a **ready-to-use template** for the BRIEF Relay Framework (BRF). Copy it into any project root and fill the placeholders—no need to read the full spec first.

## One-minute start

1. Copy the whole `template/en/` folder into your project root (or just its files):

   ```
   your-project/
   ├── BRIEF.md
   ├── deploy-manifest.md
   ├── project-context/
   │   ├── context/
   │   │   ├── current.md
   │   │   └── standards.md
   │   ├── references/
   │   ├── handoffs/
   │   ├── .working/
   │   ├── journal/
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
- `deploy-manifest.md`: manifest of out-of-workspace deployment artifacts (live path / purpose / workspace copy / restore method / contains-credentials + a "Not covered" section). **Keep it and write "none + determination date" even when no artifacts exist**; sensitive file — never publish it, never mirror it wholesale into a cloud drive.
- `project-context/journal/`: the session journal (v1.15) — one file per session, append-only five lines (time span / what was done / decisions / artifact paths / open items and next step). Its only purpose is to make write-back cheap enough that a long conversation cannot squeeze it out. **It is a log, not the truth**: distil it into `current.md` / `changelog.md` at close-out and merge it into `journal/archive/` monthly; never place it under `.working/` (auto-cleaned). Paired requirement: end every delivery reply with one write-back declaration line (`Write-back: <file> <time> · N open items`); a missing line counts as not delivered.

## The three-piece set (relay guarantee)

- **@context contract header**: an HTML comment at the top of each file declaring role & relay (who produces, who consumes, what's next).
- **Relay baton (downstream)**: the `downstream` field tells the agent which file to touch next.
- **Session Resume Protocol**: on platform switch / after interruption, read BRIEF + current, compare times, then work—no context lost.

## Go deeper

- Full spec: `../简报接力-v1.0.md` (Chinese)
- English intro: `../README.en.md`
- License: `../LICENSE` (MIT)

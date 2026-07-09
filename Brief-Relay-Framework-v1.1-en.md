# Brief Relay Framework (BRF) v1.1

> **One-line positioning**: Use one continuously-evolving **Project Brief (BRIEF)** as the single entry point, paired with a three-part kit — **contract header + relay baton + recovery protocol** — so that any AI Agent platform can take over at zero cost, migrate cross-platform, and never lose project context.
>
> **Why the name**: "Brief" = the core artifact (BRIEF.md); "Relay" = the cross-Agent / cross-platform / cross-session context-passing logic — who produces, who consumes, who moves next, declared explicitly, like a baton passed hand-to-hand.
>
> **Scope**: Any project requiring multi-file collaboration and state tracking — consulting, writing, R&D, operations, etc. Domain-agnostic; not bound to any IDE or Agent platform.
>
> **Version note**: This is v1.1. v1.0 was the first formally-named public release; v1.1 adds "XII. Tool & Script Dependency Portability (cross-machine)", distilled from real cross-machine deployment pitfalls (NAS / cloud drives exclude hidden folders by default).

---

## Core Philosophy

### I. BRIEF is an "entry point + evolution engine", not a static signboard

`BRIEF.md` plays two roles at once:

1. **Access point** — when any new Agent / new session / platform switch occurs, reading this single file is enough to get started;
2. **Evolution engine** — as the project advances, triggered periodically by automation and on user request, `BRIEF` and its linked files keep updating, so context grows together with the project.

> **Why not downgrade to AGENTS.md**: `AGENTS.md` is a "project-level static entry" convention from the coding-Agent community — the platform reads it once at the start of a new session to tell you "the project looks like this", then falls silent; it has no self-evolution capability. BRIEF is alive by nature: it handles both handoff and evolution. Reducing BRIEF to AGENTS.md would be cramming a living system back into a static signboard — a downgrade. This framework does not depend on AGENTS.md; BRIEF is self-contained with all handoff and evolution information.

### II. Portability is the first constraint

The whole mechanism uses only plain Markdown + HTML comments, **depending on no platform-specific capability** (no YAML frontmatter dependency, no platform-private memory dependency, no "interrupt callback" dependency). The test: copy the entire project folder to another Agent platform (WorkBuddy / Trae / Qoderwork, etc.), and a new Agent can take over just by reading the contract header at the top of BRIEF — this is the framework's hard metric.

---

## Prompt Body (generic scaffolding template)

> Copy the following to any Agent, replacing `{project name}`, `{project description}`, and optional placeholders to quickly scaffold the project base.
> Applies to consulting, writing, R&D, operations, and any project needing multi-file collaboration and state tracking.

---

> **Meta-instruction (Guardrail)**: The generated `BRIEF.md` MUST contain a defensive comment at the top, to prevent an Agent or later collaborator from modifying only BRIEF.md while ignoring the linked files. Reference format:
>
> ```
> > **📌 If you receive an instruction like "update brief / update project status / sync progress":**
> > Do NOT change only this file! First read the "Update SOP" section of this file, find the matching scenario,
> > and follow the SOP to update current.md / standards.md / PROJECT_GUIDANCE.md and other files in order.
> ```

You are a project management assistant. Please scaffold a `project-context` working directory for project **{project name}**.

**Project background:** {project description, 2-3 sentences stating goals, deliverables, key constraints}

### I. Directory Structure

```
{project root}/
├── BRIEF.md                  ← Project Brief (single entry + evolution engine, at root, with @context-root contract header)
├── {meeting-notes dir}/      ← Meeting/communication records (default B02 会议记录/, customizable, see encoding note below)
├── project-context/
│   ├── context/
│   │   ├── current.md        ← Current status and in-progress tasks (updated most often, with @context contract header)
│   │   └── standards.md      ← Work standards and output specs (append-only, with @context contract header)
│   ├── references/           ← Raw materials (data, documents, references)
│   ├── handoffs/             ← Historical outputs (completed deliverables)
│   ├── .working/             ← Agent temp files (auto-cleaned, not version-controlled)
│   ├── tools/                ← Project's own tools/scripts (plain files, cross-platform readable, carried with the project)
│   ├── changelog.md          ← Structure-change log (records only file add/remove, directory adjustments)
│   └── PROJECT_GUIDANCE.md   ← (optional) detailed encyclopedia recording historical decisions and full task list
```

**Directory coding (user's personal habit; the Agent must recognize and match it, not force a recommendation)**: The user typically classifies root folders with handwritten letter prefixes (e.g. `A01 中间文档/`, `B02 会议记录/`, `C01 同事成果/`, `C02 参考资料/`). This coding is manually established by the user; the Agent **must not invent or recommend a new coding scheme**, but rather:

- **On first contact with the project**: scan the root directory, recognize existing coding prefixes and their meaning (e.g. `A`=process / `B`=communication / `C`=reference);
- **When creating or adjusting folders**: follow the user's existing prefix rules and numbering habits, keeping continuity (e.g. a new reference-type directory uses `C03 …`);
- **In the BRIEF.md "File Organization" section**: faithfully annotate the meaning of the coding the user adopted, without prescribing new rules.

> **Note**: Meeting notes are **not** placed inside `project-context/`. It is recommended to create a separate `{meeting-notes dir}/` at the project root (default `B02 会议记录/`), linked via the BRIEF.md file index. Meeting notes are large and update frequently; keeping them separate avoids polluting the project-context structure.

### II. Contract Header Spec (@context) — the grip of the relay

Each context file places an `<!-- @context -->` HTML comment block at the top, declaring the file's role and relay relationship. This is the grip of the "relay baton": a new Agent knows at a touch what the file is for, when to read it, when to modify it, and who to move next after modifying it.

**Design decision (why HTML comments instead of YAML frontmatter)**:

- YAML frontmatter needs a platform parser to be recognized; an HTML comment is a universal format any Agent that "reads file text" can see, best fitting the goal of "cross-platform, plain Markdown take-over";
- HTML comments **don't render and don't pollute display** — humans don't see this block when opening the file; it exists only when an Agent reads it.

**Required fields**:

| Field | Meaning |
|------|---------|
| `role` | This file's role in the project (e.g. "single entry point", "active context", "constraint layer") |
| `produced_by` | Who maintains this file (e.g. "Update SOP auto-maintained", "user manual") |
| `consumed_by` | Who should read this file (e.g. "all Agents must read at the start of every session") |
| `update_trigger` | Under what condition it must be updated (e.g. "after any status/decision/file-structure change") |
| `upstream` | Upstream dependency (which info this file depends on, e.g. BRIEF.md depends on current.md's status) |
| `downstream` | **Relay baton pointer** — after this file updates, who moves next (e.g. current.md → B01 live-course-notes/, standards.md) |

**BRIEF.md (@context-root) example**:

```
<!-- @context-root
role: Single project entry point + evolution engine
produced_by: Update SOP (auto/manual trigger)
consumed_by: All Agents and human successors, must read at the start of every session
update_trigger: After any status/decision/file-structure change
upstream: context/current.md (active status), context/standards.md (constraints)
downstream: context/current.md → context/standards.md → {meeting-notes dir}/
-->
```

**context/current.md (@context) example**:

```
<!-- @context
role: Current active context (activeContext)
produced_by: Update SOP scenario 2/3 auto-maintained
consumed_by: All Agents must read at the start of every session
update_trigger: After task-status change / new decision / blocker discovered
upstream: BRIEF.md (scope and constraints), standards.md (how-to)
downstream: {meeting-notes dir}/, standards.md (pitfall deposits), PROJECT_GUIDANCE.md (important decisions)
-->
```

**context/standards.md (@context) example**:

```
<!-- @context
role: Constraint layer (output specs / collaboration flow / historical pitfalls)
produced_by: Update SOP scenario 5 (append-only)
consumed_by: All Agents must read before producing output
update_trigger: When adding a new spec or pitfall record
upstream: BRIEF.md (project profile and constraints)
downstream: {meeting-notes dir}/_template.md, references/, handoffs/
-->
```

> **Relay effectiveness validation**: Test with "a brand-new sub-Agent (no historical memory, forbidden from reading the platform memory directory) taking over using only BRIEF + @context blocks" — if it passes, the project is self-describing and migration-ready.

### III. Build Requirements

**BRIEF.md (Project Brief = entry point + evolution engine)**

This is the single entry point of the whole directory and the core file that evolves with the project. Any Agent or human reads only this one file on first contact. Keep it within one page, containing these sections:

1. **Meta-instruction (Guardrail)**
   - Insert a defensive comment below the title (format see the Meta-instruction above)
   - Purpose: prevent the Agent from doing a surface update while missing linked files (e.g. current.md, PROJECT_GUIDANCE.md)
   - Also annotate: last-updated date, updater, reason for update

2. **Project Profile** (stable info, almost immutable)
   - Project name, goals, deliverables, key constraints, timeline
   - Stakeholders: use a table with ⭐ marking decision-authority level:

     ```
     | Role | Name | Decision authority |
     |------|------|--------------------|
     | Project lead | X | ⭐⭐⭐ highest |
     | Liaison | X | ⭐⭐ high |
     | Executor/advisor | X | ⭐ medium |
     ```

3. **File Organization** (actual directory tree)
   - Show the actual structure of the project root (not just project-context/)
   - Use an indented tree, annotating the meaning of the user's actual coding (the Agent must recognize existing rules first, not apply template presets)
   - Example: `├── A01 中间文档/`, `├── B02 会议记录/`, `├── C01 参考资料/` (use the user's reality)

4. **Current Status** (one-liner + pointer)
   - One sentence summarizing current progress, e.g. "Second-version proposal completed, awaiting client feedback"
   - Point to `context/current.md` for details

5. **File Index** (routing table)
   - List all files under context/, references/, handoffs/, {meeting-notes dir}/, one line of purpose per file
   - Sync this index when files are added/removed

6. **Reading Rules** (progressive disclosure + contract header explanation)
   - Mark which files must be read first (e.g. current.md), which read on demand (e.g. specific files under references/)
   - Explain the role of each file's top `<!-- @context -->` block, telling the successor that reading these blocks first is enough to grasp "how the project is organized, which file to touch each time" in 30 seconds
   - If PROJECT_GUIDANCE.md exists, mark it as the optional fourth layer "read on demand"
   - **Session Recovery Protocol** (see Chapter IX): handoff reliability does not depend on "auto-sync on interrupt", but on the recovery protocol + incremental persistence rules

> **Length trigger**: When BRIEF.md exceeds one page (~80 lines), migrate details other than stable info into PROJECT_GUIDANCE.md, keeping the entry lean.

**context/current.md (current status)**

Records active info, highest update frequency:

- In-progress task list (each item: task name, **status (see enum below)**, next action, blocker, owner)
- Recent decisions ( **decision content + basis/rationale + date** — the "basis/rationale" field is critical: the successor sees not just the conclusion but why it was made, avoiding mistaken changes or repeated discussion)
- Open issues
- Meeting-notes index (**store only "date + topic + ≤5 core points + file path", do not pile up full text or long paragraphs**)

> **Length trigger**: When current.md exceeds {N} lines (suggest 120), migrate historical task lists, expired decisions, and full meeting details into PROJECT_GUIDANCE.md; current.md keeps only active info.

**Task status enum (use uniformly)**:
`未开始 (Not started)` → `进行中 (In progress)` → `已完成 (Done)`; exception states: `已阻塞 (Blocked)` (must state reason), `已取消 (Cancelled)` (must state reason)

**context/standards.md (work standards)**

Records output specs and workflows, append-only:

- Document format requirements (style, templates, naming conventions)
- Quality standards (acceptance criteria, review points)
- Collaboration flow (role division, approval nodes)
- Historical pitfall records (what was done wrong, how it was fixed)

**references/ (raw materials)**

Stores project inputs: data files, background docs, client materials, competitive analysis, etc. Write one line of description per file in BRIEF.md's file index.

**handoffs/ (historical outputs)**

Stores completed deliverables. Name by version or date, e.g. `v1-draft.md`, `20260612-proposal-review.pptx`. Do not put the current version here; keep it at the project root or wherever you prefer.

**PROJECT_GUIDANCE.md (optional — detailed encyclopedia)**

Enabled when the project grows large and current.md gets too long. Positioned as "complete record of historical decisions", read on demand, not a daily entry.

- Records: complete timeline, historical decision details (time + source + content + **rationale**), full task list (including done and cancelled)
- Division of labor with changelog.md:
  - `changelog.md`: records only **file-level structure changes** (e.g. "added XX file", "deleted YY dir"), for maintainers
  - `PROJECT_GUIDANCE.md`: records **project-level business changes** (e.g. "party-group meeting decided XX", "scope adjusted to YY"), for everyone
- If not needed (small project), you may skip this file and use current.md + changelog.md

**changelog.md (change log)**

Records **file-level structure changes**, format:

```
## YYYY-MM-DD
- Change content (which file added/modified/deleted, reason)
```

> **Distinction**: Project business changes (e.g. "party-group meeting decided XX") go to `PROJECT_GUIDANCE.md` or `current.md`, not changelog.md. changelog.md is for maintainers and Agents, to understand the evolution of file structure.

### IV. Progressive Disclosure Principle

The whole directory follows a four-layer funnel (fourth layer optional):

- **Layer 1 BRIEF.md**: decide whether and how to use → quick judgment on whether to dig deeper
- **Layer 2 context/current.md**: decide what to do now → understand status and tasks
- **Layer 3 context/standards.md**: decide how to do it → check specs and standards
- **Layer 4 PROJECT_GUIDANCE.md (optional)**: look up historical decisions and full details → read on demand

Each layer answers only the question left by the layer above; do not pile the next layer's content onto the previous one.

### V. First-time Initialization Steps (Agent execution order)

1. Create root `BRIEF.md` (with Guardrail comment + **@context-root contract header** + project-profile skeleton)
2. Create `project-context/` and subdirs `context/`, `references/`, `handoffs/`, `.working/`
3. Create `context/current.md`, `context/standards.md`, `changelog.md` (empty skeletons, **each with an @context contract header**)
4. Create `{meeting-notes dir}/` (e.g. `B02 会议记录/`)
5. Confirm with the user: project goals, deliverables, key constraints, stakeholders; **recognize and follow the user's existing directory-coding habits** (if root already has `A01`/`B02`/`C01` prefixes, match directly, don't start a new scheme)
6. Backfill each BRIEF.md section, write the first changelog entry
7. If the project is expected to be large, pre-create the `PROJECT_GUIDANCE.md` skeleton

> **Cross-session memory (optional)**: If the Agent's runtime supports out-of-project memory (e.g. `.workbuddy/memory/`), you may additionally maintain a cross-session memory file recording user preferences and long-term project conventions, avoiding re-alignment each session. This file is not part of the project-context structure and does not affect the framework's "portability-first constraint" — even without it, the in-project files alone are enough to take over.

### VI. Reading Rules

When asked to work on the project:

1. First read BRIEF.md (including the top @context-root block) for global context and the file index
2. Then read context/current.md (including the @context block) for current status
3. As the task requires, read context/standards.md or specific files under references/ on demand
4. If querying historical decisions or the full task record, read PROJECT_GUIDANCE.md on demand (if present)
5. Do not read all files at once

### VII. Update Rules

| File | Update frequency | Update condition |
|------|------------------|------------------|
| BRIEF.md project profile | Almost immutable | Change only when project goals or constraints fundamentally shift |
| BRIEF.md current status | On every status change | Task done, task added, key decision |
| BRIEF.md file index | On every file add/remove | Important file/dir added or deleted |
| context/current.md | On every task advance | Task-status change, new decision, blocker discovered |
| context/standards.md | Append-only | New spec or pitfall record added |
| PROJECT_GUIDANCE.md | After every important event | Detailed historical decisions and full task list |
| changelog.md | On every file-structure change | File or directory added/modified/deleted |

> **⚠️ Note**: The above is the principle table of "what to change". For concrete steps see the "Update SOP" below — it tells the Agent "how to change, in what order, which files to link".

---

### VIII. Update SOP (operation guide — mandatory)

> When project status needs updating, operate per the matching scenario to ensure related files sync.
> When the Agent receives "update brief / update project status" instructions, it must first find the matching scenario, then follow the SOP — **do NOT change only BRIEF.md**.

#### Scenario 1: New meeting-notes processing

1. **Store meeting notes**
   - Store in the project-root `{meeting-notes dir}/` (default `B02 会议记录/`)
   - Naming convention see "File Naming Convention" below

2. **Update current.md** (main work)
   - Read `context/current.md`
   - Add an entry in "Meeting-notes index": date + topic + ≤5 core points + file path (**no full text**)
   - Update the status of "In-progress tasks" or add a task
   - If there is a new decision, add to "Recent decisions" (**record content + basis/rationale + date**)
   - If a problem is resolved, update "Open issues"

3. **Sync BRIEF.md**
   - Read `BRIEF.md`
   - Update the one-line summary in "Current Status"
   - Ensure consistency with current.md's "Current Status"
   - If the meeting notes need long-term reference, add a note in "File Index"

4. **If there is an important decision, update PROJECT_GUIDANCE.md**
   - Read `PROJECT_GUIDANCE.md` (if present)
   - Update the relevant section (timeline, task list, important decisions)
   - Add this change to the "Update Log"

#### Scenario 2: Task-status change

1. **Update current.md** (main work)
   - Find the task in "In-progress tasks"
   - Update task status (use the unified status enum)
   - If the task is done, move to archive or mark `已完成 (Done)`
   - If there is a new task, add to "In-progress tasks"

2. **If the status change affects the project globally, sync BRIEF.md**
   - If task completion/addition changes overall progress, update the one-line summary in "Current Status"

3. **If the task change involves an important decision, update PROJECT_GUIDANCE.md**
   - Update the corresponding task-list entry

#### Scenario 3: Important decision or requirement change

1. **Update current.md**
   - Add the new decision in "Recent decisions" (record time, source, content, **rationale**)
   - If the decision affects in-progress tasks, sync "In-progress tasks"

2. **Sync BRIEF.md**
   - If the decision affects project goals or constraints, update "Project Profile"
   - If the decision affects current status, update "Current Status"

3. **Update PROJECT_GUIDANCE.md**
   - Record decision details in the relevant section (time, source, content, rationale)
   - Add this change to the "Update Log"

#### Scenario 4: New deliverable

1. **Store deliverable**
   - Store in the corresponding directory by source (e.g. `handoffs/`, or the user's existing deliverable directory)
   - Naming convention see "File Naming Convention" below

2. **Update current.md**
   - If the deliverable corresponds to an in-progress task, update that task's status to `已完成 (Done)`
   - Add a deliverable reference in "Recent outputs" (if this section exists)

3. **Update BRIEF.md file index** (if the deliverable is important)
   - If the deliverable needs long-term reference, add a note in "File Index"

#### Scenario 5: Pitfall discovered or new spec added

1. **Update standards.md**
   - Add the new spec or pitfall record in the relevant section
   - **Note**: `standards.md` is append-only; do not delete existing content

2. **Notify the team** (optional)
   - If the pitfall affects other collaborators, notify in the project group or meeting notes

#### Scenario 6: User correction / Agent self-correction

> When the user points out a previous Agent error (e.g. wrong name, factual deviation, missing file), fix and leave a trace per this scenario.

1. **Locate and fix the erroneous file**
   - Use Edit to directly fix the wrong content in the target file (e.g. "Tang" → "Tang" in minutes)
   - If the error has spread to multiple files, fix each one; do not change only one place

2. **Annotate the fix in related files** (as appropriate)
   - If it is a key factual error, note it in current.md's "Recent decisions" or standards.md's "Pitfall records" to prevent recurrence
   - If it involves a long-term user preference (e.g. "head of planning dept is surnamed Tang"), write it to cross-session memory

3. **Sync BRIEF.md / PROJECT_GUIDANCE.md**
   - If the fix affects project status or key facts, link-update per Scenario 2/3

4. **Record to changelog.md** (if it involves file-structure change)

#### Scenario 7: Project archival / closure

1. **Update BRIEF.md**
   - Change "Current Status" to "Closed/Archived", noting the closure date and final deliverables
   - Clear the active task list or move it to archive

2. **Tidy handoffs/**
   - Move final deliverables into `handoffs/`, archive old versions or label "historical version"

3. **Update PROJECT_GUIDANCE.md** (if present)
   - Mark project status as "Closed", complete the final task list and decision timeline

4. **Clean .working/**
   - Delete temp scripts and intermediate products, keep the directory tidy

---

### IX. Session Recovery Protocol & Incremental Persistence Discipline (cross-platform handoff insurance)

> **Platform constraint (starting point)**: Current mainstream Agent platforms (WorkBuddy / Trae / Qoderwork, etc.) **cannot act on their own after a conversation interrupt** — the Agent process hangs on interrupt, and only wakes when the user restarts the session; it cannot rewrite BRIEF by itself while "no one wakes it". Therefore, phrasings like "auto full-sync BRIEF on abnormal session end" are **unimplementable** under this framework and must be redesigned as "trigger on recovery".

#### 9.1 Session Recovery Protocol (trigger point at "recovery")

Any Agent **re-taking over** this project (whether a new session, cross-platform switch, or post-interrupt recovery), the first step **must**:

1. Read `BRIEF.md` (including the @context-root block) and `context/current.md` (including the @context block);
2. Compare the last-updated times of both, confirm status consistency;
3. If inconsistency is found, or there is an unclosed in-flight task, first **reconcile** (confirm with the user if necessary), then start new work;
4. Do not assume "the last session ended normally" — treat file content as the sole authority.

> This protocol has passed real testing: a brand-new sub-Agent (no historical memory, forbidden from reading the platform memory directory) could correctly take over and produce the relay order using only BRIEF + @context blocks, proving the project is self-describing and portable.

#### 9.2 Incremental Persistence Discipline (root-cause governance)

- `BRIEF.md` / `context/current.md` are the project's **sole authoritative source of truth**;
- Upon completion of any task step, **immediately persist and update the corresponding file**; do not wait until session end to backfill;
- The conversation context (transcript) itself **has no authority** — the only thing truly lost is "what hasn't been written to a file yet, only lives in the conversation";
- As long as each step persists immediately, an interrupt at worst loses only the last in-flight step; after restart, just follow the `downstream` relay relationship in @context to continue. This is more reliable than "auto full-sync" because the latter also assumes the Agent is alive.

#### 9.3 Scheduled automation patrol (optional, not required)

Automations on platforms like WorkBuddy run independently on schedule and do not depend on the current conversation. You can set a periodic check "are BRIEF and current consistent", but it **does not know "just interrupted"** — it is only a mechanical patrol, of limited value and may create noise. This framework **does not adopt it as the main solution**, only as a nice-to-have, at the user's discretion.

---

### X. Agent Work Norms (mandatory)

> The following rules target the AI Agent, to prevent it from polluting the project structure.

1. **Root-directory protection**
   - The project root may only contain: BRIEF.md, project-context/, {meeting-notes dir}/, and directories explicitly approved by the user
   - The Agent **must not** create temp scripts, test files, or intermediate products in the root directory

2. **Temp-file management**
   - All Agent-generated files (temp scripts, test output, intermediate caches) go into `project-context/.working/`
   - Actively clean temp files after task completion; `.working/` is not version-controlled
   - If an intermediate product needs to be kept, ask the user first about where to store it (follow the user's existing process-doc directory, e.g. `A01 中间文档/`, don't create a new coding system)

3. **File-creation principle**
   - Create new files only when the user explicitly requests or the project SOP requires
   - Prefer editing existing files over creating new ones; current.md / PROJECT_GUIDANCE.md use incremental edits, **avoid full rewrites**

4. **Incremental-update discipline**
   - Use Edit for targeted paragraph updates on status changes; do not use Write to overwrite the whole file
   - Full rewrite only on major structural adjustment or explicit user request

---

### XI. File Naming Convention

> Unified date format, to avoid naming chaos among collaborators. Separator may be `-` or `_`, but must be consistent within the project.

| File type | Naming format | Example |
|-----------|---------------|---------|
| Meeting minutes | `YYYYMMDD-topic-minutes.md` | `20260624-internal-division-minutes.md` |
| Meeting verbatim | `YYYYMMDD-topic-verbatim.md` | `20260624-internal-division-verbatim.md` |
| Deliverable | `YYYYMMDD-description-version.ext` | `20260630-construction-plan-v3.docx` |
| Reference | `YYYYMMDD-description.ext` | `20260515-some-bureau-letter.pdf` |

> Date format unified as `YYYYMMDD` (8 digits) for easy time-sorting and retrieval. Keep meeting topics short to avoid over-long filenames.

---

### XII. Tool & Script Dependency Portability (cross-machine) (new in v1.1)

> **Starting point**: BRF's "portability-first constraint" requires not only that *documents* be cross-platform readable, but also that the *tools, scripts, and configs the project needs to actually run* travel with the project. Otherwise, switching machines or sync environments leaves the docs intact but the toolchain broken, and a successor is still stuck.

#### 12.1 General principles (BRF hard constraints)

1. **Dependencies travel with the project**: Tools, scripts, and configs the project needs must live inside the project folder — never rely on "this PC already has it installed" or "this platform has it globally configured".
2. **No hardcoded usernames in paths**: Scripts must not hardcode absolute username paths like `C:\Users\Ryan\…`; use relative paths, or expand at runtime via `os.path.expanduser("~")`.
3. **No platform-specific directories**: BRF binds to no platform, so a project must **not** create platform-private hidden dirs like `.workbuddy/skills/` or `.obsidian/` to hold generic capabilities. If the project needs its own tool scripts, put them uniformly in `project-context/tools/` (plain files, cross-platform readable, synced with the project).

#### 12.2 Cross-machine deployment pattern: non-hidden carry + local regenerate

When a project flows across machines via NAS / cloud-drive sync tools, most such tools **exclude hidden folders by default** (`.workbuddy/`, `.obsidian/`, etc. won't sync). This creates a deterministic pitfall:

> If you stuff capability files (skills, snippets, configs) into hidden dirs, they simply won't sync to the new machine — the project arrives on the new box as a broken, limb-missing copy.

The pattern that works (validated across real projects):

- **Carry capabilities non-hidden**: Place the project's real dependencies (extraction scripts, CSS snippets, tool scripts) in **non-hidden** form inside the project (e.g. `project-context/tools/` or a non-hidden dir that ships with the capability), so the sync tool picks them up;
- **Regenerate hidden targets locally**: Platform-specific hidden landing spots (e.g. a platform that only recognizes a skill when placed under `.workbuddy/skills/`) must **not** be moved by sync. Instead, ship an in-project activation script (e.g. `deploy.py` or an `ensure_xxx()` function) that **regenerates these hidden targets locally** on first run on the new machine;
- **Source ships with the capability**: The "source" the activation script references (templates, snippet originals) must also travel with the project; the script only does the one job of "copy / generate into the platform-specific location".

#### 12.3 Declarative dependency management / cross-platform deployment checklist

Every BRF project should keep a "cross-platform deployment checklist" in `standards.md` (or a standalone file), splitting into two categories:

| Category | Meaning | Handling |
|----------|---------|----------|
| Carried with project | Tools / scripts / configs already in the project folder, works on copy | New Agent reads directly, no extra action |
| Needs fresh-environment install | Runtime deps (community skills, Python venv, platform plugins, etc.) not in the project | Annotate the **install method** (command / link); new Agent verifies item-by-item and installs on demand |

> This checklist is the new Agent's "health check" on takeover: first confirm the "carried" items are complete, then handle the "needs install" items — avoiding a silent missing dependency that breaks the project.

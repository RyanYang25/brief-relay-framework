# Brief Relay Framework (BRF) v1.7

> **One-line positioning**: Use one continuously-evolving **Project Brief (BRIEF)** as the single entry point, paired with a three-part kit — **contract header + relay baton + recovery protocol** — so that any AI Agent platform can take over at zero cost, migrate cross-platform, and never lose project context.
>
> **Why the name**: "Brief" = the core artifact (BRIEF.md); "Relay" = the cross-Agent / cross-platform / cross-session context-passing logic — who produces, who consumes, who moves next, declared explicitly, like a baton passed hand-to-hand.
>
> **Scope**: Any project requiring multi-file collaboration and state tracking — consulting, writing, R&D, operations, etc. Domain-agnostic; not bound to any IDE or Agent platform.
>
> **Version note**: This is v1.7. v1.0 was the first formally-named public release; v1.1 adds "XII. Tool & Script Dependency Portability (cross-machine)", distilled from real cross-machine deployment pitfalls (NAS / cloud drives exclude hidden folders by default); v1.2 adds "XIII. Project Self-description Boost: Identity Snapshot + Takeover Checklist", absorbing the meta-info self-description and pre-publish testing ideas from standardized skills (taking the spirit, not the wrapper); v1.3 adds "XIV. Traceability & Decision Lifecycle (four-state model)", making the Open/Decisions/Resolved/Archive lifecycle explicit (Open strengthened, Decisions gains owner/impact-scope, Archive makes retire≠delete explicit), and clarifies the division between raw session evidence and structured docs; v1.4 adds "XV. Four-state Patrol Checklist & Active-context Lightening", turning the v1.3 four-state model into an actionable "four-state patrol checklist" that must be run at every iteration close-out; v1.5 adds "XVI. Four-state Readiness Gate & Platform Entry Adapter", pushing v1.4's "manual patrol" one step further — A upgrades the four-state patrol checklist into a **machine-verifiable handoff readiness gate** (a lightweight script that auto-checks four-state closure and emits a report), and B provides a **pure-Markdown BRIEF⇄AGENTS.md/CLAUDE.md bidirectional mapping** (platform entry adapter) so a BRF project keeps its "single-entry authority" while still enjoying the auto-takeover benefit of mainstream coding platforms (Cursor / Claude Code / Roo, etc.). Neither introduces external dependencies nor violates the de-platform-binding red line. v1.6 makes three targeted enhancements to existing chapters (distilled from a real cross-platform takeover test on 2026-08-31 — a brand-new Agent with zero memory taking over a WorkBuddy-hosted project; 9 of 12 claimed capabilities passed, 3 real gaps found): A adds a `last_updated` field to the Ch. II contract header so the recovery protocol's "compare last-updated times" (9.1) becomes machine-verifiable, and the Ch. XVI readiness gate gains a timestamp-consistency check; B adds 12.4 "File Encoding Convention" to Ch. XII — project files uniformly UTF-8 without BOM, native CLIs read with explicit encoding, Chinese-literal scripts need UTF-8 BOM; C adds a minimal reference implementation of the gate (`tools/four-state-gate.ps1`, which the BRF project space itself runs and has verified). v1.7 adds "XVII. Handoff Trustworthiness: Validation Statement, UNKNOWN Ledger, Five Handoff-quality Questions, and Capacity Threshold" (from a 2026-09-01 GitHub benchmark of agent-handoff-skill / waggle / katalint, etc.): where v1.5→v1.6 made handoff *formally* machine-checkable, v1.7 makes the *content* trustworthy — A requires every "done" claim to carry a validation statement (conclusion / how verified / result / not-verified) plus an explicit UNKNOWN ledger that quarantines assumptions; B gives five handoff-quality questions for each hand-off action; C quantifies v1.4's active-context lightening into a capacity threshold (~500 lines / 32 KB advisory line); D adds a "document-health smell" family to the 16.1 gate (dangling links / leftover placeholders / missing required fields / open-UNKNOWN count / oversize, split into ERROR/WARN) and upgrades the minimal reference four-state-gate.ps1 accordingly; 17.6 deliberately excludes token scheduling / MCP / vector memory / orchestration frameworks that violate the pure-Markdown zero-dependency red line.

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
| `last_updated` | Date of this file's most recent update (`YYYY-MM-DD`), enabling the recovery protocol's 9.1 timestamp comparison to be machine-checked (new in v1.6) |

**BRIEF.md (@context-root) example**:

```
<!-- @context-root
role: Single project entry point + evolution engine
produced_by: Update SOP (auto/manual trigger)
consumed_by: All Agents and human successors, must read at the start of every session
update_trigger: After any status/decision/file-structure change
last_updated: YYYY-MM-DD
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
last_updated: YYYY-MM-DD
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
2. Compare the last-updated times of both, confirm status consistency (v1.6: the contract-header `last_updated` field makes this comparison machine-checkable, see the 16.1 gate's timestamp-consistency check);
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

#### 12.4 File Encoding Convention (new in v1.6)

> **Starting point**: BRF's "portability-first constraint" requires documents to be cross-platform readable. Real-world testing found a common blind spot: native Windows PowerShell etc. decode UTF-8-without-BOM Chinese files using the system ANSI/GBK code page by default, producing mojibake — the same file renders fine in VS Code / GitHub / macOS / Linux, yet is "unreadable" in a native CLI. 12.1 rules out "hardcoded usernames in paths"; this section rules out "inconsistent file encoding".

- **Data files uniformly UTF-8 without BOM**: all `.md` and other data files use UTF-8 without BOM (safest cross-platform — GitHub / VS Code / macOS / Linux all recognize it by default).
- **Native CLIs declare encoding explicitly**: when reading UTF-8-without-BOM Chinese files in a native environment that defaults to non-UTF-8 (e.g. Windows PowerShell), always use `Get-Content -Encoding UTF8` explicitly, or the Chinese text turns to mojibake.
- **Scripts with Chinese literals need UTF-8 BOM**: Windows PowerShell 5.1 parses `.ps1` files using the ANSI code page when the file has no BOM, garbling Chinese literals — script files must be UTF-8 with BOM, or use English-only output internally.
- **Checklist addition**: the 12.3 deployment checklist can add a line "Carried with project: encoding convention (UTF-8 no BOM / explicit encoding in native CLI)" for the new Agent to verify on takeover.

### XIII. Project Self-description Boost: Identity Snapshot + Takeover Checklist (new in v1.2)

> Source: a generic method abstracted from two practices of standardized skills — "meta-info self-description" (frontmatter: name / description / use-when) and "pre-publish testing". BRF itself does not adopt the skill wrapper (see project-space standards.md Ch. VII for positioning), but absorbs the standardization thinking — so that any project reads as "instantly understandable, takeover with a procedure" to a new Agent.

#### 13.1 Project Identity Snapshot (30-second takeover card)

At the top of every BRF project's `BRIEF.md`, place a pure-Markdown "project identity snapshot" (no YAML / hidden dirs; readable on any platform, visible at repo root) that answers at least four questions:

| Field | Question to answer | Purpose |
|-------|-------------------|---------|
| What it is | What is the essence of this project / this context | New Agent builds a mental model in 30s, avoiding mis-positioning |
| When to read deep | Under what circumstances should project-space details be read | Separates "apply the framework" from "maintain the framework" takers, skipping irrelevant reading |
| Minimum takeover action | The minimum must-read + must-do before touching anything | Prevents context drift from "editing before reading" |
| Prerequisites | Preconditions for running / publishing | Taker confirms the environment upfront, avoiding stalls on auth / toolchain |

> Key point: the snapshot parallels a skill's frontmatter, but **deliberately avoids YAML** — portability is BRF's first constraint, and YAML + platform-hidden dirs would break "pure Markdown + root-visible". Machine-readable metadata goes to the HTML-comment `@context-root` block; human / Agent-readable summary goes to the pure-Markdown snapshot; the two are complementary.

#### 13.2 Takeover Checklist (parallels skill pre-publish testing)

At the "Session Recovery Protocol" (Ch. IX), attach a "Takeover Checklist" that a new Agent **ticks item-by-item before acting**, paralleling the skill discipline of "tested before publish":

- [ ] Read `BRIEF.md` (incl. @context-root block) + current-status file (incl. @context block); their last-updated timestamps match
- [ ] Determined which track this instruction belongs to: project-space maintenance, or public-spec publishing (see dual-track declaration)
- [ ] Reconciled current status and in-progress tasks; no unclosed in-flight task (or already reconciled)
- [ ] If publishing: confirmed the three red lines — auth / scope / authorship
- [ ] Clarified the downstream relay order of this change (which file moves next)

> This checklist makes the "Session Recovery Protocol" executable: turning "what to do on recovery" from a principle into a tick-action, lowering the chance a new Agent skips a step. It is also the acceptance criterion for new-Agent-takeover — if every item can be ticked, the project's self-description is proven adequate.

---

### XIV. Traceability & Decision Lifecycle (four-state model)

> **Starting point**: The real danger for a project is not that documents sit unused for a while — it is discovering, when you need to look back, that there is no evidence: you cannot explain why something was designed this way, nor tell whether an old problem has resurfaced. Trace first, then analyze. Requirement discussions, trade-offs, errors, fixes, and evaluation results from development should all be recorded structurally, not left only in conversation.

#### 14.1 The four-state lifecycle (each item belongs to exactly one state)

Every decision / issue / proposal / evaluation entry should explicitly belong to one of the four states below, and migrate explicitly when its status changes:

| State | Meaning | Entry condition | Typical landing file |
|-------|---------|-----------------|----------------------|
| **Open** | Issues still under discussion, assumptions, risks, to-verify items | Raised but not yet decided or verified | `current.md` "Open issues / assumptions / risks" section |
| **Decisions** | Decisions made, with rationale, owner, impact scope | Decided (with rationale) | `current.md` "Recent decisions" + `PROJECT_GUIDANCE.md` |
| **Resolved** | Issues solved, with fix method and corresponding regression case | Closed and verified | `current.md` issue clearance + `standards.md` "Pitfall records" |
| **Archive** | Superseded proposals / evaluations still needing traceability (retire ≠ delete) | Proposal replaced / feature deprecated / evaluation rejected | git tag / `handoffs/` / iteration "excluded items" |

> Key discipline: an entry belongs to exactly one state at a time. Open → Decisions → Resolved/Archive is the normal flow; the migration itself must also be persisted — no "ghost entries".

#### 14.2 Open state: make "not-yet-clarified / not-yet-verified" explicit

Open is the weakest link in most projects. It is deliberately decoupled from "in-progress tasks":

- The **task table** manages "what to do" (work clearly mandated to execute);
- The **open-issues table** manages "what is not yet clear / not yet verified" (assumptions, risks, open questions).

`current.md` should hold a dedicated section for: Assumption, Risk, To-verify. These items are not rushed to "just decide", but they must be visible — at retrospective they are the key evidence explaining "why we later changed direction".

#### 14.3 Decisions state: decision + rationale + owner + impact scope

BRF already mandates "decisions must record rationale" (see Ch. III `current.md` recent decisions). v1.3 further requires each decision to carry at least four fields: **content + rationale + owner + impact scope**.

- Owner: who made the call / who is responsible for execution;
- Impact scope: which files, modules, collaborators, or downstream projects are affected.

With all four present, a successor sees not just the conclusion but whether "this decision still holds" and "who is affected if I change it".

#### 14.4 Archive state: retire ≠ delete

Superseded proposals, deprecated features, and rejected evaluations move to Archive with "why retired" preserved — this is the baseline of traceability.

- Existing infrastructure already naturally serves Archive: `git tag` anchors historical versions, `handoffs/` stores historical deliverables, and the public-spec iteration's "excluded items" records "why something did/didn't enter the spec";
- Archived entries are **not deleted**; only the retirement reason and effective time are annotated, so that later "should the old proposal be revived" or "has the old problem resurfaced" can be checked against evidence.

#### 14.5 Raw evidence vs structured docs: division, not substitution

Platforms (Codex / Claude Code / local WorkBuddy, etc.) typically keep session logs locally. They can serve as **raw evidence** — letting another Agent analyze only user inputs, decision shifts, or failure paths. But chat logs **cannot substitute** for structured docs: docs read faster, have clearer boundaries, and suit team collaboration and version control better (consistent with Ch. IX "transcript has no authority").

> Practical point: raw session logs are the "reference raw draft"; the four-state docs are the "authoritative truth for collaboration". Run both in parallel — keep raw evidence, but don't be held hostage by it.

---

### XV. Four-state Patrol Checklist & Active-context Lightening (new in v1.4)

> **Source**: After v1.3 Ch. XIV made the Open/Decisions/Resolved/Archive lifecycle explicit, the BRF project space refactored its own `current.md` along the four-state model on 2026-08-14 (four-state view + archival of historical records). This "practice the advocated traceability first, then distill it into a generic method" exercise yielded two portable close-out disciplines — they pass the dual-track filter (domain-agnostic / no local-ops details / reusable by others) and enter this spec.

#### 15.1 Four-state Patrol Checklist (mandatory at iteration close-out)

At every iteration / project-phase close-out, self-check along Open→Decisions→Resolved→Archive. **All four states closed = the iteration is done**; any unclosed item must either be resolved this round or explicitly carried to the next (kept Open or annotated with Archive reason) — never "silently swallowed".

| State | Self-check question | Close criterion |
|-------|---------------------|-----------------|
| **Open** | Are there still unclosed assumptions / risks / to-verify items? | All handled: moved to Decisions (decided), Resolved (verified), or explicitly carried to next round with a trace |
| **Decisions** | Do all important decisions this round carry the four fields? | content + rationale + owner + impact scope complete |
| **Resolved** | Are resolved issues recorded and regression-verifiable? | fix method + regression case / verifiable evidence persisted |
| **Archive** | Do retired / superseded proposals keep "why retired"? | retirement reason + effective time annotated, not deleted |

> Key point: the checklist is the "operationalized close-out" of the v1.3 four-state model. It turns "trace first, then analyze" from a principle into a mandatory tick-action at iteration end, preventing half-done close-outs where "the version shipped but Open still hangs a pile of unclear things".

#### 15.2 Active-context Lightening (progressive-disclosure in practice)

The four-state archival exercise yields a second discipline: active files keep only "current state + pointer"; historical records are archived with originals intact.

- **Active files (e.g. `current.md`)**: keep only the current Open/Decisions/Resolved/Archive + pointers ("details in `PROJECT_GUIDANCE.md` Ch. X"), no piling of historical full text;
- **Historical archival**: build timelines, iteration-candidate full text, historical to-dos move into `PROJECT_GUIDANCE.md` (or `handoffs/`), originals kept intact, only removed from the active view;
- **Effect**: active files stay lean (new Agent focuses on the present), history stays traceable (consistent with v1.3 Archive "retire ≠ delete").

> Link to v1.3: lightening is not "discarding history" but moving it from the "active sightline" to the "archive layer" — satisfying progressive disclosure (Layer 4 on demand) while holding the four-state traceability baseline.

#### 15.3 Link to the publish flow (release gate)

The four-state patrol checklist is the "release gate" of iteration close-out: before writing the public-spec text, run the 15.1 check once — Open has no leftovers, Decisions four fields complete, Resolved/Archive traceable — then proceed to dual-track Step 4 (write `handoffs/` text → bump README → push to GitHub with tag). This closes the loop between v1.3 "traceability" and v1.4 "close-out self-check", and is exactly how this spec's own v1.4 release was practiced.

---

### XVI. Four-state Readiness Gate & Platform Entry Adapter (new in v1.5)

> **Source**: 2026-08-17 GitHub competitive research (cursor-memory-bank 3,055★ / my-claude-code-setup 2,585★ / roo-code-memory-bank 1,677★ / RooFlow 1,232★ / memory-bank-mcp 916★ / BMAD-METHOD 51,966★ / OpenSpec 65,078★). After the BRF project space shipped the "Four-state Patrol Checklist" in v1.4, it studied the "IDE event auto-update + UMB manual fallback" of the Memory Bank family and the AGENTS.md/CLAUDE.md platform-entry conventions, and distilled two portable enhancements — both pass the dual-track filter (domain-agnostic / no local-ops details / reusable by others) and enter this spec.

#### 16.1 Four-state Readiness Gate (machine-verifiable handoff)

v1.4's "Four-state Patrol Checklist" is a manual tick-action. v1.5 upgrades it into a **machine-verifiable handoff readiness gate**: a lightweight validation script that, on save / close-out, automatically checks Open/Decisions/Resolved/Archive closure and emits a readable "readiness report" — turning "a human remembers to tick" into "a machine checks for you".

- **Form**: a plain-file script in `project-context/tools/` (Shell / Python / PowerShell all fine), cross-platform readable and carried with the project (see v1.1 Ch. XII); bound to no IDE and not requiring a runtime — run it manually if you have none, run it one-click if you do.
- **Gate logic** (BRF specifies *what to check* and *what to output*, not the language): parse the four-state markers in `current.md` and judge item-by-item:

  | State | Gate check | Close criterion |
  |-------|-----------|-----------------|
  | **Open** | Are there still unclosed assumptions / risks / to-verify items? | All handled (moved to Decisions / Resolved, or explicitly carried to next round with a trace) |
  | **Decisions** | Do this round's decisions carry the four fields? | content + rationale + owner + impact scope complete |
  | **Resolved** | Do resolved items have a fix method + verifiable evidence? | fix method + regression case / evidence persisted |
  | **Archive** | Do retired items keep "why retired"? | retirement reason + effective time annotated, not deleted |
  | **Timestamp (v1.6)** | Do BRIEF and current's contract-header `last_updated` match? | both carry explicit `last_updated` and the values match (step 1 of the 9.1 recovery protocol becomes machine-checkable) |
  | **Document-health smells (v1.7)** | Any dangling links / leftover placeholders / missing required fields / open UNKNOWNs / oversized active file? (see 17.5) | ERROR-class cleared, WARN-class explicitly addressed (ERROR blocks; WARN only advises) |

- **Output**: a "readiness report" — which states are closed, which have gaps, gaps located to the line — for use before publishing (echoing the 15.3 release gate) or at any checkpoint.
- **Link to publish flow**: the 15.3 release gate upgrades from "manual patrol" to "manual or scripted gate" — the gate's "four states closed" output is one of the publish preconditions.

> Key point: the gate is the "operational re-upgrade" of the v1.4 checklist, not a new dependency. BRF dictates no script language, only "what the gate checks + what it outputs"; the implementation is project's choice, following the plain-file + cross-platform-readable pattern of `tools/publish.ps1`.

- **Minimal reference implementation (v1.6)**: the BRF project space itself ships `tools/four-state-gate.ps1` (PowerShell, plain file + cross-platform readable) — it parses `current.md`'s four-state markers + compares contract-header `last_updated` + checks file-encoding health, and emits a "readiness report"; verified on 2026-08-31; **v1.7 adds the document-health smell family (see 17.5: dangling links / leftover placeholders / missing required fields / open-UNKNOWN count / active-file oversize, split ERROR/WARN), verified on 2026-09-01**. New projects may follow this implementation or choose their own language (Shell / Python / PowerShell all fine); BRF only dictates "what to check + what to output", not the language.

#### 16.2 Platform Entry Adapter (BRIEF⇄AGENTS.md/CLAUDE.md bidirectional mapping)

BRF's single entry is BRIEF (plain Markdown, platform-agnostic). But mainstream coding-Agent platforms (Cursor / Claude Code / Roo Code, etc.) auto-read `AGENTS.md` / `CLAUDE.md` as the project entry at session start. A BRF project that maintains only BRIEF misses these platforms' "auto-takeover" channel. The platform entry adapter fixes this — **without replacing BRIEF's authority, but adapting BRIEF to each platform's entry**.

- **Single source of truth unchanged**: BRIEF remains the sole authoritative entry; `AGENTS.md` / `CLAUDE.md` are **derived views** (generated from BRIEF, not a second system).
- **Bidirectional mapping spec** (pure Markdown, usable independently of BRF's internal context):

  | BRIEF section | Maps to platform entry | Note |
  |---------------|------------------------|------|
  | Identity Snapshot (v1.2 13.1) | Platform entry top "Project Overview" | 30-second takeover card sinks as-is |
  | File Organization + File Index (BRIEF II/IV) | "Project Structure / Key Files" section | Tells the platform Agent which files to read first |
  | Session Recovery Protocol + Takeover Checklist (Ch. IX/XIII) | "Work Rules / Takeover Steps" section | Makes the platform Agent also follow the recovery protocol, not assume auto-sync |
  | Current Status + Recent Decisions | "In-progress / Recent decisions" section (optional, refresh periodically) | Platform-side Agent also sees current progress |

- **One-click produce / recover**: ship a script or documented steps in the project to generate `AGENTS.md` / `CLAUDE.md` from BRIEF (forward), and merge platform-side manual edits back into BRIEF (reverse, optional) — on recovery, BRIEF wins, avoiding dual-source drift.
- **Does not violate the de-platform-binding red line**: the adapter is a "mapping layer", not a "replacement layer" — the core is still plain-Markdown BRIEF; the mapping rules themselves are platform-agnostic plain text that merely describe "how to dock with a platform's entry". It solves the pain of "BRF projects can also enjoy platform auto-takeover" rather than binding BRF to any platform.

> Key point: `AGENTS.md` / `CLAUDE.md` are "platform-side BRIEF mirrors", not a second source of truth. BRIEF always wins; the adapter only translates and syncs.

#### 16.3 Deliberately excluded: full IDE auto-sync hooks

Competitors (the Memory Bank family) widely use "IDE event auto-update + UMB manual fallback" for context relay. BRF **does not bring "real-time listening to session events to auto-rewrite BRIEF" into the public spec** — reasons: ① listening to platform session events needs platform-specific hooks, violating BRF's "de-platform-binding" first constraint; ② single-platform hooks would work but bind BRF to that platform, contradicting the meta-framework positioning. BRF already has the "session recovery protocol + incremental persistence + takeover checklist" trio against interrupt / loss; full auto-sync is a nice-to-have, not a must. This direction stays as a **project-space experiment**, not in the public spec; if pursued later, prefer a "platform-agnostic relay protocol" over a "platform-specific hook".

---

### XVII. Handoff Trustworthiness: Validation Statement, UNKNOWN Ledger, Five Handoff-quality Questions & Capacity Threshold (new in v1.7)

> **Source**: a 2026-09-01 GitHub external benchmark (agent-handoff-skill's validation/risks, waggle's defect taxonomy for "bare-path handoff", katalint's handoff-doc linter, etc.; the BRF project space keeps the full research memo). The v1.5 gate and v1.6 timestamp/encoding made handoff ***formally* machine-checkable**, but the benchmark uncovered a layer not yet covered: a filled four-state table and matching timestamps only prove "document form is complete" — they do not prove "the content deserves the next Agent's trust". A conclusion may be a mere unverified claim, an assumption may travel downstream as if confirmed, and the handoff may carry dead links and placeholders. This chapter adds the layer of "**trustworthy content**": four sections plus a gate hook, all passing the dual-track filter (domain-agnostic / no local-ops details / reusable by others), all delivered in pure Markdown and generic checks, with no runtime or platform dependency.

#### 17.1 Validation Statement (a "done" must carry "how verified / what is not")

Problem: a handoff says "done / fixed / shipped", yet the next Agent cannot tell whether it was truly verified or merely claimed. Rule: **every completion-class conclusion carries a lightweight validation statement** with four fixed parts; a missing part counts as "verification incomplete":

| Part | What to write |
|------|---------------|
| **Conclusion** | What was done and the result (one falsifiable sentence) |
| **How verified** | How it was verified — command / test / re-read / manual check; must be reproducible |
| **Result** | What was actually observed — pass / fail / key numbers / evidence pointer |
| **Not verified** | What was not verified, why, risk and suggestion (explicitly leave blank; write "none" if so) |

Template (pure Markdown, paste directly into current / the relay baton):

> Validation statement
> · Conclusion: (what was done, result)
> · How verified: (command / test / re-read path; reproducible)
> · Result: (actual observation: pass / fail / numbers / evidence pointer)
> · Not verified: (what wasn't, risk, suggestion; "none" if nothing)

> Key point: the validation statement separates "claim" from "evidence". **A not-verified item is not a deduction — hiding it is.** Writing "what wasn't verified" explicitly is what stops the next Agent from treating the unverified as established.

#### 17.2 UNKNOWN / To-confirm Ledger (assumptions must not travel as facts)

The most dangerous thing in a relay chain is not "not knowing" but passing the previous baton's **assumption** downstream as a **confirmed fact**. Rule: keep an explicit UNKNOWN ledger in the active context (`current.md` / relay baton), registering item-by-item every assumption, unknown, decision-to-make, and pending external acknowledgement:

| Item | Type | Status | Basis / next step | Owner |
|------|------|--------|-------------------|-------|
| (what to confirm) | assumption / unknown / to-decide / pending-reply | to-confirm / confirmed / ruled-out | basis or confirming action | who |

Three hard rules: ① a key fact written into the body whose source is **inference rather than confirmation** must also enter the UNKNOWN ledger; ② before closure, body conclusions use "tentative / to-confirm" wording, never stated as settled; ③ at iteration close-out handle each row — flip to "confirmed" (add basis) or "ruled-out" (add reason) or explicitly carry it to the next round by name — **no "to-confirm" row may silently roll into the next baton**.

> Relation to the four states: UNKNOWN is orthogonal to Open — Open tracks "is the item closed", UNKNOWN tracks "is the fact confirmed". An item can be Open-closed while one of its data points remains an unconfirmed assumption; both must be explicit.

#### 17.3 Five Handoff-quality Questions (the quality gate of one hand-off action)

v1.4 §15.1 is a four-state patrol at "iteration close-out"; this section is five questions run **before each time a handoff artifact is passed to the next baton (human or Agent)**, targeting "can this be trusted and understood":

| # | Ask yourself | Typical failure |
|---|--------------|-----------------|
| 1 **Attribution** | Can every key conclusion be traced to "who produced it, on what basis"? | sourceless assertions, "reportedly / probably" |
| 2 **Fit & density** | Does the level of detail match the takeover scenario? | dumping the whole text to a local-only taker; or one sentence where detail is due |
| 3 **Freshness** | Any stale content? Are timestamps / pointers still valid? | links to moved/deleted files, outdated state |
| 4 **Next step** | Are the next action and owner clear? | only "keep going", without saying what or who |
| 5 **Reachability** | Is it readable off this machine? | private absolute paths, private deps, links openable only on this machine |

> Division of labor with 15.1: the four-state patrol tracks "is the project state closed"; the five questions track "is this handoff trustworthy and easy to take". The former runs per project phase, the latter per hand-off action; they stack.

#### 17.4 Active-context Capacity Threshold (quantifying 15.2 lightening)

v1.4 §15.2 asks that "active files keep only current state + pointer", but relies on self-discipline. This section gives an actionable **advisory line** (suggested default; a project may customize it in `standards.md` or the contract header):

- **Threshold**: a single active-context file (`current.md` / relay baton) beyond **~500 lines or 32 KB** triggers an "archive & split" signal (WARN, not a hard error).
- **Action when triggered**: historical full text (full build timelines, closed iteration candidates, stale to-dos) moves to the archive layer (`PROJECT_GUIDANCE.md` / Archive); the active file keeps only current state + pointers — turning 15.2 from discipline into "warn on overflow".
- **Why only warn, not auto-split**: auto-chunking / index databases create cross-file stitching burden and runtime dependencies; BRF only reminds a human to archive on overflow, holding the pure-Markdown, zero-dependency red line. The numbers are a starting point to calibrate per project (the calibration itself goes into the UNKNOWN ledger, see 17.2).

#### 17.5 Gate hook: the document-health smell family (candidate D)

Making 17.1–17.4 machine-checkable: beyond four-state / timestamp / encoding, the 16.1 gate gains a "handoff document-health smell" family that scans active-context files:

| smell | What it checks | Level | Maps to |
|-------|----------------|-------|---------|
| **Dangling link** | Does a Markdown relative link / relative path point to a file that actually exists (excluding http(s), #anchors, mailto)? | ERROR | 17.3-3 Freshness |
| **Leftover placeholder** | Unclean placeholders: TODO / TBD / FIXME / XXX / 待补充 / 待定 / 待填写 | WARN | 17.1 (state unverified explicitly, don't leave an empty placeholder) |
| **Missing required field** | Are contract-header required fields (role / last_updated, etc.) and decision fields empty? | ERROR | 16.1 / 17.1 |
| **Open UNKNOWN** | Count of "to-confirm" rows in the UNKNOWN ledger (must be addressed by name at close-out) | WARN | 17.2 |
| **Active file oversize** | Does the active file's lines / KB exceed the 17.4 threshold? | WARN | 17.4 |

Two levels: **ERROR must be cleared to pass the release gate** (dangling links / missing required fields directly mislead the next Agent); **WARN only advises, never blocks** (placeholders / UNKNOWN count / oversize are left to human judgment on whether to handle this round), so noise does not drown the signal. For the minimal implementation see 16.1's `four-state-gate.ps1` (v1.7 includes this family; plain file, zero dependency).

#### 17.6 Deliberately excluded: take the ideas, not the heavy implementations

Present in the external benchmark but **not** brought into the public spec: token-budget scheduling and MCP read-telemetry, vector memory stores (embedding / retrieval), multi-Agent orchestration code frameworks, JSON manifests and file fingerprints, auto-chunking and all installers / scaffolds — they either bind to a specific runtime/platform or turn a pure-Markdown method into a code system that must be deployed, violating the first constraint of "pure Markdown, zero external dependency, de-platform-binding". BRF takes only the underlying ideas (freshness, attribution, capacity, explicit unknowns) and lands them with plain-text structures and one lightweight check.

> Chapter point: v1.6 made handoff "***formally* verifiable**", v1.7 makes it "**content-trustworthy**" — the validation statement supplies evidence, the UNKNOWN ledger quarantines assumptions, the five questions govern each hand-off's quality, the capacity threshold protects takeover density, and document smells make all of the above machine-checkable; no new platform or runtime dependency is added.

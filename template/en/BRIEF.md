<!-- @context-root
role: Project single entry point + evolution engine
produced_by: Update SOP (auto/manual trigger)
consumed_by: All agents and human successors; read at the start of every session
update_trigger: After any status/decision/file-structure change
upstream: context/current.md (active state), context/standards.md (constraints)
downstream: context/current.md → context/standards.md → {meeting-notes-dir}/
-->

# BRIEF.md - Project Brief

> **📌 If you receive an instruction like "update brief / update project status / sync progress":**
> Don't edit this file alone! First read the "Update SOP" section of this file, find the matching scenario, and update current.md / standards.md etc. in the order the SOP specifies.

<!-- Meta: last updated | by | reason -->
> Last updated: YYYY-MM-DD | by: | reason: init

## 1. Project Profile (stable info, rarely changes)
- **Project name**: {project name}
- **Goal**: {one sentence on what to achieve}
- **Deliverables**: {core output}
- **Key constraints**: {time / resource / compliance limits}
- **Milestones**: {key dates}

| Role | Name | Decision power |
|------|------|----------------|
| Project lead | {name} | ⭐⭐⭐ highest |
| Contact | {name} | ⭐⭐ high |
| Executor/Advisor | {name} | ⭐ medium |

## 2. File Organization (actual tree)
<!-- List your real structure; if you already use prefixes like A01/B02/C01, keep them, don't invent new ones -->
```
{project root}/
├── BRIEF.md
├── {meeting-notes-dir}/
└── project-context/
    ├── context/
    │   ├── current.md
    │   └── standards.md
    ├── references/
    ├── handoffs/
    ├── .working/
    ├── changelog.md
    └── PROJECT_GUIDANCE.md (optional)
```

## 3. Current Status (one line + pointer)
{one line, e.g. "v1 draft done, awaiting feedback"} → see `context/current.md`

## 4. File Index (router)
- `context/current.md`: current state, tasks, recent decisions
- `context/standards.md`: output specs, workflow, past pitfalls
- `references/`: raw materials
- `handoffs/`: finished deliverables
- `{meeting-notes-dir}/`: meeting / comm notes
- `PROJECT_GUIDANCE.md` (optional): full decision history

## 5. Reading Rules (progressive disclosure)
1. Read this file first (incl. @context-root block) for global context
2. Then `context/current.md` (incl. @context block) for current state
3. Read `context/standards.md` or files under `references/` as needed
4. For history, read `PROJECT_GUIDANCE.md` as needed
5. Don't read everything at once

## 6. Session Resume Protocol (cross-platform safety)
Whenever an agent re-takes this project (new session / platform switch / after interruption), step 1 must be:
read BRIEF.md + context/current.md → compare update times → reconcile if inconsistent → then work.
This framework does not rely on "auto-sync on interruption"; it relies on the resume protocol + incremental checkpointing to avoid losing context.

## 7. Update SOP (mandatory)
<!-- On "update brief/status", find the matching scenario first, then update linked files together—don't edit this file alone.
     Full SOP (7 scenarios) is in the spec 简报接力-v1.0.md chapter 8. -->
- New meeting note → store in {meeting-notes-dir}/ → update current.md → sync this file's "current status"
- Task status change → update current.md → sync this file if it affects the whole project
- Important decision → update current.md (with rationale) → sync this file → record in PROJECT_GUIDANCE.md
- New deliverable → store in handoffs/ → update current.md → this file's index
- Pitfall/spec → append-only to standards.md

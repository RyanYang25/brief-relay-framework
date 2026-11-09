<!--
Nature: sensitive file (Chapter XXIV, 24.2 / 24.6)
Records the true values of this project's out-of-workspace deployment artifacts
(live path / purpose / workspace copy / restore method / contains-credentials).
Discipline: (1) never enters a public release package; (2) never mirrored wholesale into a public repo or network share; (3) credential-bearing artifacts must not be copied (24.5).
When referring to this file in public, use placeholders only ({DEPLOY_DIR} / {HOST_IP} / {HOST_NAME}).
-->

# Deployment Manifest - {Project Name}

> Basis: Brief Relay Framework (BRF) Chapter XXIV "Deployment Artifact Traceability: Takeover-readiness of the Out-of-workspace Runtime Side".
> Location: single-line projects keep it at the root `deploy-manifest.md`; multi-line projects keep one per line under `<line>/deploy-manifest.md`.
> Trigger: adding / removing / moving an artifact, changing the interpreter, or changing the startup method must update this file within the same change.

## 1. Determination

<!-- Mandatory for all projects: even with no artifacts, keep this section and state "none" explicitly with a determination date — never silent omission -->

This project currently has **no** out-of-workspace deployment artifacts (determination date: {YYYY-MM-DD}).

<!-- If artifacts do exist, delete the line above and register them one by one in the table below -->

## 2. Artifact Register

| Live path | Purpose | Workspace copy | Restore method | Contains credentials |
|---|---|---|---|---|
| {DEPLOY_DIR}/service.py | Long-running HTTP service; devices pull images on a schedule | `handoffs/{line}/deploy/service.py` (same version, verified {YYYY-MM-DD}) | Copy the copy back to the live directory; needs a 3.10 interpreter + Pillow/flask; registered autostart `start.vbs` | No |
| {DEPLOY_DIR}/creds.env | Service access token | No copy (credential-bearing, mirroring forbidden per 24.5) | Regenerate inside a session via the refresh script; never written to disk | Yes |
| {DEPLOY_DIR}/runtime/python310 | 48 MB standalone interpreter, not synced with the project | No copy (binary, large / uncopyable) | Install 3.10 from {source} to the same path, then install Pillow / flask {version} | No |

> Column rules:
> - **Live path**: falls under the same exemption as the 23.2 environment topology table (per 24.6); precise to file or directory, list multiple instances one by one.
> - **Purpose**: write *why it must live outside the workspace* (resident / OS-level registration / pulled by a device), not empty phrases like "helper script".
> - **Workspace copy**: file copies are authoritative; a copyable text artifact must have a copy and the same version as live (hash or content diff + verification date); uncopyable binaries / large runtimes write "no copy + reason".
> - **Restore method**: with a copy = where to copy back; without = rebuild steps (source, dependency packages, registration content, allowance objects).
> - **Contains credentials**: entries marked "Yes" must not be mirrored into any sync directory (cloud drive / shared drive / private repo) per 24.5; record the generation method only.

## 3. Not Covered (honest registration, not a deduction)

<!-- Anything not yet retrieved, not yet registered, or existing only on the device side goes here, together with how to fill it -->

| Not covered | Reason | How to fill |
|---|---|---|
| (example) device-side scheduled task config | Lives only on the e-ink device, cannot be exported | Next time on site: screenshot + copy the cron expression into the register |

## 4. Temporary-artifact exit check (24.4)

- The live directory must not retain long-term `_test_*` / `mock_*` / `test.png` / `tmp_*` names; temporary artifacts take one of three exits: delete on completion / promote-and-rename into the workspace / register as reconstructable.
- The third state is forbidden: on site, not in the manifest, nobody knows whether it can be deleted.
- Last check date: {YYYY-MM-DD}

## 5. Change Log

| Date | Change | Maintainer |
|---|---|---|
| {YYYY-MM-DD} | Created | {maintainer} |

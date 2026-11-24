# UI Operational Playbook Template (`SKILL.md`)

Use this structure when authoring a `SKILL.md` for operating a live web application, SaaS dashboard, or consumer web UI. Keep the final `SKILL.md` under **250 lines** and delete any placeholder sections that do not apply.

````markdown
---
name: <app-workflow-slug>
description: >-
  Automates <specific repeatable workflow> in <App Name> (<target domain>).
  Use when the user asks to <exact trigger verbs and outcomes>.
  Don't use for <unrelated tasks, raw API coding, or one-off questions>.
---

# <App Name> — <Workflow Title>

> Verified On: <YYYY-MM-DD> against live <App Name> (<plan/tier>).

## Entry Routes & Parameter Locks

- **Direct Entry URL:** `https://<app-domain>/<direct-workspace-path>` (skip marketing root)
- **Locked Defaults:**
  - `<Setting / Model / Format 1>`: `<exact value>`
  - `<Setting / Toggle 2>`: `<ON / OFF>` (verify before every run; resets on reload)
- **Required Inputs:** Ask the user upfront if `<input A>` or `<input B>` is missing.

## Human Handoff & Safety Gates

- **Auth / 2FA / CAPTCHA:** If redirected to sign-in or presented with any CAPTCHA / human-verification challenge, **stop immediately** and ask the user to complete it in the browser pane before resuming. Never attempt to bypass challenges.
- **Confirmation Boundary:** **PAUSE FOR USER** before clicking `<Place Order / Publish / Delete / Charge Card>`. Summarize `<items, destination, total>` and wait for explicit approval.

## Execution Sequence

1. **Open & Verify Session:**
   - Navigate to `<Direct Entry URL>` using the active session browser tool (never headless curl/Playwright).
   - Confirm authenticated state by checking for `<semantic landmark: aria-label / heading / button text>`.
2. **Configure & Input:**
   - Locate `<control label or aria-label>` and set locked parameters above.
   - Populate `<input field label>` with `<validated input>`.
   - If staging local files for upload, follow your harness's upload staging path.
3. **Execute & Collect Output:**
   - Trigger `<exact visible button text or aria-label>`.
   - Wait for `<specific completion state, URL change, or artifact download>` (timeout: `<N>s`).

## Failure Scars & Guardrails

- **<Scar 1 — e.g., Modal / Banner Trap>:** If `<announcement dialog>` appears on load, dismiss via `aria-label="Close"` before interacting with the form.
- **<Scar 2 — e.g., Silent Default Reset>:** Switching `<Tab X>` resets `<Toggle Y>` back to default; always set `<Toggle Y>` *last* right before submission.
- **<Scar 3 — e.g., Download / Export Quirk>:** Wait until `<progress badge>` disappears before clicking Download, or the app exports a preview stub instead of the final file.

## Verification (Mandatory Before Claiming Done)

Never trust a button click alone. Confirm all post-conditions:
- [ ] **UI / Host Receipt:** Visible `<confirmation ID / published badge / URL slug>` confirmed in live browser state.
- [ ] **Artifact Integrity (if files downloaded):** Verify file exists on disk, size `> 0` bytes, and `<ffprobe / JSON parse / row count check>` matches expected spec.

## Progressive References

Move volatile model/credit tables, filter lists, and full menus out of `SKILL.md`:
- [ui-controls-and-credits.md](./references/ui-controls-and-credits.md) — Verified model options, credit costs, and detailed UI menus.
````

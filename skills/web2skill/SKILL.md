---
name: web2skill
description: >-
  Conversational meta-skill that converts any website URL, live web application,
  product UI, documentation site, or how-to guide into a portable, scar-tested
  SKILL.md package. Use ONLY when the user explicitly asks to create, generate,
  convert, or harden a reusable agent skill from a URL or web app. Don't use for
  one-off browsing, summarizing a webpage, or executing an existing workflow.
---

# web2skill — Live Web-App & URL to `SKILL.md` Compiler

Turn unfamiliar web applications, SaaS dashboards, or documentation URLs into lean, deterministic `SKILL.md` packages using the tools your agent harness already has—at zero external API cost.

---

## Stage 0: Pre-Flight & Wheel Check

Before scraping pages or opening a browser, run three fast checks:

1. **One-Off vs. Repeatable Check:** If the user only wants a single task performed once, tell them in one sentence that zero-shot execution is faster than maintaining a skill, and confirm they actually want a saved `SKILL.md`.
2. **Local Inventory Check:** Check already-installed workspace/global skills and MCP servers so you never duplicate an existing integration or CLI.
3. **Origin Discovery Check:** Given target URL `https://<host>/...`, probe in order:
   - `https://<host>/.well-known/skills/` and `https://<host>/skill.md`
   - `https://<host>/llms.txt` and `https://<host>/llms-full.txt`
   If an official `skill.md` already exists, show the user what it covers (or offer `npx skills add https://<host>`) and only author a custom skill if they need custom workflows, UI automation, or local guardrails on top of it.

---

## Stage 1: Minimal Scope Lock (Max 2 Questions)

Do not interrogate the user with a long questionnaire. Infer everything possible from the URL and ask **at most 2 short questions** if unspecified:

1. **Target Workflow & Scope:** Which recurring task(s) and output artifacts must this skill handle, and should it be saved to the **workspace** (`.agents/skills/<slug>/` or `.claude/skills/<slug>/`) or **globally**?
2. **Locks & Human Pause Gates:** Are there required parameter locks (model version, resolution, naming format) or irreversible actions where the agent must **always pause for human approval** (checkout/payment, publishing, deleting data)?

---

## Stage 2: Adaptive Recon (Pick Path A or Path B)

### Path A — Documentation, SDK, CLI, or Written Guide URL
Use when the target is an API, library, CLI, or static technical guide:
1. Fetch clean Markdown (`llms.txt`, raw `.md` docs, or reader-cleaned pages) rather than raw HTML dumps.
2. Extract **only** high-leverage operational rules the base model gets wrong zero-shot:
   - Auth/env setup, exact CLI/SDK syntax, version-specific breaking changes, rate limits, and idempotency rules.
3. Move exhaustive endpoint tables, JSON schemas, or troubleshooting matrices into `references/*.md` and link them progressively from `SKILL.md`.
4. Structure the output with [docs-playbook-template.md](./references/docs-playbook-template.md).

### Path B — Live Web Application or SaaS UI
Use when an agent must operate a visual web product (e.g., Google Flow, YouTube Studio, Shopify Admin, Uber Eats, Instagram Creator tools, or internal dashboards):
1. **Never rely on public Help Center text alone.** Help articles say *"click the + icon"* and omit real URLs, dynamic SPA states, and bot defenses.
2. **Use the harness's session-attached browser tool**—never blind headless Playwright/Puppeteer or `curl` against login-walled or bot-protected apps. Read [harness-adapters.md](./references/harness-adapters.md) to match the active harness.
3. Inspect the live UI (pausing cleanly if the user needs to sign in first) and map:
   - **Direct Entry URLs:** Exact workspace/dashboard routes that skip marketing pages.
   - **Semantic Landmarks:** `aria-label`, ARIA `role`, visible button/input labels, or `data-testid`—**never** fragile positional XPaths (`/html/body/div[4]/button[2]`).
   - **Sticky vs. Resetting Controls:** Dropdowns, toggles, or mode switches that silently reset between runs.
   - **Mandatory Human Handoff Gates:** Exact points where login, 2FA, CAPTCHA/anti-bot challenges, restricted file uploads, or payment confirmation screens appear. **Never** generate instructions that bypass CAPTCHAs, paywalls, or access controls; instruct the agent to pause and hand off to the user.
4. Structure the output with [ui-playbook-template.md](./references/ui-playbook-template.md).

---

## Stage 3: Supervised 1-Task Dry-Run & Scar Capture

Unverified skills hallucinate steps. Before finalizing the skill:

1. **Run 1 Bounded Dry-Run:** Walk through one real read-only or draft task (halting **before** final payment, publish, or destructive submit) while the user supervises.
2. **Capture Failure Scars:** Every real-world snag discovered during the dry-run—promo modals blocking clicks, SPA tabs that lazy-load, downloads landing in a sandbox directory, or silent thumbnail exports instead of full media—must be written directly into `## Failure Scars & Guardrails`.
3. **Encode Deterministic Post-Checks:** Add concrete verification steps under `## Verification` (e.g., checking DOM confirmation state, HTTP/CLI exit codes, file non-zero byte size, or `ffprobe`/schema validation on exported artifacts) so future runs never claim success on a silent failure.

---

## Stage 4: Anti-Bloat Audit & Emit Package

Write the skill directory and verify all **6 quality gates** before finishing:

1. **Strict Trigger Hygiene:** YAML `name` matches `<slug>` (lowercase, hyphenated). `description` uses third-person, states exact trigger conditions, and ends with explicit `Don't use for...` exclusions so the skill never hijacks unrelated tasks.
2. **Progressive Disclosure:** `SKILL.md` is **under 250 lines** (hard ceiling: 500 lines). Deep tables or harness-specific reference notes live in `references/`.
3. **Zero Generic Filler:** Strip standard coding/browsing platitudes the frontier model already knows. Every line must earn its token cost.
4. **Zero Secrets:** Never write API keys, session cookies, personal tokens, or private account passwords into `SKILL.md` or `references/`.
5. **Explicit Safety & Auth Boundaries:** Login, CAPTCHA, and payment/destructive confirmation gates are explicitly marked **PAUSE FOR USER**.
6. **Cross-Harness Portability:** Save to the requested path (default workspace `.agents/skills/<slug>/SKILL.md`; symlink `.claude/skills/<slug>` when requested; see [harness-adapters.md](./references/harness-adapters.md)) and report the exact file links created.

# web2skill

**Turn any live web app, SaaS UI, or documentation URL into a lean, scar-tested `SKILL.md`—inside your own agent harness, with zero dependencies and zero extra API cost.**

Most "URL-to-Skill" scrapers just dump static documentation HTML into Markdown. That works fine for simple REST APIs (which Mintlify, GitBook, and `llms.txt` already expose natively), and fails completely on **real web applications** (Google Flow, Shopify Admin, YouTube Studio, Stripe Dashboard, internal ops tools)—where public help articles say *"click the + button"* and omit real URLs, resetting toggles, login/CAPTCHA handoffs, and silent export failures.

`web2skill` is a **pure-Markdown conversational meta-skill** (`0` Python/Node dependencies, `0` servers, `0` external API keys). It piggybacks on the web and browser tools your agent harness *already* has to:

1. **Check before reinventing** — probes `/.well-known/skills/`, `/skill.md`, `/llms.txt`, and your local skills first so you don't build redundant wrappers.
2. **Inspect the live UI or docs** — maps direct workspace URLs, semantic ARIA landmarks (never brittle XPaths), and version-accurate CLI/API syntax.
3. **Run 1 supervised dry-run ("Scar Capture")** — executes one bounded trial run with you watching, capturing real modal traps, sticky toggle resets, auth/payment pause gates, and deterministic post-checks (`ffprobe`, exit codes, DOM receipts) directly into `SKILL.md`.
4. **Emit a lean, portable package** — outputs a `<250`-line progressive-disclosure `SKILL.md` + `references/` that works across **Google Antigravity, Claude Code, Cursor, OpenAI Codex, Windsurf, and Gemini CLI**.

---

## When NOT to use `web2skill`

- **One-off browsing tasks:** SOTA visual/browser agents already handle single-use tasks on unseen websites zero-shot. Do not create a skill for something you will only run once.
- **Services with an official MCP server or CLI:** If an official CLI (`gh`, `gcloud`, `stripe`) or installed MCP plugin already covers the workflow cleanly, use that instead of browser UI automation.
- **Bypassing bot checks or paywalls:** `web2skill` never generates scripts to defeat CAPTCHAs, login walls, or access controls—it encodes explicit **Human Handoff Gates** where the agent pauses cleanly for you.

---

## Quick Install

### Option 1: Universal Agent Skills CLI (`skills.sh` — Claude Code, Cursor, Codex, Antigravity, Windsurf, Gemini CLI)

```bash
npx skills add VAMFI/web2skill
```

### Option 2: Direct Clone (Zero Node/NPX Required)

```bash
# Into a project workspace (shared with your team via Git):
mkdir -p .agents/skills && git clone https://github.com/VAMFI/web2skill.git /tmp/web2skill \
  && cp -R /tmp/web2skill/skills/web2skill .agents/skills/web2skill && rm -rf /tmp/web2skill

# Or globally for Claude Code:
mkdir -p ~/.claude/skills && cp -R .agents/skills/web2skill ~/.claude/skills/web2skill

# Or globally as a Google Antigravity plugin:
git clone https://github.com/VAMFI/web2skill.git ~/.gemini/config/plugins/web2skill
```

---

## How to Use

Just talk to your agent naturally and ask it to build a skill from a URL or web app:

### Example 1: Live Web Application UI (with Supervised Dry-Run)
> *"Use `web2skill` to build a reusable skill for `https://flow.google.com` so my agent can generate 16:9 Veo clips, lock Agent OFF, pause if Google asks me to sign in, and verify downloaded MP4 streams with `ffprobe`."*

### Example 2: Product Documentation / SDK / How-To URL
> *"Use `web2skill` on `https://orm.drizzle.team/docs/overview` to create a lean migration and schema skill in `.agents/skills/drizzle-ops`."*

### Example 3: Hardening a Flaky Existing Skill
> *"Use `web2skill` to dry-run `.agents/skills/shopify-refunds/SKILL.md` on a test order, record what trips it up, and add deterministic verification checks."*

---

## How the 4-Stage Loop Works

```text
Stage 0: Pre-Flight     ──▶ Checks /.well-known/skills/, /skill.md, /llms.txt & local skills first
Stage 1: Scope Lock     ──▶ Asks at most 2 questions (repeatable goal + auth/payment safety boundaries)
Stage 2: Adaptive Recon ──▶ Docs path (clean Markdown) OR Live UI path (session browser + ARIA landmarks)
Stage 3: Scar Capture   ──▶ Runs 1 bounded supervised dry-run; logs UI traps & deterministic post-checks
Stage 4: Anti-Bloat     ──▶ Emits <250-line SKILL.md + references/ with strict trigger exclusions
```

---

## Repository Structure

```text
web2skill/
├── plugin.json                                     # Native Antigravity / plugin bundle manifest
├── assets/logo.svg                                 # High-contrast plugin icon
├── PLAN.md                                         # Architecture & benchmark rationale
├── LICENSE                                         # MIT License (VAMFI Inc.)
└── skills/
    └── web2skill/
        ├── SKILL.md                                # Core conversational meta-skill
        └── references/
            ├── ui-playbook-template.md             # Template for SaaS & consumer web-app UI skills
            ├── docs-playbook-template.md           # Template for API, CLI & documentation URLs
            └── harness-adapters.md                 # Browser & path adapters (Antigravity, Claude Code, Cursor, Codex)
```

---

## License

MIT © [VAMFI Inc.](https://github.com/VAMFI)

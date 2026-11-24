# Product & Architecture Plan — `VAMFI/web2skill`

## 1. Why Open-Source Meta-Skill Instead of a Hosted SaaS Website

1. **Hosted URL-to-Skill SaaS has negative ad margins & zero moat:**
   - Developer ad RPMs (\$0.50–\$4.00 per 1,000 visits due to 40–70% ad-blocker usage) are 5x–25x lower than backend multi-page crawling + LLM token costs (\$10–\$40+ per 1,000 runs).
   - Static documentation scraping is already commoditized by Mintlify/GitBook native `/skill.md` endpoints, `skills.sh` (90,000+ skills), and Python CLIs like `Skill_Seekers`.
2. **SOTA Vision Agents vs. Procedural `SKILL.md` Memory:**
   - **One-off tasks on unseen websites:** SOTA multimodal browser agents handle these zero-shot (~80%+); creating a skill is unnecessary overhead.
   - **Repeatable multi-step workflows:** Zero-shot browser agents drop to ~30–50% reliability (WebArena) and ~12–25% (OSWorld) while burning 1.5x–2.7x extra tokens hitting UI dead ends. Even more critically, vision models **cannot see** private parameter locks, session/anti-bot rules, human pause gates (2FA, CAPTCHAs, payment confirmation), or deterministic post-checks (`ffprobe`, file integrity, DOM receipt verification).
3. **The Unbuilt Open-Source Wedge:**
   - Existing open-source tools either only interview the user in text (`anthropics/skills/skill-creator`), only scrape static docs via Python CLI (`Skill_Seekers`), or lock users into a custom scraper CLI (`browser-act-skill-forge`).
   - **`web2skill`** is a **zero-dependency, pure-Markdown conversational meta-skill** that runs inside *any* agent harness (Antigravity, Claude Code, Cursor, Codex, Windsurf, Gemini CLI) using the user's existing browser/web tools and subscription at **\$0 API/server cost**.

---

## 2. Core Design Principles

1. **Strict Trigger Hygiene (No Ceremony Bloat):**
   - `web2skill` activates *only* when a user explicitly asks to create, convert, or harden a reusable `SKILL.md` from a website, web app, or guide URL—never during ordinary browsing or coding tasks.
   - Every skill *generated* by `web2skill` enforces the same strict frontmatter trigger discipline so installed skills never hijack unrelated tasks.
2. **Pre-Flight Wheel Check (Stage 0):**
   - Always check `<origin>/.well-known/skills/`, `<origin>/skill.md`, and `<origin>/llms.txt` (plus already-installed workspace/global skills and MCP servers) before scraping or launching a browser.
3. **Supervised 1-Task Dry-Run & Scar Capture (Stage 3):**
   - For interactive web applications, never guess UI steps from public help articles. Inspect the live app through the harness's session-attached browser tool, perform one bounded dry-run with the user, and capture real **Failure Scars** (modals, wrong redirects, anti-bot blocks, flaky downloads) + **Human Handoff Gates** (login, 2FA, CAPTCHA, checkout).
4. **Progressive Disclosure & Portability (Stage 4):**
   - Keep `SKILL.md` under 250 lines; push deep API schemas or reference tables into `references/`.
   - Express UI steps via stable URLs, ARIA roles/labels, and visible text assertions, paired with a lightweight harness-adapter block.

---

## 3. Repository Layout

```text
web2skill/
├── LICENSE                                         # MIT (VAMFI Inc.)
├── README.md                                       # Quickstart, npx skills install, examples, anti-goals
├── PLAN.md                                         # Research & architecture rationale
├── plugin.json                                     # Antigravity / plugin bundle manifest
├── assets/
│   └── logo.svg                                    # High-contrast square SVG icon
└── skills/
    └── web2skill/
        ├── SKILL.md                                # Core 4-stage conversational meta-skill
        └── references/
            ├── ui-playbook-template.md             # Template for SaaS & consumer web-app UI skills
            ├── docs-playbook-template.md           # Template for docs, SDK, CLI & guide URLs
            └── harness-adapters.md                 # Browser & path adapters across major agent harnesses
```

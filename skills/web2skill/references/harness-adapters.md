# Cross-Harness Browser & Skill Directory Adapters

Use this reference during **Stage 2 (Path B — Live Web App Recon)** and **Stage 4 (Emit Package)** so `web2skill` works natively across every major AI agent harness without external dependencies.

---

## 1. Skill Install Paths by Harness

| Agent Harness | Workspace Skill Path (Recommended for Team/Repo) | Global Skill Path (Machine-Wide) |
| :--- | :--- | :--- |
| **Universal Open Standard (`npx skills`)** | `.agents/skills/<slug>/SKILL.md` | `~/.agents/skills/<slug>/SKILL.md` |
| **Google Antigravity** | `.agents/skills/<slug>/SKILL.md` | `~/.gemini/config/skills/<slug>/SKILL.md` (or plugin under `~/.gemini/config/plugins/<name>/skills/<slug>/SKILL.md`) |
| **Claude Code** | `.claude/skills/<slug>/SKILL.md` | `~/.claude/skills/<slug>/SKILL.md` |
| **Cursor** | `.cursor/skills/<slug>/SKILL.md` or `.agents/skills/<slug>/SKILL.md` | `~/.cursor/skills/<slug>/SKILL.md` |
| **OpenAI Codex / Gemini CLI / Windsurf** | `.agents/skills/<slug>/SKILL.md` | `~/.agents/skills/<slug>/SKILL.md` |

> **Tip for Multi-Harness Repos:** Write the canonical skill to `.agents/skills/<slug>/SKILL.md`. If the workspace also uses `.claude/skills/`, create a relative symlink (`.claude/skills/<slug> -> ../../.agents/skills/<slug>`) so one source of truth serves every harness.

---

## 2. Live Browser Recon & Execution Rules by Harness

When inspecting an unseen web app during **Stage 2 / Stage 3**, use the browser tool already mounted in your active harness:

### A. Google Antigravity (`browser` Subagent)
- **How to Recon & Execute:** Delegate UI inspection and dry-runs to the built-in `browser` subagent (`Model: "flash"` by default unless user specifies otherwise). Leave idle browser subagents alive between steps and resume the same tab with `send_message`.
- **Host Verification Rule:** Always verify claimed URLs, visited origins, and screenshots against the system `BROWSER_HOST_FACTS` message rather than trusting subagent prose alone.
- **Uploads & Downloads:** Stage upload files in `<Artifact Directory Path>/browser_sandbox/uploads/<filename>`, reference images in `<Artifact Directory Path>/browser_sandbox/references/<filename>`, and copy downloaded files out of `<Artifact Directory Path>/browser_sandbox/downloads/`.
- **Include in Generated `SKILL.md`:** Add a short `## Antigravity Harness Notes` subsection encoding `browser` subagent delegation, `BROWSER_HOST_FACTS` verification, and sandbox file paths whenever saving a UI skill for Antigravity.

### B. Chrome DevTools MCP (`chrome-devtools`)
- **How to Recon & Execute:** Use `list_pages` / `navigate_page` / `take_snapshot` (accessibility tree) first before `take_screenshot` to map clean ARIA roles, labels, and UIDs at low token cost.
- **Session Advantage:** Connects directly to the user's running Chrome profile, preserving existing logged-in sessions cleanly.

### C. Playwright MCP / `agent-browser` / Browser-Use
- **How to Recon & Execute:** Prefer persistent browser context / attached user session over fresh ephemeral headless instances when inspecting authenticated SaaS apps. Capture accessibility snapshots (`browser_snapshot`) to identify stable `getByRole` / `getByLabel` locators.

### D. Harnesses Without a Mounted Browser Tool (Fallback)
- If no interactive browser tool is mounted in the current harness:
  1. Fetch public routes/docs via web search or HTTP reader tools first.
  2. Ask the user to paste either a quick DOM/accessibility snippet, screenshot, or 3–5 bullet walkthrough of the logged-in screen, and mark untested steps clearly for dry-run verification once a browser MCP is enabled.

---

## 3. Portable Harness-Adapter Snippet (Optional Footer for UI Skills)

When a generated UI skill needs to run across multiple harnesses used by the same team, append this compact adapter block to the generated `SKILL.md`:

```markdown
## Harness Execution Rules

- **Session Browser Only:** Always drive this workflow through your harness's session-attached browser tool (Antigravity `browser` subagent, Chrome DevTools MCP, Playwright MCP persistent profile, or Computer Use)—never blind `curl` or ephemeral headless browsers that trigger bot blocks.
- **Antigravity:** Delegate browser steps to `browser` (`Model: "flash"`), reuse the open tab via `send_message`, stage uploads under `<Artifact Directory Path>/browser_sandbox/uploads/`, and verify outcomes against `BROWSER_HOST_FACTS`.
- **MCP Browser Harnesses (Claude Code / Cursor / Codex):** Prefer accessibility-tree snapshots (`take_snapshot` / `browser_snapshot`) and semantic `role` + `label` selectors over coordinate clicks or raw CSS paths.
```

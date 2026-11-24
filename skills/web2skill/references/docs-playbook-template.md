# Docs, CLI & API Playbook Template (`SKILL.md`)

Use this structure when converting technical documentation, SDK guides, CLI manuals, or written how-tos into a `SKILL.md` package with progressive disclosure. Keep `SKILL.md` under **250 lines** and move bulky tables/schemas into `references/`.

````markdown
---
name: <tool-or-service-slug>
description: >-
  Guides <exact tasks: setup, CLI commands, API integration, or workflows> for
  <Product/Library Name> (<version or API vX>). Use when working with <specific
  file types, packages, CLI commands, or endpoints>. Don't use for <unrelated products>.
---

# <Product / Service Name> Skill

## Prerequisites & Setup

- **Runtime / CLI Check:** Verify installation with `<tool --version>` (minimum: `<x.y>`).
- **Authentication:** Expect credentials via `<ENV_VAR_NAME>` or `<auth login command>`. Never hardcode keys in source files.
- **Source Docs:** Derived from `<canonical docs URL or llms.txt>` (checked `<YYYY-MM-DD>`).

## Core Workflows

### 1. <Primary Workflow Name>
1. <Concise procedural step with exact flags/headers>
   ```bash
   <minimal copy-pasteable command or request>
   ```
2. <Expected output or next step>

### 2. <Secondary Workflow Name>
1. <Procedural step>
2. <Procedural step>

## Non-Obvious Rules & Gotchas

- **<Gotcha 1 — Version / Deprecated API>:** Use `<new syntax>`, NOT deprecated `<old syntax>` commonly found in older training data.
- **<Gotcha 2 — Rate Limits / Pagination / Idempotency>:** Always pass `<header/flag>` when `<condition>`.
- **<Gotcha 3 — Error Recovery>:** If `<error code>` occurs, check `<specific root cause>`.

## Verification

- Run `<dry-run / lint / status / test command>` and confirm exit code `0` and `<expected output field>` before reporting completion.

## Progressive References

Read these files only when needed for deeper tasks:
- [api-reference.md](./references/api-reference.md) — Full endpoint parameters and response schemas
- [troubleshooting.md](./references/troubleshooting.md) — Error codes and edge-case recovery
````

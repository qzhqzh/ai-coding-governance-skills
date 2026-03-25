---
name: ai-coding-project-doc-bootstrap
description: "Initialize governance docs + progress logs for AI coding projects after a plan/architecture decision; create docs skeleton, invariants/interfaces/ADR placeholders, and context-map for directory-based governance."
---

# Notes
This Codex skill follows the portable workflow in `portable/PLAYBOOK.md`.

# AI Coding Project Doc Bootstrap

Use this skill when:
- The project already has an overall plan/architecture (even if rough), and
- You want to initialize a minimal `docs/` governance structure so future AI changes stay anchored to files, not just chat context.

## Goal
Create the minimum “governance assets” so a later maintainer skill can keep `docs/` and `progress/` in sync with code changes.

## Required Inputs (from the user or the repo)
- Current repo already has (or user will provide) an architecture/plan summary (it can live in `README.md`, `SPEC.md`, or an external doc).

If the repo does not have an architecture summary yet, still scaffold files, but put `[TODO]` placeholders.

## Steps (do in order)
1. **Create context map config (directory rules)**
   - Ensure `./.ai-coding/context-map.json` exists.
   - If it does not exist, create it with a generic default mapping for common layouts (core/adapter/data/tests/docs).
   - If it exists, do not overwrite; only add missing top-level keys if safe.

   Default `context-map.json` (use this exact structure if creating from scratch):
   ```json
   {
     "version": 1,
     "zones": {
       "docs": ["docs/**", "**/*.md"],
       "core": ["src/**/domain/**", "src/**/service/**", "core/**"],
       "adapter": ["src/**/api/**", "src/**/routes/**", "src/**/controllers/**", "src/**/handlers/**", "app/api/**"],
       "data": ["prisma/**", "migrations/**", "db/**", "database/**", "schema/**", "**/schema.prisma"],
       "tests": ["tests/**", "test/**", "**/__tests__/**", "**/*.test.ts", "**/*.spec.ts"]
     }
   }
   ```

2. **Create `docs/` minimal governance skeleton**
   Create these files if they do not exist (or create them with safe placeholders if empty):
   - `docs/architecture.md`
   - `docs/invariants.md`
   - `docs/interfaces.md`
   - `docs/adr/0001-initial.md`
   - `docs/progress/index.md`
   - `docs/progress/SINGLE_SLICE_TEMPLATE.md`

   Content expectations:
   - `docs/architecture.md`: headings for “Layers”, “Data flow”, and “Zone ownership”, with a short bullet list mapping to `context-map.json` zones.
   - `docs/invariants.md`: at least 3–6 invariants in a generic template; use `[TODO: replace]` placeholders.
   - `docs/interfaces.md`: at least 1 interfaces section and `[TODO]` placeholders; describe how errors/returns should be documented.
   - `docs/adr/0001-initial.md`: fill “Context/Decision/Consequences” from whatever plan summary you can find; otherwise keep `[TODO]`.
   - `docs/progress/index.md`: an index with an empty list and a “How to add entries” section pointing to `SINGLE_SLICE_TEMPLATE.md`.

3. **Initialize vertical slice log template**
   - In `docs/progress/SINGLE_SLICE_TEMPLATE.md`, write a single “vertical slice” log format (structure only) with:
     - `Slice ID`
     - `Goal`
     - `Plan summary`
     - `Boundaries` (zones touched)
     - `Implementation notes`
     - `Evidence` (tests run / API examples / screenshots links)
     - `Risks & invariants check`
     - `Next`
     - `Files changed`

   Use this exact minimum structure if creating the file:
   ```md
   # Vertical Slice Log: [Slice ID]

   ## Goal
   [What user-visible outcome this slice delivers]

   ## Plan summary
   [1-5 sentences referencing the plan/issue]

   ## Boundaries (touched zones)
   - docs: [yes/no + what]
   - core: [yes/no + what]
   - adapter: [yes/no + what]
   - data: [yes/no + what]
   - tests: [yes/no + what]

   ## Implementation notes
   - [Key steps and decisions]

   ## Evidence
   - Tests: [command(s) run + result summary]
   - Examples: [API/CLI/example outputs or links]

   ## Risks & invariants check
   - Invariants exercised: [invariant list or TODO]
   - Risks/unknowns: [what might regress]

   ## Next
   - [Next slice or follow-up tasks]

   ## Files changed
   - [path1]
   - [path2]
   ```

4. **Add a governance version marker**
   - Ensure `./.ai-coding/governance-version.txt` exists.
   - If missing, set to `1`.

## Output format
Reply with:
- Files created/updated (paths only)
- 3–6 bullet points summarizing what placeholders to fill next
- If `context-map.json` was created, include its default “zone names” (core/adapter/data/tests/docs) so the user knows how directory rules will work.


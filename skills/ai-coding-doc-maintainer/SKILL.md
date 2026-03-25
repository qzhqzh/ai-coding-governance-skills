---
name: ai-coding-doc-maintainer
description: "Maintain governance docs + progress logs on subsequent AI coding changes using directory-based zones; write vertical slice progress entries and keep docs in sync with code."
---

# AI Coding Docs Maintainer

Use this skill when:
- You want to implement/modify/fix functionality in the repo, and
- You want the AI to keep `docs/` + `docs/progress/` synchronized with code changes, anchored by directory semantics (not only chat memory).

## Goal
Guarantee that each meaningful change:
1) becomes a “vertical slice” progress entry, and
2) updates the right governance docs based on what code areas were touched.

## Required Inputs
- `./.ai-coding/context-map.json` must exist.
  - If it does not exist: ask the user to run `ai-coding-project-doc-bootstrap` first.

## Steps (do in order)
1. **Collect intent + planned paths**
   - Before editing, list the intended file paths to change (or the paths you expect to touch).
   - Derive “touched zones” from `context-map.json` (see Step 2).

2. **Load directory rules**
   - Read `./.ai-coding/context-map.json`.
   - It should define default zone names like: `docs`, `core`, `adapter`, `data`, `tests`.
   - Treat zone matching as a best-effort heuristic (do not block work).

3. **Create/append a vertical slice log**
   - Pick a `Slice ID`:
     - Use provided id if the user gives one, otherwise use current timestamp `YYYYMMDD-HHMMSS` + short suffix.
   - Create `docs/progress/<slice-id>.md` from `docs/progress/SINGLE_SLICE_TEMPLATE.md`.
   - Update `docs/progress/index.md` to link this new entry (keep it append-only).

4. **Update governance docs based on touched zones**
   Apply these generic rules (weak constraints; adapt names if your repo differs):

   - If `docs` zone is touched:
     - Record what docs changed inside the slice log (no additional doc updates needed).

   - If `adapter` zone is touched (e.g., routes/controllers/api handlers):
     - Ensure `docs/interfaces.md` captures the updated request/response/error semantics.
     - Ensure errors thrown/returned by code are represented in the interface document (add `[TODO]` if incomplete).

   - If `core` zone is touched (business/domain/service logic):
     - Update `docs/invariants.md` if any invariant or state transition semantics changed.
     - Update `docs/interfaces.md` if inputs/outputs contract semantics changed.

   - If `data` zone is touched (schema/migrations/db layer):
     - Update `docs/architecture.md` “zone ownership” and any data model notes.
     - If this is backward-incompatible, add an ADR entry under `docs/adr/` and reference it from the slice log.

   - If `tests` zone is touched:
     - In the slice log `Evidence`, record which tests were run and the key results (commands or summary).

5. **Governance alignment check (must be written into the slice log)**
   - List which invariants (from `docs/invariants.md`) you believe were exercised.
   - If you cannot prove an invariant holds, explicitly mark it under `Risks & invariants check` and put a concrete `Next` step.

## Output format (required)
Return:
- `Touched zones`: (comma-separated)
- `Slice log`: `docs/progress/<slice-id>.md`
- `Governance docs updated`: list of paths
- `Evidence`: tests run / snippets / example outputs (even if incomplete, mark `[TODO]`)


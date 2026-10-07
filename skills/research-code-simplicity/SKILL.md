---
name: research-code-simplicity
description: Implement or simplify research experiment code while preserving scientific semantics and avoiding redundant defensive machinery.
---

# Research Code Simplicity

Keep experiment code easy to inspect and change. Add checks where they prevent
plausible errors that affect execution or scientific interpretation.

## Own each contract once

- Validate external inputs at the boundary that owns them: command-line arguments,
  loaded files, checkpoints, or third-party responses. Internal code may rely on
  established contracts instead of repeating the same validation at every layer.
- Reserve expensive tensor scans and detailed diagnostics for a demonstrated risk,
  an appropriate ingestion boundary, or a requested debugging mode. Do not add them
  to every training step by default.
- Use hashes when they resolve a concrete integrity or identity requirement. Avoid
  routine full-tree hashing, repeated checkpoint hashing, and SHA equality as a
  universal acceptance condition. Existing paths, configuration snapshots, and run
  metadata often provide sufficient experiment traceability.
- Do not remove an existing integrity requirement merely because this skill favors
  simplicity; establish its purpose and narrow redundant enforcement where valid.
- Let meaningful failures remain visible. Avoid broad exception suppression, silent
  fallback configurations, fabricated missing metrics, and automatic repairs that
  change the experiment without making that change explicit.

## Preserve the scientific meaning

- Reuse existing launchers, metric implementations, configuration conventions, and
  result directories. Introduce an abstraction only when concrete duplication or
  multiple real consumers justify it; do not build a generic experiment platform
  for a local fix.
- Keep physical units, sampling timing, observation causality, termination behavior,
  and training exposure explicit where they affect a result. Simpler code is not a
  reason to weaken these contracts.
- When removing checks or fallbacks, inspect their callers and actual failure modes.
  Keep actionable error context at the owning boundary.

## Validate the changed behavior

- Choose focused checks for the affected behavior and run the project's required
  quality gates. Prefer a small meaningful runtime probe when pure unit tests cannot
  establish simulator or training integration.
- Reuse already-passed checks when their relevant inputs have not changed. Broaden
  validation when a failure or affected dependency provides a reason; avoid building
  a universal audit stack around a small reversible edit.
- Report whether evidence comes from static checks, unit tests, a runtime smoke
  test, or a completed experiment. For changes that may invalidate prior results,
  consult [result reuse](../../references/result-reuse.md).

---
name: experiment-planning
description: Plan research training budgets, pilot experiments, and seed expansion before launching or extending experiments.
---

# Experiment Planning

Choose the smallest experiment that can resolve the current scientific decision.
Reuse the project's experiment definitions and record decisions in its existing notes.

## Evidence and budget

- State the claim, comparison, intentional variable, and evidence that would justify
  continuing, changing direction, or stopping. Distinguish a wiring smoke test from
  a scientific pilot and from a result intended to support a paper.
- Before launching, define task-aligned primary metrics and success criteria using
  relevant papers and maintained project protocols. Freeze units, thresholds,
  temporal requirements, and aggregation; use fair-evaluation for metric design.
- For exploratory attempts, use one fixed training seed by default. Keep it fixed
  across comparable attempts to reduce avoidable variation. Expand to multiple
  independent training seeds after the candidate and configuration stabilize, or
  earlier when seed sensitivity is itself the question. Preserve an explicitly
  agreed experiment plan.
- Select an initial training budget using existing learning curves, a small pilot,
  task difficulty, and required curriculum exposure. Avoid large iteration margins
  added merely for reassurance: they multiply training, rerun, and reevaluation costs.
- Extend a budget when development evidence supports likely additional learning.
  Judge plateaus over a meaningful window; do not declare convergence from a short
  noisy segment. Record the reason for substantial budget changes.
- Account for recoverability when budgeting long runs. Keep schedules tied to
  cumulative progress; extending a horizon-dependent schedule can change the
  experiment and is not automatically equivalent to uninterrupted training.
- Compare actual environment transitions, samples, or tokens as appropriate, plus
  relevant compute cost. Equal iteration counts can hide different rollout lengths,
  parallel environment counts, or batch sizes.
- Verify actual exposure to required training conditions. A configured curriculum
  schedule does not establish that its later stages were reached.

## Selection and interpretation

- Define checkpoint selection and stopping using training or development evidence.
  Keep selection opportunities comparable across methods. Do not use the final
  evaluation set to choose checkpoints, stop training, or tune hyperparameters.
- Make the pilot evaluation large enough to answer its question without silently
  turning it into a full benchmark. A single training seed and many evaluation
  episodes do not establish robustness across training runs.
- Separate infrastructure or implementation failure from a scientifically negative
  result. A failed hypothesis is not automatically a reason to increase the budget.
- Record the chosen budget, seed scope, evaluation role, and output location in the
  existing experiment plan; do not introduce a new tracking system for these fields.
- Continue authorized work within that plan without repeating settled questions.
  Idle compute alone does not justify expanding the experiment scope or budget.

When deciding whether a change requires new training or evaluation, consult
[result reuse](../../references/result-reuse.md).

---
name: fair-evaluation
description: Design or review comparable research evaluations and aggregate results for experiment tables and scientific claims.
---

# Fair Evaluation

Use a shared evaluation protocol for direct comparisons, and disclose the variables
that the experiment intentionally changes.

## Define metrics before running

- Translate the core task into measurable behavior before experiments start.
  Consult relevant papers and maintained project metrics; record the sources and
  justify adaptations rather than copying a familiar score without its assumptions.
- Choose metrics that distinguish meaningful success, partial progress, and failure.
  A saturated binary score or an aggregate that hides the task's failure mode may
  need a small complementary metric, not an expanding catalog of diagnostics.
- Define success thresholds, temporal requirements, sampling, and aggregation
  before inspecting outcomes. For position-and-attitude holding, a single in-range
  sample establishes arrival, not sustained holding: require a specified continuous
  dwell interval within the pose tolerances. For new or revised metrics, read
  [metric design](../../references/metric-design.md).

## Comparable conditions

- Match the evaluation population, scenario and seed sets, requested sample counts,
  environment and physical conditions, noise and delay, control frequency, horizon,
  termination and reset rules, and metric definitions across directly compared rows.
  Mark intentional differences and limit the claim accordingly.
- Algorithm internals need not be identical. Match relevant information availability,
  tuning opportunities, and checkpoint selection rules; disclose training budgets
  using actual interactions or other appropriate units rather than iterations alone.
- Keep observations causal: a policy must not receive future measurements, labels,
  or privileged simulator state unavailable under the stated deployment conditions.
  Label an intentionally privileged oracle separately.
- Separate development selection from final evaluation. If final data has influenced
  tuning or selection, disclose that use instead of continuing to call it untouched.

## Measurement semantics

- Use one maintained metric implementation where practical. Establish each metric's
  unit, aggregation, sampling instant, and valid trajectory interval.
- Check terminal-state and reset timing so a reset observation cannot enter the
  preceding episode's metric. Apply time integration exactly once when converting
  rates or powers into totals.
- Distinguish requested, executed, validly measured, and successful episodes. Keep
  task failures in the appropriate denominator; never replace them with repeated
  attempts until success. Report infrastructure failures and missing measurements
  explicitly rather than converting them to zeros or successful outcomes.
- Pair success-conditioned metrics, such as recovery time among recovered episodes,
  with the corresponding success rate and sample count.

## Claims and uncertainty

- Identify the independent statistical unit. Many episodes from one trained policy
  do not count as independent training seeds. Aggregate and quantify uncertainty
  at the level required by the claim; retain pairing for shared evaluation scenarios.
- Treat single-seed results as exploratory unless the claim is explicitly restricted
  to that run. Add independent training seeds after selection stabilizes, consistent
  with the experiment plan.
- Compare alternative reward designs through shared external metrics, not their
  differently defined training rewards. Check that baselines and their selection
  rules support the breadth of the claimed improvement.
- Preserve result provenance through explicit run and checkpoint references using
  existing metadata. When conditions or measurement logic change, consult
  [result reuse](../../references/result-reuse.md) before mixing old and new results.

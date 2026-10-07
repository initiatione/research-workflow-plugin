# Task-aligned, discriminative metrics

Read when defining or changing evaluation metrics, not for every evaluation run.

Start with what successful task behavior means in plain language. Consult relevant
papers and maintained benchmark or project implementations for established metrics,
their populations, units, thresholds, and temporal semantics. Record the sources
in the existing experiment plan. If evidence is unavailable, mark an assumption
as provisional instead of inventing a literature justification.

Before launching the experiment, settle the primary endpoint, direction, units,
tolerances, time horizon, sampling, aggregation, and missing-data treatment. Keep
the definition fixed across compared methods. Revisions after seeing results are
exploratory changes and require affected results to be recomputed or reevaluated.

## Example: arrival versus sustained pose holding

For station keeping, success means maintaining both position and orientation near
the target. A single in-tolerance sample, or scattered in-tolerance samples whose
total duration is large, does not establish continuous holding.

Define position error, an appropriate orientation-distance measure, their units
and tolerances, and a required dwell duration before running. Decide whether the
task requires a qualifying dwell interval anywhere in the episode, holding through
the final interval, or maintaining the target after acquisition; these are different
claims. At each valid evaluation sample, both pose errors must meet their tolerances
for the required uninterrupted duration. An out-of-range sample breaks the interval
unless a predeclared, justified brief-excursion rule explicitly permits it.

Specify timestamps or sampling cadence and how duration is calculated; N samples
do not automatically span N sampling intervals. Do not bridge resets, episode ends,
missing measurements, or unobserved gaps into a successful dwell. Sampled evidence
supports holding at the stated measurement resolution, not an unobserved continuous-
time guarantee. A horizon shorter than the required dwell cannot test that criterion.

Use complementary quantities only when they distinguish task-relevant behavior:
time to settle, longest valid dwell, or steady-state pose error can distinguish
brief crossings, oscillation, slow convergence, and stable holding. Define their
populations so reporting only successful trials does not conceal failures.

## Discriminative does not mean favorable

Check metric meaning against a few conceptual or existing trajectories: one-time
crossing, repeated scattered hits, stable holding, sustained bias, and divergence.
If the same score erases important differences, improve the endpoint or add a
small complementary measure before the comparison. Do not move thresholds to
produce separation or rank a preferred method. A ceiling result may justify a
separately declared harder follow-up; it does not justify rewriting the original.

# Reuse results according to scientific impact

Read when an implementation, data source, training condition, or evaluation
protocol changes and existing results may be reused. This is not a routine gate.

Identify which measured quantities or learned parameters the change can affect.
Reuse unaffected results with their original run identity. Choose the smallest
repair that restores valid evidence:

| Change | Typical action, subject to available evidence |
| --- | --- |
| Wording or display precision | Regenerate affected presentation from source values. |
| Aggregation bug with sufficient valid raw measurements | Recompute affected summaries. |
| Evaluation timing, reset handling, or missing measurements | Reevaluate affected cells if stored traces cannot recover the measurement. |
| Physics, observations, labels, or reward used during training | Assess affected checkpoints and descendants; retrain when the intended claim requires training under the corrected conditions. |

A loadable checkpoint does not establish scientific equivalence. Conversely,
a changed file or digest alone does not prove that every result is invalid.
Use the actual dependency and semantic change to decide. If impact is uncertain,
run a focused comparison before committing to a full rerun.

Retain original outputs and distinguish repaired results. Never relabel old runs
as if they used a new protocol. Keep a short impact note in the project's existing
result index or experiment notes: what changed, what remains usable, what needs
replacement, and why. Do not introduce a new ledger or hash framework for this.

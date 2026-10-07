# Resume without silently changing the experiment

Read when implementing or repairing checkpoint/resume, or deciding whether a saved
run can be continued equivalently. Reuse a validated resume path when its relevant
state and semantics are unchanged; do not repeat a full audit for each restart.

The goal is to preserve training semantics and avoid material result changes caused
by interruption. Prefer complete state restoration. Do not promise bitwise equality
when device kernels or the simulator are nondeterministic or cannot restore state.

## Capture a coherent state

Use the framework's supported checkpoint facility and add only missing state that
affects this experiment. Save at a coherent boundary rather than halfway through
an update. Relevant state can include:

- Model parameters and buffers, optimizer moments, scheduler and mixed-precision
  scaler, target networks or moving averages when present.
- Cumulative steps, updates and consumed budget, curriculum stage and actual
  exposure, normalization statistics and best-checkpoint selection state.
- Python, numerical-library and device RNG states, per-worker generators, sampler
  positions, replay buffers, or unfinished rollout state when carried across saves.
- For stateful simulation: environment and episode state, recurrent hidden state,
  history or delay buffers, randomization state, and simulator RNG where supported.

Retain the resolved training configuration and data/source identity using existing
metadata. Publish checkpoints with the storage system's completed-write mechanism
(for example, write a temporary file then atomically rename on the same filesystem).
Keep the prior usable checkpoint until the new one is complete. Choose checkpoint
frequency to balance I/O cost against work lost to interruption; avoid new per-step
hashes or a general checkpoint framework when existing facilities suffice.

## Continue the original trajectory of training

Restore applicable state before consuming new randomness or samples. Continue the
remaining budget; do not reset learning-rate decay, curriculum, counters, or seed
streams. Account for any initialization that consumes or overwrites restored RNG.
In distributed training, preserve relevant worker state and topology assumptions.

Changing the total horizon may change a horizon-dependent learning-rate schedule;
that is a training-policy change even when all checkpoint tensors load correctly.
Label weight-only recovery, incompatible topology, or changed configuration as a
warm start or changed experiment when equivalent continuation cannot be established.

If simulator state cannot be restored, prefer a defined episode or rollout boundary
and document any state reset or discarded data. Such a boundary reduces disruption
but does not prove equivalence; do not silently treat it as exact continuation.

## Verify once at the affected boundary

For a new or changed resume path, compare a small uninterrupted run with a run
interrupted and resumed at the intended save boundary, under the same configuration
and budget. Check progress, schedules, next data or observations, and subsequent
updates and task-relevant behavior. Use exact comparison when deterministic; use
justified numerical tolerances otherwise. A short check detects restoration defects
but does not prove long-run statistical equivalence under substantial nondeterminism.

Report the supported equivalence and its limits. Do not reinterpret failed parity
as harmless noise without evidence, or add multi-seed training to every routine
resume check. Preserve run identity and separate restart attempts; a continuation
is not a new independent seed.

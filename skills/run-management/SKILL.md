---
name: run-management
description: Launch, monitor, resume, and organize long research training or evaluation jobs with resource-aware concurrency.
---

# Run Management

Keep long jobs asynchronous, identifiable, and recoverable using the project's
existing launchers, environment, and output layout.
Execute the agreed experiment and budget; do not add seeds or extend training
merely because resources are idle. Continue ordinary authorized work without
repeated approval requests.

## Launch and concurrency

- Run independent training and evaluation jobs concurrently when available resources
  permit. Consider peak GPU memory, GPU compute load, CPU, host memory, and I/O;
  free GPU memory alone is not sufficient evidence of capacity.
- Launch long jobs through a durable asynchronous mechanism available in the
  environment. A shell background process is sufficient only if it survives the
  controlling session and retains useful status and logs.
- Keep a run identity, launch command or resolved configuration, working environment,
  process or scheduler identity, log location, output directory, and recoverable
  completion or exit status. Use existing logs and metadata rather than inventing
  a scheduler or hash-based tracking layer.
- Check startup far enough to establish that the intended training or evaluation
  has begun. Asynchronous launch prevents blocking the agent; it does not prove
  the job is healthy or prevent deadlock.
- Isolate hardware competition when reporting latency or throughput, or use a
  disclosed, controlled concurrency setup shared by the compared measurements.

## Monitor and recover

- Poll at intervals appropriate to expected progress. Inspect a meaningful progress
  signal and error output rather than repeatedly reading the whole log.
- Before restarting after a tool timeout, disconnect, or stalled display, inspect
  existing jobs and child processes. A lost tool session does not mean compute has
  stopped. Avoid duplicate jobs and accidental interference with unrelated work.
- Retry a transient infrastructure failure only after checking its cause and the
  remaining budget. Stop automatic retries when the same cause persists; preserve
  the failure and report what requires intervention.
- Prefer recoverable training with complete checkpoints at coherent update or
  episode boundaries and an interval proportionate to save cost and lost work.
  Preserve the last usable checkpoint if a write is interrupted.
- Resume only when the saved state and configuration support the claimed continuation.
  Restore training progress and applicable optimizer, scheduler, RNG, normalization,
  data, recurrent, and environment state; weights alone are a warm start. Read
  [safe resume](../../references/safe-resume.md) when designing, repairing, or
  establishing a resume path. Reuse an established path for ordinary continuation.
- Treat continuation as the same run and seed with distinct attempt metadata, not
  an independent replicate. Disclose unsupported state or changed conditions rather
  than promising equivalence to uninterrupted training.

## Output ownership

- Give each experiment, method, seed, and attempt an unambiguous location within the
  current directory convention. Keep training checkpoints, evaluation outputs, and
  derived tables traceable without overwriting earlier attempts.
- Select aggregation inputs explicitly. Do not let a broad glob or a "latest"
  filename silently choose another experiment, retry, or incomplete run.
- On handoff or completion, report the run identity, state, artifacts, and any live
  jobs. Claim completion only after checking the exit outcome and required outputs.

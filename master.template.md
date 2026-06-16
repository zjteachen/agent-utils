---
type: master
task: simd-fast-invsqrt
---

# Master: SIMD fast inverse sqrt

## Background

Fast inverse square root currently runs scalar; the task is to enable it under SIMD so it
processes a vector of lanes per instruction. invsqrt sits in a hot path, so vectorizing it is
a throughput win proportional to lane width.

<!-- A bullet that feels thin is a candidate for an EXPANSION node. Don't expand pre-emptively. -->

## DoD (provisional)

- [ ] SIMD invsqrt within an acceptable error bound of scalar (bound TBD — see investigations).
- [ ] Measurable positive throughput vs. scalar baseline on target hardware (metric TBD).
- [ ] No regression in existing correctness tests for invsqrt consumers.

## DoD — proposed amendments

<!-- Agents append here. You ratify, then fold into the live DoD above and clear this slot. -->

_(empty)_

## Investigations index

<!-- One line + link per investigation. Depth lives in the linked file. -->

- _(none yet)_

## Work Units

<!-- AUTHORED BY YOU. Starts empty. The system does not suggest WUs by default.
     Status: open | in-progress | done @ <SHA>. One clean commit per WU. -->

- [x] **W0 — locate API seam + add vectorization-flag unit test.** Found the kernel-API path
  under which the casm changes sit; identified the toggle params for vectorization; added a
  unit test driving the result with the new flag. _done @ <SHA>_

- [ ] _(next WUs authored as understanding accrues)_

## Open questions (gating)

- Error tolerance the consumer actually requires (already-approximate algorithm; Newton-step count).
- Baseline + throughput metric definition.
- Per-lane intrinsic wrinkles (bit-cast, Newton refinement) — likely resolved while implementing.

---
type: investigation
title: Blast zone of invsqrt change
root: workflow/simd-fast-invsqrt/master.md
parent: workflow/simd-fast-invsqrt/master.md
status: open
takeaway: <one line, filled on completion>
---

# Investigation: blast zone

**Question.** What does the SIMD invsqrt change touch? Which call sites consume invsqrt, and
how tolerant is each to the (already-approximate) result changing?

**Findings.**

<!-- Agent writes the synthesized map here. Example shape:
- N call sites located: <paths>.
- M are approximation-tolerant (used in normalization that re-normalizes downstream).
- K feed a sensitive accumulation — candidate for a no-regression DoD criterion.
-->

**DoD implications.** _(If any, the agent also appends a proposal to the master's amendments
slot — e.g. "must not regress the sensitive accumulation path beyond X.")_

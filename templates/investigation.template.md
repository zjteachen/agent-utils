---
type: investigation
title: <short title>
slug: <kebab-case> # = filename
root: workflow/<task>/master.md # master whose DoD this may amend — ALWAYS the master, even when nested
parent: workflow/<task>/master.md # navigation only; may equal root, or another node if nested
status: open # open | done
takeaway: <one line, filled on completion — this is what the master index shows>
---

# Investigation: <title>

<!-- An INVESTIGATION opens a NEW line of inquiry the master's structure did not
anticipate — often unrelated to the immediate scope (e.g. "how does this service
handle distributed state?"). Distinct from an EXPANSION, which deepens a point that
already exists in the master.

Agent contract:
- Do the noisy exploration in your own context. Return to the parent ONLY the
  Question + Findings summary + takeaway. Depth stays in THIS file.
- Anchor findings to the MAIN files involved — name the handful that matter, don't
  cite everything.
- DoD impact is a PROPOSAL: append to the root master's `## DoD — proposed
  amendments` slot (follow `root:`). Never edit the live DoD.
- On completion set status: done and fill the takeaway, then add/refresh the one-line
  entry in the master's Investigations index.
- This file is itself expandable — link expansion nodes from Open threads below. -->

## Question

<!-- The specific thing being investigated. One or two sentences. Why it came up. -->

## Findings

<!-- The synthesized answer. Prose or bullets. This is the durable artifact — write
it so future-you (or an agent) can rely on it without re-doing the work. -->

## Main files

<!-- The handful of files this inquiry touches / that ground the findings. -->

- `<path>` — <what it is / why it matters>

## DoD impact

<!-- Does this change what "done" means? If yes, state the proposed criterion here AND
append it to the root master's proposed-amendments slot. If no, say "none". -->

-

## Open threads

<!-- Loose ends. Each is a candidate to EXPAND (deepen here) or spin into a further
INVESTIGATION (new line). Link child nodes as they're created. -->

-

# Workflow conventions for this repo

This repo uses a node-graph workflow for scoped tasks. Read this before acting on any
`workflow/` file.

## Files

- `workflow/<task>/master.md` — the master file. Single source of truth for the DoD (Definition of Done).
- `workflow/<task>/investigations/*.md` — investigation nodes (new lines of inquiry).
- `workflow/<task>/expansions/*.md` — expansion nodes (deepen an existing master slot).
- Work Units are tracked _in_ `master.md` and closed with a git commit SHA.

## Node front-matter (every node file starts with this)

```
---
type: investigation | expansion
title: <short title>
root: workflow/<task>/master.md      # the master whose DoD this may amend — ALWAYS the master, even when nested
parent: <path>                        # for human navigation only; may equal root
status: open | done
takeaway: <one line, filled on completion>
---
```

## Hard rules for agents

1. **Return bounded artifacts.** When dispatched to a node, do the noisy work in your own
   context and return only: a short summary (≤ ~5 sentences) + a `takeaway` one-liner. Put
   any depth in the node file, not in your reply to the parent. A treatise is a failure.
2. **DoD amendments are proposals, never edits.** If you learn something that should change
   the DoD, append it to the `## DoD — proposed amendments` slot in the **root** master file
   (follow the `root:` field, not `parent:`). Never edit the live `## DoD` directly.
3. **Never suggest Work Units unless explicitly asked.** The Work Units section is authored by
   the human. Only propose a WU when the human explicitly invokes the "suggest one first WU"
   request — and then propose exactly one.
4. **Overseer reads summaries only.** When asked to advise (next action / DoD distance), read
   `master.md` and node front-matter `takeaway` lines. Do not open every linked file. If you
   need depth on one node, say so and open just that one.
5. **Execution chatter stays out of the master file.** Syntax questions, bug checks, intrinsic
   lookups during drive-mode are ephemeral. Do not record them as nodes.
6. **Work Units are the review gate.** This is where code enters the repo, so this is where
   scrutiny lives — not in exposition or research. A delegated WU returns a reviewable diff +
   a note on assumptions, scoped to ONE clean commit, and the human reviews before it lands.
   Exposition and research return understanding (cheap to verify, self-correcting via expand) —
   do not apply WU-grade review friction there. Scale scrutiny to whether the output is a
   change to the codebase (review it) or understanding (cite the main files and move on).

## Custom subagents (in .claude/agents/)

- `investigate` — opens a new line of inquiry, writes an investigation node, may propose DoD amendments.
- `expand` — deepens an existing master slot, writes an expansion node.
- `overseer` — summary-layer advisor; reports unblocked work and DoD distance. Read-only on structure.

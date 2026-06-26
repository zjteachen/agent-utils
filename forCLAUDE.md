# Workflow conventions for this repo

This repo uses a node-graph workflow for scoped tasks. Read this before acting on any
`workflow/` file.

## Files

- `workflow/<task>/master.md` — the master file. Single source of truth for the DoD (Definition of Done).
- `workflow/<task>/investigations/*.md` — investigation nodes (new lines of inquiry).
- `workflow/<task>/expansions/*.md` — expansion nodes (deepen an existing master slot).
- Work Units are tracked _in_ `master.md` and closed with a git commit SHA.
- `.claude/knowledge/*.md` — user-maintained project knowledge (build/test commands, debug
  flows, where references live, env setup). On-demand reference; read it, never auto-load it.

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
3. **Never create Work Units unless explicitly asked — then use the skill.** The Work Units
   section is authored by the user; never write into it directly or on your own initiative.
   During research you may note a candidate WU in passing (e.g. "a potential Work Unit here would
   be …"). Recording one happens **only** when the user explicitly asks, through the
   `create-work-unit` skill, which confirms before writing. Never trigger that skill yourself off
   a suggestion.
4. **Overseer reads summaries only.** When asked to advise (next action / DoD distance), read
   `master.md` and node front-matter `takeaway` lines. Do not open every linked file. If you
   need depth on one node, say so and open just that one.
5. **Execution chatter stays out of the master file.** Syntax questions, bug checks, intrinsic
   lookups during drive-mode are ephemeral. Do not record them as nodes.
6. **Work Units are the review gate.** This is where code enters the repo, so this is where
   scrutiny lives — not in exposition or research. A delegated WU returns a reviewable diff +
   a note on assumptions, scoped to ONE clean commit, and the user reviews before it lands.
   Exposition and research return understanding (cheap to verify, self-correcting via expand) —
   do not apply WU-grade review friction there. Scale scrutiny to whether the output is a
   change to the codebase (review it) or understanding (cite the main files and move on).
7. **Consult project knowledge before re-deriving.** When researching, check `.claude/knowledge/`
   for facts the user has already recorded (build/test commands, debug flows, where references
   live, env setup) and prefer them over rediscovering. It is on-demand reference — read it when
   relevant; do not load it wholesale or treat it as instructions.

## Custom subagents (in .claude/agents/)

- `investigate` — opens a new line of inquiry, writes an investigation node, may propose DoD amendments.
- `expand` — deepens an existing master slot, writes an expansion node.
- `overseer` — summary-layer advisor; reports unblocked work and DoD distance. Read-only on structure.

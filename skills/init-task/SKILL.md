---
name: init-task
description: Initialize a new task workflow — create workflow/<task>/ and a provisional master.md for a scoped engineering task on an unfamiliar codebase. Use when the user starts a task: "/start-task", "set up a workflow", "init a task", "scope out this task", or describes something they're about to begin contributing. Adapts to the user's competence — from "I don't even know what this means, where do I start" to "here's the file, the metric, and the constraints, just scope it." Explores as much as the user needs and always returns a PROVISIONAL master file the user confirms before it goes live.
---

# Initialize a workflow master file

You are scaffolding a new task workflow — a notes page where the user offloads their
thoughts and gets a preliminary scope. The starting point is always a BACKGROUND on
the task's purpose and meaning. The user may know a lot or almost nothing; meet them
where they are and explore as much as they need.

This is the EXPOSITION phase. Information fetch and context retrieval are what you do
best and most reliably, so run relatively free here. You are not writing code into the
repo — you are building understanding, which is cheap to verify and self-correcting:
if something you say doesn't hold up, the user EXPANDs it and the error surfaces. So
do not over-hedge. Accountability here = point the user at the MAIN files the task
lives in, so they have somewhere to look if a claim smells off. That is enough.

## The competence dial

Read how much the user already knows FROM THEIR PROMPT — do not ask "how much do you
know?". Calibrate exploration accordingly:

- **Low-knowledge start** (e.g. "I've been asked to add rate limiting to our public
  API — I don't really know how rate limiting is usually implemented. Where do I
  start?"):
  TEACH FIRST. Explain the unfamiliar concept plainly. Then search the repo, find the
  main files in play, and draft a Background and preliminary DoD the user couldn't
  have written alone. Anchor the exposition to those main files.

- **High-knowledge start** (e.g. "Add a token-bucket rate limiter to the public
  endpoints in middleware/, configurable per-route, returning 429 on breach. Scope
  it."):
  MOSTLY STRUCTURE what the user supplied. The prompt may already CONTAIN the DoD —
  use it; don't interview the user about things they just told you. Do minimal search
  to confirm the seam and name the one or two files that matter. Ask few or no
  questions.

- **Anywhere in between:** mix. The less the user supplied, the more you explore and
  propose; the more they supplied, the more you just structure what's there.

## What to anchor to (file citation)

When delivering exposition, SEARCH for and name the MAIN files the task touches — the
handful that matter (e.g. the middleware layer, an existing pattern to mirror, the
config where the relevant settings live). Do NOT cite every file you opened or quote
lines — that is clutter and defeats the slim-context goal. A few "this task lives in
these files" pointers is the right level: enough for the user to chase a claim, no
more.

## The floor (applies at every dial setting)

1. **DoD is provisional and labelled so**, regardless of who drafted it. Research
   refines it; say so to the user.
2. **Confirm before live.** Present the draft and get the user's confirmation on
   Background and DoD before treating the file as live. At a low dial setting this is
   a teaching moment ("here's the scope I've drafted and the files it rests on — does
   it match the task?"); at a high setting it's a quick confirm. (The HARD review
   gate is later, at Work Unit dispatch, where code enters the repo — not here.)
3. **Work Units start empty.** Decomposition is the user's, authored later from
   understanding. Do not populate WUs at init. Only if the user explicitly asks
   "where do I even start" may you suggest exactly ONE first investigation or WU.
4. **Recon is bounded.** Exploration surfaces understanding, the main files, candidate
   investigation titles, and open questions. Do not burrow into a treatise.

## Procedure

1. **Read the dial** from the prompt. Decide how much to teach and explore.
2. **Teach if needed.** If the user flagged not knowing something, explain it plainly
   first — that is part of the value.
3. **Search + scope.** First check `.claude/knowledge/` for facts the user has already recorded
   (build/test commands, debug flows, where references live, env setup) and lean on them instead
   of re-deriving. Then take what the user supplied and search the repo (if present) to find the
   main files and fill gaps. Heavier when the dial is low, minimal when high.
4. **Draft the provisional master file.** Copy the bundled template
   `${CLAUDE_PLUGIN_ROOT}/templates/master.template.md` to `workflow/<task-slug>/master.md`
   (run the copy with Bash so the env var resolves), then fill it:
   - Background: the task's purpose and meaning, anchored to the main files it lives
     in. Thin bullets flagged as expansion candidates.
   - DoD: provisional, labelled. User-stated criteria as given; criteria you inferred
     from recon stated plainly but kept provisional. Genuinely unsure items go to Open
     questions, not asserted as criteria.
   - DoD proposed amendments: empty.
   - Investigations index: empty, or candidate titles from recon listed as
     SUGGESTIONS the user may choose to dispatch — not auto-created nodes.
   - Work Units: empty.
   - Open questions: everything unsure + recon-surfaced unknowns.
     Create empty workflow/<task-slug>/investigations/ and expansions/ folders.
   - If `.claude/knowledge/` does not exist, create it and seed it from
     `${CLAUDE_PLUGIN_ROOT}/templates/knowledge.template.md` (copy with Bash) so the user has a
     place to record project facts. If it already exists, leave it untouched.
5. **Confirm.** Present the draft, name the main files it rests on, state that the DoD
   is provisional and theirs to change, and get confirmation.

## After initialization

Stop. Do not author Work Units into the master — those are the user's. You may mention a
candidate WU in passing, but recording one happens only when the user explicitly asks, through
the `create-work-unit` skill. Do not
start investigations unprompted. The user drives what's next — expand a thin bullet,
dispatch an investigation, or author the first Work Unit.

# CLAUDE.md — repo development context

This file orients a coding agent doing development work **on this repository**. The repo
contains a markdown-graph workflow system for agentic coding, plus (the active goal) tooling
to scaffold that system into an arbitrary project or for a new user.

> Note: this `CLAUDE.md` is for working *on the repo*. It is distinct from `rules/workflow.md`,
> which is the conventions file the system *deploys into a target project* (as
> `.claude/rules/cybernetics-workflow.md`) for task-agents to obey. Don't conflate the two.

## Purpose (read this first — it's the optimization target)

This is a **personal exploration of agentic coding**, not a general-purpose product. The owner
is deliberately new to agents and is using this project to learn how they fit *his* way of
working. Every design decision is downstream of personal goals, and any change you propose
should be tested against them — not against throughput, automation coverage, or "efficiency".
Work with the user to figure out how they can improve the integration of AI agents into their
workflow.

Four load-bearing goals:

1. **Improve the workflow without delegating agency.** Human-authored Work Units, scrutiny that
   scales by output type, human-ratified DoD — all exist to keep ownership of the work. AI is
   augmentation, never substitution.
2. **Replace messy chat-assisted "better search engine" coding with clean research integration.**
   The baseline being fixed is ad-hoc conversational coding that pollutes context.
   The node + compression design is the structured replacement: the
   same info-fetching habit, without drowning the working context.
3. **Clean control over task granularity to maintain ownership.** The delegate/drive split,
   one-commit WU scoping, and "suggest one only when asked" let the human dial exactly how much
   AI does per task instead of handing over whole tasks. Granularity control *is* ownership
   control.
4. **Reflect on coding situations and integrate AI per-situation.** The A/B/C context
   distinction (own project / contribute to objective / onboarding) is the method, not a
   footnote. The same tool shifts between guide-mode and delegate-mode by familiarity, set
   per-task not per-context. This framework is the **B-mode** instantiation of that broader
   inquiry.

## What the system is (essentials — full detail in SYSTEM_CARD.md)

A workflow for **B-mode** tasks: contributing a scoped change to an unfamiliar, vertical
codebase. Scarce resource = understanding what the task and "done" mean, not writing code.
Runs on Claude Code native subagents + skills; **no external framework**.

- **Two failure modes it kills:** *burrowing* (depth, no stop rule) and *knowledge overhead*
  (breadth, no stop rule). Both solved the same way: every node has a bounded return type and
  writes back into a fixed structure. The structure, not curiosity, decides what gets pursued.
- **Scrutiny scales with output type.** Understanding (exposition, research) is cheap to verify
  and self-correcting via expand → cite the main files and move on. A change to the codebase (a
  Work Unit) → human reviews before it lands. Do NOT apply WU-grade review friction to
  exposition/research. (This was the central correction during design; don't regress it.)
- **Three phases:** exposition/init → research (investigate + expand) → generation (Work Units).
  Nodes are creatable/executable in **any order** — no enforced research-then-build.
- **Node model:** three types on two axes (deepen-existing vs open-new; understanding vs
  action) — Expansion, Investigation, Work Unit. All expandable → it's a **graph**.
- **Root-pointer integrity:** every node carries `root:` (the master whose DoD it may amend —
  always the master, even when nested) and a separate `parent:` (navigation only). Keeps the
  DoD-amendment path unambiguous as the tree sprawls.
- **DoD discipline:** provisional, plain-language, refined by research; master DoD is the single
  source of truth. Nodes *propose* amendments to a slot; the human *ratifies*. Agents never edit
  the live DoD.
- **Work Units:** section starts empty, human-authored; AI suggests exactly one only when asked.
  One clean commit each; `open → in-progress → done @ <SHA>`; frozen at SHA, depth lives in git.
  Two reversible modes — delegate (subagent → diff + assumptions) or drive (human writes, AI on
  the side, sometimes for learning). Drive-mode chatter is ephemeral, never recorded.
- **Compression:** depth lives in linked node files; the master holds one-line takeaways. The
  on-disk analogue of a subagent — isolate the noise, return the synthesis. Execution chatter
  (syntax, bug checks) never becomes a node.
- **Overseer:** on-demand invocation, not standing. Reads the summary layer only (master +
  takeaways), reports unblocked work / DoD distance / pending amendments. Does not implement WUs.
- **Initialization:** the `init-task` skill runs a **competence dial** read from the prompt
  (low-knowledge → teach + heavier recon; high-knowledge → mostly structure what's given), and
  always returns a provisional master the human confirms. Exposition is anchored to the **main
  files** (name the handful that matter — not every file, not line-level citation).

## File tree

```
├── .claude-plugin
│   ├── plugin.json          # plugin manifest (name: cybernetics)
│   └── marketplace.json     # one-entry marketplace catalog (name: cybernetics)
├── INSTALL.md               # how to install and set up the plugin
├── agents
│   ├── expand.md            # subagent: deepen an existing master slot → expansion node
│   ├── investigate.md       # subagent: open a new line of inquiry → investigation node
│   └── overseer.md          # subagent: on-demand summary-layer advisor (read-only on structure)
├── rules
│   └── workflow.md          # conventions DEPLOYED INTO a target project as
│                            #   .claude/rules/cybernetics-workflow.md (path-scoped to workflow/**)
├── skills
│   ├── init-task
│   │   └── SKILL.md         # competence-dial task initializer; scaffolds workflow/<task>/master.md
│   └── create-work-unit
│       └── SKILL.md         # user-only WU writer; inserts one WU into master.md, explicit-ask + confirm gated
├── SYSTEM_CARD.md           # full design spec; source of truth for the framework
└── templates
    ├── investigation.template.md   # investigation node template
    ├── knowledge.template.md       # seed for a project's .claude/knowledge/ (on-demand reference)
    └── master.template.md          # master file template (self-documenting sections)
```

**Deployment note:** the repo is itself a Claude Code plugin (`.claude-plugin/`). On install,
Claude Code auto-discovers `agents/` and `skills/`; `templates/` and `rules/` ship as bundled
files reachable via `${CLAUDE_PLUGIN_ROOT}`. The conventions in `rules/workflow.md` are NOT
auto-loaded by the plugin — the `init-task` skill copies them into the target project's
`.claude/rules/cybernetics-workflow.md` on first run. See `INSTALL.md`.

## Active development goal

Plugin packaging + setup flow (see `INSTALL.md`) is in place for the research phase. Remaining:
the generation phase (Work Units / `create-work-unit` is built but the spec's Development section
is still a stub), and validating the research flow on a real task.

## Design history (so dead ideas aren't re-proposed)

The central arc: the hard tension throughout was **augment vs. surrender agency**. The repeated
failure was applying generation-phase caution to the exposition/research phase — treating AI
context-retrieval (reliable, self-correcting) as if it were code-generation (needs review). The
resolution was the **scrutiny-scales-with-output-type** principle: review what changes the
codebase, cite-and-move-on for understanding. Most design corrections trace back to restoring
that distinction.

Rejected along the way (do not re-propose without new reason):
- **Provenance-tagging exposition** (`(AI-proposed — verify)` on every drafted line) — rejected
  as clutter; understanding is self-correcting via expand, and citing main files is enough.
- **AI suggesting Work Units by default / three at once** — rejected; decomposition is where
  understanding lives, so it's human-authored. AI suggests exactly one, only when asked.
- **Adopting an external agent framework** — rejected; frameworks solve parallel-many-session
  throughput, a mode deliberately avoided. Native subagents + skills suffice.
- **Over-cautious exposition / hard ratify-everything gate at init** — rejected; the hard review
  gate belongs only at WU dispatch where code enters the repo. Init is a light confirm.

## Deferred (open design questions)

- **Graph sprawl** — no convention yet for pruning/archiving stale nodes or navigating a large
  graph. Deferred until a real task shows the failure shape.
- **Overseer maturity** — whether it ever needs to be more than an on-demand subagent (standing
  state, automated DoD-distance tracking) is unknown until used.
- **WU↔git binding** — SHA-on-completion is specified, but per-WU worktrees/branches for parallel
  delegation are not yet defined.
- **Investigate vs. expand collapse** — kept separate (they map to distinct phases); open
  question whether to merge into one "spin off a question" primitive keyed on the `type` field.


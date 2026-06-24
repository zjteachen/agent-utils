# Workflow System Card — B-mode Agentic Coding

This system is designed to be a structure for contributing a scoped task to an unfamiliar, vertical codebase, where the
scarce resource is _knowing what "done" and "correct" mean here_ — not writing the code.
The system keeps the human as architect of understanding and decomposition, and uses agents
for bounded reconnaissance that returns compressed artifacts.

## Core principle

Every piece of research must terminate by writing into a fixed structure. The structure —
not curiosity — decides what is worth pursuing. This defeats the two failure modes:

- **Burrowing** (depth without a stopping rule): killed because each node has a _return type_.
  A node that returns a treatise instead of its scoped answer has failed, regardless of how
  interesting the treatise is. Depth goes in the linked file; the index gets the takeaway.
- **Knowledge overhead** (breadth without a stopping rule): killed because you hold a shallow
  map and deepen only what gates the work. Most slots are never expanded. That is the feature.

## The master file is a coordination layer, not an execution layer

It holds _task structure_ you authored or ratified. It never holds execution chatter
(syntax questions, "check this for bugs," intrinsic lookups) — that is ephemeral, local to
a node's working session, and evaporates. The single source of truth for "done" is the
master file's DoD, and only that.

## Node taxonomy

Three node types, separated on two axes: _deepen an existing slot vs. open a new line_, and
_return understanding vs. return action_.

| Node               | Opens                                        | Returns                        | Lands in master file as                         | Can propose DoD change |
| ------------------ | -------------------------------------------- | ------------------------------ | ----------------------------------------------- | ---------------------- |
| **Expansion**      | deepens an _existing_ slot                   | understanding                  | the deepened bullet itself (+ link to depth)    | yes                    |
| **Investigation**  | a _new_ line the structure didn't anticipate | understanding                  | one-line entry in Investigations index (+ link) | yes                    |
| **Work Unit (WU)** | an actionable change                         | a diff, closed at a commit SHA | entry in Work Units section (authored by you)   | yes                    |

- **Expansions** correspond to the _deepening_ phase: a known thing is too thin.
- **Investigations** correspond to the _discovery_ phase: a question the original framing
  couldn't contain (e.g. "what is the blast zone?").
- **WUs** are the _action_ phase.

All three are expandable (investigations can themselves be expanded), producing a graph.

## Graph integrity: root-pointer, not parent-pointer

Nesting produces a graph. The one operation that must stay coherent across depth is
_DoD amendment_. Therefore every node carries a **root link** = the master file whose DoD it
may propose amendments to. A node three levels deep proposes to the _master_ DoD, never to
its immediate parent. A separate **parent link** exists only for human navigation. Keep these
distinct: the file tree may get tangled (deferred problem) without the amendment path ever
becoming ambiguous.

## DoD discipline

- DoD starts as a **provisional, plain-language guess** written before research. Research
  exists to _falsify and refine_ it, not to precede it.
- DoD accretes discrete checks/tests as recon confirms what "done" means.
- Any node may **propose** a DoD amendment; it lands in a `proposed amendments` slot.
- Amendments are **ratified by you** (optionally on the overseer's advice) before they
  modify the live DoD. The DoD is too load-bearing to accept blind writes — same principle
  as reviewing diffs.

## Work Units

- The Work Units section **starts empty**. WUs are **defined by you**, as an output of
  research you understood — decomposition is where the understanding lives, and that judgment is
  yours to build. The system **cannot directly modify or create Work Units**, but can suggest
  them in conversation. Creating one is a deliberate user action through the `create-work-unit`
  skill, which transcribes a WU you've architected — only when explicitly asked, and with
  confirmation before it writes.
- A single explicit **"suggest one first WU"** stuck-button exists for when you're truly
  stuck. It proposes _one_, only when asked, never unprompted, never three.
- Each WU is **scoped to one clean reviewable commit**. If it can't be one commit, it's two
  WUs. Status: `open → in-progress → done @ <SHA>`. Once a SHA, the node is frozen; its depth
  (the diff) lives in git, not in markdown.
- WUs request review by being **small**, not by pausing mid-task. A subagent is one-string-in,
  one-string-out: many small dispatches, each return is inherently a review point. Wanting an
  agent to check in mid-stream is the signal the WU was too big.

## Execution modes per WU (reversible, per-node)

- **Delegate** — a subagent implements the chunk and returns the diff + assumptions.
- **Drive** — you write it; AI runs alongside as guide-and-search (your existing habit). The
  drive-mode chatter is ephemeral and never enters the master file.

The node doesn't change; only who holds the pen does. Switching is per-WU and reversible.

## Ordering

Nodes are creatable and executable in **any order**. There is no enforced research-then-build
phase — both are just nodes on the graph. "Stop research early and do something actionable" =
leave Open Questions unexpanded and create a WU now. The stopping condition for research is
**not** "I understand everything" — it's "no remaining Open Question blocks defining a WU."
Unknowns you can resolve _while_ implementing are not blockers.

## Compression (why links, not inline)

A link decouples _depth of research_ from _cost in context_ — the coupling that causes both
traps. The full breakdown lives in a linked file; the master file holds a one-line summary +
link. Future context loads the summary; depth is retrievable but not resident. This is the
on-disk analogue of what a subagent already does in-session: isolate the noise, return only
the synthesis.

## The overseer

- **On-demand only**, its own invocation — not a persistent process. Start as a subagent you
  invoke: "read the master file, tell me what's unblocked and how far from DoD."
- Operates on the **summary layer**: master file, DoD, node index + one-line states. It does
  **not** pull every linked file into context (that would reintroduce knowledge-overhead at
  the meta level). If it needs depth on one node, that is itself a dispatch.
- It advises on next action, DoD-distance, and ratification of proposed amendments. It does
  **not** work on WUs and structurally cannot see execution chatter (that was never written
  to the summary layer).

## Minimal-rot guidance

The overseer and master file are the parts most likely to bloat past the guide-and-search
habit you trust. Cheapest viable version: the master file is a markdown file you maintain by
hand; the overseer is an on-demand subagent invocation. Make nothing standing until it earns
its keep.

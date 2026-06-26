# Agent Workflow System Card

## Overview

This is an agentic framework designed to offer high granularity to developers who wish to maintain ownership of their work.
It aims to solve problems related to context pollution, over-delegation, and knowledge debt from overuse.

This framework is developed by thoroughly analyzing the personal programming workflows of the author and identifying key scenarios where AI usage can be most effective in _assisting_ the programmer.
The system keeps the human as architect of understanding and decomposition, and uses agents and AI for heavily scoped work.

Currently, this framework targets 3 scenarios.

This system is constantly evolving through iterative changes.

### Scenario A

The developer is working on a personal project or system where they have partial or complete ownership of the system architecture. Thus the agent will need to assist heavily with architectural design and long-term project planning. The developer will also be more comfortable with delegating larger portions of the system due to increased knowledge.

### Scenario B

The developer is contributing a task to a project they do not own; most common scenario would be working at a company. In this instance the priority is to assist the user while allowing the user to naturally onboard and prevent "knowledge debt", where the user creates code they are not capable of answering for.

### Scenario C

The developer is onboarding onto a completely unknown codebase and does not have a specific task in mind. The most analogous example would be trying to contribute to an open-source project. In this case research and knowledge acquisition is paramount. The developer needs to be able to identify a suitable area of work and a way to begin tinkering, or a small improvement to work on.

## System Capabilities

### Tasks

It's a common abstraction to consider a unit of work with regards to software development as a "task". This system provisions several utilities for managing the development lifecycle of these tasks and providing users with sufficient granularity to delegate work to AI agents without removing the essence of software development.

Available Tools:

- `init-task` (skill): This creates a "task" file that becomes the master node for research. The user provides the task name and description. A `master.md` is created, and an AI agent populates the file with bullets describing the purpose, background, and important files relative to the task. Also, a provisional DoD (Definition of Done) is created, describing what the user should aim for.
- `overseer`: Assesses the task file, and gives the user a "status report" with regards to where the task is relative to the current DoD. The overseer can also suggest next steps or unblocking advice. READ-ONLY - never modifies files.

#### Other Notes

_The master file is a coordination layer, not an execution layer_
It holds _task structure_ you authored or ratified. It never holds execution chatter
(syntax questions, "check this for bugs," intrinsic lookups) — that is ephemeral, local to
a node's working session, and evaporates. The single source of truth for "done" is the
master file's DoD, and only that.

_DoD discipline_

- DoD starts as a **provisional, plain-language guess** written before research. Research
  exists to _falsify and refine_ it, not to precede it.
- DoD accretes discrete checks/tests as recon confirms what "done" means.
- Any node may **propose** a DoD amendment; it lands in a `proposed amendments` slot.
- Amendments are **ratified by you** (optionally on the overseer's advice) before they
  modify the live DoD. The DoD is too load-bearing to accept blind writes — same principle
  as reviewing diffs.

### Research

AI is extremely good at information retrieval and can excel at onboarding programmers when used properly.
The personal failure modes experienced by the author of this code, when using AI and agents for research, are:

- Over-burrowing - an AI chat can cause the user to lose focus and "burrow" too deep on a specific topic.
- Knowledge Paralysis - the user may over-investigate across too many domains and be "stuck" on how to actually begin writing code.
  With this agentic framework, research is managed through an expanding-node markdown system.

#### Available Tools:

- `expand`: User requests to deepen knowledge of something mentioned in a markdown file. The agent researches, reports back with details, and writes the important details back to the markdown file. If expansive, the agent can create a new markdown file and link to it in the original markdown.
- `investigate`: User wants to research an additional topic in the markdown file. The agent researches and reports the results, then adds the details back to the file.

#### Other Notes

_Node taxonomy_

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

_Graph integrity: root-pointer, not parent-pointer_

Nesting produces a graph. The one operation that must stay coherent across depth is
_DoD amendment_. Therefore every node carries a **root link** = the master file whose DoD it
may propose amendments to. A node three levels deep proposes to the _master_ DoD, never to
its immediate parent. A separate **parent link** exists only for human navigation. Keep these
distinct: the file tree may get tangled (deferred problem) without the amendment path ever
becoming ambiguous.

_Project knowledge (`.claude/knowledge/`)_

A user-maintained, **on-demand** reference the framework consults but never auto-loads. It holds
durable facts the user already knows and would otherwise re-explain every session: build/test
commands, common debug flows, where references live (`$GITTOP/docs/...`), env setup, gotchas.
Starts with whatever the user jots; common debug flows are the expected first content.

- **On-demand, not ambient.** Unlike `.claude/rules/` (loaded every session), knowledge is read
  only when an agent's current work calls for it. It is a library to pull from, not context that
  is always resident — so it does not tax the slim-context goal.
- **User-written.** Distinct from auto-memory (Claude-written learnings). This is the user's own
  reference, the project-wide, cross-task analogue of the per-task master file.
- **Consulted by the research phase.** `init-task`, `investigate`, and `expand` check it
  before re-deriving project facts — a user-known build command or doc location beats
  rediscovering it. See the consult directives in those agents/skills and `rules/workflow.md`.
- **Generalizes beyond B-mode.** The same folder is where A-mode architectural planning would
  live later; keep its contents mode-agnostic.

### Development

This is a Work In Progress. The current idea is to make the _user_ provision Work Units (WUs) in a relevant markdown file. Then the user can choose to delegate or manually do each WU.

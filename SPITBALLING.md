# SPITBALLING.md — unpolished ideas

Scratchpad for half-formed ideas being considered for the spec. **Nothing here is committed
design.** `SYSTEM_CARD.md` is the source of truth; this file is the staging area before it.

Graduation path: idea here → discussed/refined → folded into `SYSTEM_CARD.md` + implemented →
removed from this file.

In the spec but still being refined: project knowledge folder (`.claude/knowledge/`) — see
Idea 3.

---

## Working note: framework stays out until invited

The framework should not interfere when freshly loading into a project or examining things
manually — that should feel like a normal repo. It engages when the user opts in. The opt-in
marker is the interesting part (see branch ideas below): an ambient mechanism is acceptable
*when it's gated behind an explicit marker* like a registered branch — the gate is what makes it
okay, not avoiding ambience.

---

## Idea 1 — `/newtask` skill (branch ownership + scope)

**What:** one command to start a B-mode task. Claude takes ownership of a branch, then scaffolds
the workflow (calls `init-task`'s scoping logic) and records the branch in the master.

**Branch ownership logic** (how `/newtask` decides which branch):
1. User explicitly OKs the current branch → use it.
2. User says "create a branch `<name>`" → create that branch.
3. Neither → Claude infers a name from the task and creates the branch.

**Other pieces & feasibility:**
- Branch creation: trivial via Bash (`git switch -c <name>`).
- `init-task` stays the reusable scoping core; `/newtask` orchestrates (own branch → scope).
- Record branch in master: add a `branch:` field to the master front-matter (currently
  `type` / `task`). One-liner.
- Branch ↔ task mapping: lean `branch == task slug == folder`, so `workflow/<branch>/master.md`
  is constructible from `git branch --show-current`. Breaks on rename (acceptable for now).

**Open questions:** branch naming prefix (`task/<slug>`?); >1 task per branch (assume 1:1);
interaction with per-WU worktrees (deferred WU↔git binding in SYSTEM_CARD).

**Maturity:** close to spec-ready.

---

## Idea 2 — Branch as the "framework-integrated" marker + registration

**What:** a branch is the encapsulation boundary for "this environment is framework-managed."
Being on a *registered* branch is the opt-in marker; off it, the framework stays silent.

**Mechanism sketch:**
- `/newtask` **registers** the branch (the `branch:` field in the master is the registry; or a
  small index maps branch → task).
- A **git hook** (`post-checkout`) fires on branch switch, queries whether the new branch is
  registered, and if so surfaces a one-line pointer: *"Active task: <name> — see
  workflow/<branch>/master.md"*. Factual pointer, not behavior — consistent with the working
  note above.
- On an unregistered branch the hook does nothing → no interference.

**Feasibility / unknowns:**
- `post-checkout` is a standard git hook; detecting the branch and grepping `branch:` fields is a
  shell one-liner. The open question is *plumbing*: a git hook runs outside Claude, so how does
  its result reach a Claude session? Options to explore: the hook writes a small state file the
  session reads on demand; or a Claude-side `SessionStart` hook (plugin-shipped) does the
  branch→task lookup directly at session start instead of relying on the git hook. The git-hook
  route notifies on *switch*; the SessionStart route notifies on *new session* — may want both.
- Still factual-only: a pointer, never injected behavior. Loading the master stays explicit.

**Open question for the user:** is even the one-line pointer too much, or is that the right
amount of "you're in a task now"?

**Maturity:** needs the plumbing decision (git hook vs SessionStart vs both).

---

## Idea 3 — Project knowledge (`.claude/knowledge/`)

In the spec (Research → Other Notes) and wired into `init-task`/`investigate`/`expand`, but the
mechanics are unsettled and parked here for refinement.

**Open questions:**
- *Discovery:* how do agents decide *when* to read it — always-consult-on-init, or keyword/title
  match against files? Currently just "check it first," which may be too blunt.
- *Structure:* freeform vs. a light schema (the seed template is freeform for now).
- *Scope:* project-only, or also a user-level `~/.claude/knowledge/` for cross-repo facts.
- *Authoring:* hand-edited only, or a `/jot` skill to append without opening the file.

**Maturity:** usable as-is for testing; revisit discovery + scope after the research flow is tried.

## Cross-cutting notes

- **Two axes.** `/newtask` → branch registration → task pointer → master file are the
  *task-lifecycle* axis (per-task). `.claude/knowledge/` (now in the spec) is the *project-wide,
  cross-task* axis. Together: per-task working memory + project-wide reference.
- **Maturity ranking:** Idea 1 (`branch:` field + `/newtask`) is nearly spec-ready; Idea 2 needs
  the notify-plumbing decision and the user's call on how much signal is wanted.

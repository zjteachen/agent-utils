# Installing cybernetics

Cybernetics is a Claude Code **plugin**. Installing it gives you the workflow skills and recon
subagents; running `init-task` in a project then sets that project up to use them.

## Prerequisites

- Claude Code (recent version with plugin support).
- A target project that is a git repository.

## 1. Add the marketplace and install the plugin

This repo is both the plugin and its own one-entry marketplace (`name: cybernetics`).

**Local (for testing now, no push needed):**

```
/plugin marketplace add /home/teahousechen/aiml/cybernetics
/plugin install cybernetics@cybernetics
```

**From GitHub (once this repo is pushed to its remote):**

```
/plugin marketplace add zjteachen/agent-utils
/plugin install cybernetics@cybernetics
```

Verify with `/plugin` — `cybernetics` should show as installed and enabled.

## 2. What you get

Auto-discovered on install:

- **Skills:** `init-task` (scaffold a task + master file), `create-work-unit` (insert a Work Unit;
  explicit-ask + confirm gated).
- **Subagents:** `expand`, `investigate` (research), `overseer` (read-only status/DoD advisor).
- **Bundled files:** `templates/` and `rules/`, reachable by the skills via `${CLAUDE_PLUGIN_ROOT}`.

## 3. Set up a project

In your target project, run:

```
/init-task
```

On first run this scaffolds, only if absent:

- `workflow/<task>/master.md` (+ `investigations/`, `expansions/`) — the task's master node.
- `.claude/knowledge/` — seeded reference for facts you already know (build/test, debug flows).
- `.claude/rules/cybernetics-workflow.md` — the workflow conventions task-agents obey.

The conventions file is **path-scoped to `workflow/**`**: it loads only when an agent touches
`workflow/` (or `.claude/knowledge/`) files, so it does not interfere with normal work in the
project. To make it always-on, delete the `paths:` front-matter at the top of that file.

## 4. Try the research flow

1. `/init-task` — describe your task; confirm the provisional master.
2. Dispatch `investigate` to open a new line of inquiry, or `expand` to deepen a thin bullet.
   Each writes a node and a one-line takeaway back into the master.
3. Ask `overseer` for a status report (what's unblocked, distance to DoD).

## Updating / removing

- Update: `/plugin marketplace update cybernetics`, then reinstall if needed.
- Disable/remove: `/plugin` → manage `cybernetics`. The project's `workflow/`, `.claude/knowledge/`,
  and `.claude/rules/cybernetics-workflow.md` are plain files you own and can keep or delete.

---
name: create-work-unit
description: Insert a new Work Unit entry into a task's master.md `## Work Units` section. ONLY for when the user EXPLICITLY asks to create/add/record a Work Unit (e.g. "create a work unit", "add a WU for this", "log this as a work unit"). This is a manual, user-architected action — do NOT invoke on your own initiative, do NOT trigger from research, exposition, or because a candidate WU came up in conversation, and do NOT invoke if the user only mused about a possible WU. Always confirms with the user before writing.
---

# Create a Work Unit

Work Units are **architected by the user**. This skill only transcribes a WU the user has
already decided on into the master file's structure. It does **not** design the WU, and it does
**not** decide that a WU should exist. The job is mechanical: build the boilerplate entry from
exactly what the user supplied, show it, and insert it only after the user approves.

## Safeguard 1 — run only on an explicit request

Run ONLY when the user has explicitly asked to create / add / record a Work Unit. Do **not** run
because:

- research or an investigation surfaced a "potential Work Unit" — that is a suggestion, not a request;
- you judged that a WU would be a good idea;
- the user described some work without asking to record it as a WU.

If you are not certain the user is asking to create a WU **right now**, ask them first — do not
assume. A candidate WU mentioned in passing is not authorization to create one.

## Safeguard 2 — confirm before writing

Never write to the file until the user has approved the exact entry. Show the rendered line and
where it will go, and wait for an explicit yes. If they want changes, revise and re-show.

## Safeguard 3 — do not embellish

Populate the entry with ONLY what the user gave you. Do not invent scope, acceptance criteria,
file lists, or a description the user did not state. If the user gave only a title, the entry is
just a title. A thin WU is correct, not a gap to fill — detail is the user's to architect. Never
pull detail from research/exposition context the user did not ask to include.

## Procedure

1. **Locate the master file.** Find the relevant `workflow/<task>/master.md`. If more than one
   task exists and it is ambiguous, ask which one. Read its `## Work Units` section.
2. **Pick the next number.** WUs are numbered `W0, W1, W2 …` sequentially. Use the next unused
   number after the highest existing entry.
3. **Build the entry** in the section's format, from what the user supplied only:

   ```
   - [ ] **W<n> — <title>.** <one-line scope, only if the user gave one.> _open_
   ```

   Status starts `open` with an unchecked box. Omit the scope clause entirely if the user did
   not state one.
4. **Confirm** (Safeguard 2). Show the exact line and its insertion point; wait for explicit
   approval.
5. **Insert** the approved entry into the `## Work Units` section — after the last real WU and
   above any placeholder bullet (e.g. `_(next WUs authored as understanding accrues)_`). Change
   nothing else: do not touch the DoD, Background, Investigations index, or any other section.
6. **Stop.** Report the WU id and that it is inserted as `open`. Do not start implementing it, do
   not suggest the next WU, do not add detail.

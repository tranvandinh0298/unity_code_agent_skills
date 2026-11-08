---
name: task-create
description: Create or refine a structured Unity game-development task from a user's rough feature description. Use before task-ba, task-analysis, task-plan, and task-implement. Produce requirements and acceptance criteria only; do not code, change Unity assets, or implement the feature.
---

# Unity Task Create

## Purpose

Turn a rough game idea, bug report, improvement request, or gameplay description into a precise, reviewable task specification. Preserve the user's intent and create a stable input for `$task-ba`, `$task-analysis`, `$task-plan`, `$task-implement`, and `$task-review`.

The task specification defines **what** should happen and **how success is observed**, not how to program it.

## Boundaries

- This skill may read project documentation, folder names, manifest files, and existing task files to discover terminology and avoid duplicates. Keep inspection lightweight. Do not conduct code-level root-cause analysis; defer that to `$task-analysis`.
- Do not edit C# code, `.unity` scenes, `.prefab` assets, `.asset` files, Unity settings, packages, or generated Unity metadata.
- Only create or update task documentation under `docs/tasks/`, unless the user explicitly specifies another documentation directory.
- Do not start another skill or implement the task automatically.
- Do not assert that a Scene, Prefab, animation, package, or API exists without verifying it.
- Do not silently turn an example, tentative suggestion, or AI invention into a confirmed requirement.
- Do not introduce new gameplay mechanics, platforms, plugins, architectural patterns, or visual assets unless requested. If useful, record them as optional suggestions.
- Respect existing project conventions and `AGENTS.md` when available.
- Never delete or overwrite existing task documents without explicit user authorization. Never use destructive Git operations.

## Input

Accept free-form instructions, for example:

`$task-create Add a double-jump ability when the player collects a feather.`

Additional user material can include screenshots, reference links, video descriptions, existing Unity asset names, gameplay specifications, or bug reproduction steps.

Do not require the user to fill out a form.

## Process

### 1. Capture original intent

- Record the user's description in its original language, preserving meaningful details.
- Identify the request type: `feature`, `bug`, `improvement`, `refactor`, `content`, `technical`, or `research`.
- Extract explicit goals, requested behavior, constraints, exclusions, affected systems, and references.
- Keep user-stated behavior separate from inferred behavior.

### 2. Check project context without implementing

When the repository is accessible, inspect only what is needed to identify the project context:

- `AGENTS.md`, relevant README/design docs, existing tasks, and obvious directory conventions.
- `Packages/manifest.json` for relevant installed Unity packages, when necessary.
- `ProjectSettings/ProjectVersion.txt` for the Unity version, when necessary.
- Existing paths/names for Scenes, Prefabs, scripts, and assets explicitly mentioned by the user, when easy to find.

If the repository is unavailable, do not invent project details. Use `Not verified` wherever relevant.

### 3. Resolve ambiguity conservatively

Classify each point as:

- **Confirmed**: explicitly stated by the user or reliably established by project documentation.
- **Proposed**: reasonable default that still requires user approval; it is not a requirement.
- **Open question**: missing information that could change gameplay or implementation scope.

If a missing detail materially changes expected gameplay or task scope, add it to Open Questions; do not guess. Ask at most three high-impact clarification questions when essential, but still create a useful draft from available information.

If the detail is not essential, leave it unspecified and let `$task-ba` resolve it later.

### 4. Set the task boundary

- State the player-facing or developer-facing outcome.
- Define in-scope and out-of-scope behavior.
- Identify relevant Unity domains only if applicable: Scenes, GameObjects, Prefabs, MonoBehaviours, input, 2D/3D physics, animation, UI, audio, camera, spawning, saving/loading, performance, editor tooling, build targets.
- Avoid prescribing specific components, file names, design patterns, APIs, or algorithms unless the user explicitly demands them. Record existing named assets as context, not implementation mandates.
- Keep task size focused. If several independently testable features are present, propose a parent task with separate child tasks. Do not split a tightly coupled, small feature just to produce more tasks.
- For bugs, capture reproducible steps, expected and observed behavior, frequency, and environment when supplied; never fabricate reproduction evidence.

### 5. Write verifiable acceptance criteria

Use numbered criteria `AC-01`, `AC-02`, etc. Each must describe an observable outcome, not implementation steps.

Good: `AC-01: When the player collides with an active feather, the feather disappears and the player can perform one additional jump while airborne.`

Bad: `AC-01: Create a FeatherPickup.cs script with OnTriggerEnter2D.`

Cover only applicable cases:

- Core gameplay and player input.
- State transitions, limits, cooldowns, and resets.
- Relevant edge cases: repeated collisions, pause/resume, scene transitions, death/restart, duplicate effects.
- Player feedback, UI, animation, audio only when requested.
- Backward compatibility with existing gameplay.
- Testability in Unity Play Mode or a target build.

Never add arbitrary numerical thresholds (speed, duration, score, memory usage, frame rate) that the user did not provide. Record them as open questions or proposed values.

### 6. Name and store the task

Default output path:

`docs/tasks/<task-id>-<short-kebab-case-title>/TASK.md`

Task ID format: `TASK-YYYYMMDD-NNN`, where the date is the current local date and `NNN` is the next free three-digit daily sequence after checking the existing `docs/tasks/` directory. If local date is unavailable, use `TASK-UNSCHEDULED-NNN` and mark the date for later confirmation; never pretend to know a date.

Use a short and descriptive title (for example `player-double-jump`). Do not reuse an existing ID or folder. For an existing task, update only if the user clearly requests a revision, and preserve confirmed requirements unless the user changes them.

Do not generate child task files unless the user requests the split. For a large task, record the recommended child tasks in the parent document.

### 7. Output document schema

Create `TASK.md` with these headings in exactly this order:

```markdown
# TASK-YYYYMMDD-NNN — <Title>

## Metadata

- **Type:** <feature | bug | improvement | refactor | content | technical | research>
- **Status:** Draft
- **Created:** <YYYY-MM-DD or Not verified>
- **Source:** User request
- **Next step:** `$task-ba`

## Original Request

> <Preserve the meaningful original user wording.>

## Objective

<One short paragraph explaining the intended outcome.>

## Confirmed Requirements

- **REQ-01:** <User-confirmed behavior or constraint>

## Scope

### In Scope

- ...

### Out of Scope

- ...

## Gameplay / System Flow

<Numbered player-visible or system-visible interactions; use Not specified for missing flows.>

## Acceptance Criteria

- **AC-01:** <Observable, verifiable success condition>

## Edge Cases and Failure Behavior

- <Only known cases; otherwise identify as unresolved.>

## Unity Context

- **Unity version:** <Verified version or Not verified>
- **Relevant scenes/assets:** <Verified names or Not verified>
- **Affected systems:** <High-level areas; not an implementation plan>
- **Target platform/input:** <Confirmed or Not specified>

## Constraints and Non-Goals

- <User constraints and explicitly excluded work>

## Assumptions / Proposals (Not Approved)

- **P-01:** <Optional proposal and rationale, or None>

## Open Questions

- **Q-01 [Blocking | Non-blocking]:** <Question and what decision it affects, or None>

## Suggested Task Breakdown

- <Only if genuinely needed; otherwise Single focused task>

## References

- <User-supplied assets, URLs, documents, or None>

## Handoff to task-ba

- **Confirmed decisions:** <What task-ba must preserve>
- **Questions to resolve:** <Q identifiers>
- **Scope boundaries:** <What must not be added without permission>
```

### 8. Validate before finishing

Check:

1. The task reflects the original request without unrelated mechanics.
2. Every confirmed requirement is backed by user input or verified project context.
3. Acceptance criteria are testable without dictating code architecture.
4. Unknowns are labeled, not silently decided.
5. The task is small enough to implement and review, or a breakdown is proposed.
6. No project/runtime file was changed.
7. The target `TASK.md` was created successfully and the path is reported.

If writing the file is impossible, return the full document in the answer and clearly state it was not saved.

## Final response

Keep the response concise:

- Created/updated task ID and file path.
- One-paragraph task summary.
- Counts of requirements, acceptance criteria, and open questions.
- Up to three important unresolved questions if any.
- State that no code or Unity assets were modified.
- Suggest `$task-ba` with the created task path. Do not launch it automatically.

## Language

Write the task in the user's input language by default. Preserve Unity and programming terms in English where translations would obscure their meaning. Do not translate user quotations or file paths.

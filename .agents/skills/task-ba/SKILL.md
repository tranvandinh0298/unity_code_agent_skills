---
name: task-ba
description: >
    Analyze a Unity game development task and produce a detailed,
    testable Game Design / BA specification. Use after task-create
    and before task-analysis. Clarifies gameplay behavior, player
    interactions, rules, game states, edge cases, and acceptance
    criteria without implementing code or selecting architecture.
---

# Unity Task BA — Game Design & Requirements Analysis

## 1. Role

You are a Game Design Analyst and Business Analyst specializing
in Unity game development.

Your responsibility is to transform a task specification into
a clear, consistent, testable Game Design / BA document.

You MUST preserve the user's intended gameplay experience.

You are NOT responsible for technical implementation.

Primary objectives:

- Eliminate ambiguous gameplay requirements.
- Identify missing mechanics and interactions.
- Define expected player-facing behavior.
- Identify game states and transitions.
- Define testable acceptance criteria.
- Detect conflicts with existing gameplay rules.
- Record assumptions and unresolved decisions.
- Prepare requirements for task-analysis.

## 2. Invocation

Command:

    $task-ba <task-id or task-directory>

Examples:

    $task-ba TASK-20261108-001

    $task-ba docs/tasks/TASK-20261108-001-enemy-spawning

If no task identifier is provided:

1. Search docs/tasks for existing tasks.
2. If exactly one appropriate active task exists, use it.
3. If multiple plausible tasks exist, ask the user to select one.
4. Never guess which task should be modified.

## 3. Required Inputs

Primary input:

    docs/tasks/<task-directory>/TASK.md

Optional inputs:

- Existing BA specifications.
- Game Design Document (GDD).
- Gameplay documentation.
- Existing Unity project structure.
- Existing Scenes and Prefabs.
- Existing gameplay scripts.
- Relevant user-provided references.

TASK.md remains the source of user intent.

Existing project behavior is supporting evidence,
not automatically a requirement.

If the task document is missing, do not invent it.
Ask the user to run task-create first or provide a valid path.

## 4. Output

Create or update:

    docs/tasks/<task-directory>/BA.md

Do not overwrite TASK.md.

BA.md is the source of approved functional requirements
for subsequent workflow stages.

Do not create implementation files or Unity assets.

## 5. Scope of Analysis

Analyze only the selected task and directly related
gameplay dependencies.

Do not redesign unrelated mechanics.

Do not invent additional features to make the game
more sophisticated.

Distinguish:

- User-requested behavior.
- Existing documented behavior.
- Existing observed behavior.
- Analyst assumptions.
- Optional improvement suggestions.

Never silently promote assumptions or suggestions
into confirmed requirements.

## 6. Workflow

### Step 1 — Read the task

Read TASK.md completely.

Extract:

- Task objective.
- Player-facing goal.
- Requested gameplay mechanics.
- Functional requirements.
- Constraints.
- Acceptance criteria.
- Known edge cases.
- Open questions.
- Explicit exclusions.

Preserve the original intent.

If requirements conflict, record the conflict.

Do not resolve conflicting requirements silently.

### Step 2 — Inspect relevant project context

When project files are available, inspect only what
is necessary to understand existing gameplay behavior.

Possible areas:

    Assets/Scripts/
    Assets/Scenes/
    Assets/Prefabs/
    Assets/Resources/
    Assets/Settings/
    Packages/manifest.json
    ProjectSettings/

Identify:

- Existing player actions.
- Existing game states.
- Related gameplay systems.
- Existing rules.
- Shared mechanics.
- Relevant task dependencies.

Treat observed behavior as evidence, not design authority.

Do not use code structure to dictate gameplay design.

Do not modify any inspected files.

Do not assume a Unity feature or package is installed
without evidence.

### Step 3 — Classify requirements

Classify every major requirement as:

CONFIRMED:
Explicitly requested by the user or approved in
existing authoritative documentation.

INFERRED:
Strongly implied by confirmed requirements but
not explicitly approved.

PROPOSED:
A design suggestion introduced by the analyst.

OPEN:
Requires user confirmation or further investigation.

CONFLICT:
Contradicts another requirement or existing
authoritative design rule.

Each requirement must have a unique ID.

Example:

    FR-001 — Enemy spawning starts when gameplay begins.
    Status: CONFIRMED
    Source: TASK.md

    FR-002 — Enemy spawning pauses when the game is paused.
    Status: OPEN
    Source: Analyst question

Do not describe an OPEN or PROPOSED requirement as approved.

### Step 4 — Analyze gameplay mechanics

For each mechanic, identify:

1. Trigger
   What causes the mechanic to activate?

2. Preconditions
   What must be true before activation?

3. Behavior
   What should happen during execution?

4. Player interaction
   Can the player influence the mechanic?

5. Result
   What is the expected outcome?

6. Failure conditions
   When can the mechanic fail or be interrupted?

7. Recovery
   What happens after failure?

8. Dependencies
   Which existing gameplay rules affect it?

9. Feedback
   What should the player see, hear, or feel?

10. Completion
    When is the mechanic considered finished?

Do not require irrelevant categories for trivial tasks.
Use N/A where appropriate.

### Step 5 — Analyze game states

Identify relevant gameplay states.

Examples:

- Main Menu
- Ready
- Playing
- Paused
- Game Over
- Victory
- Loading

Do not assume every game uses these states.

Define:

- Valid state transitions.
- Transition triggers.
- Allowed actions per state.
- Restricted actions per state.
- Expected behavior during transitions.

Example:

    Playing -> Game Over

    Trigger:
    Player health reaches zero.

    Expected:
    - Player control stops.
    - Enemy spawning stops.
    - Game Over feedback appears.

Record unclear timing or ordering as OPEN.

### Step 6 — Analyze gameplay edge cases

Consider relevant edge cases in these categories.

A. Input

- Repeated input.
- Simultaneous actions.
- Input during pause.
- Input during transitions.
- Unsupported input devices.

B. Gameplay

- Player death during an active mechanic.
- Mechanic activation before initialization.
- Multiple triggers at once.
- Missing target or player reference.
- Objects leaving the playable area.

C. Timing

- Frame-rate differences.
- Interrupted animations.
- Delayed events.
- Rapid state transitions.
- Time-scale changes.

D. Scene lifecycle

- Scene reload.
- Scene transition.
- Restarting gameplay.
- Persistent objects.
- Destroyed or disabled objects.

E. Progression

- Minimum or maximum values.
- Difficulty boundaries.
- Score or reward duplication.
- Saving and restoring state, if applicable.

F. Presentation

- Missing visual or audio feedback.
- UI state inconsistency.
- Feedback during pause or game over.

Only document edge cases relevant to the task.

Do not turn hypothetical technical failures into
mandatory gameplay features.

### Step 7 — Analyze Unity-specific constraints

Identify functional constraints that may affect
later technical analysis.

Examples:

- 2D or 3D gameplay.
- Target platform.
- Input methods.
- Physics-dependent interactions.
- Animation-dependent behavior.
- Scene transitions.
- Prefab-based content variations.
- Runtime object lifecycle.
- Required gameplay responsiveness.

Do NOT decide:

- Which C# classes to create.
- Which MonoBehaviour owns a mechanic.
- Whether to use ScriptableObject.
- Whether to use object pooling.
- Whether to use coroutines or Update().
- Which design pattern to implement.
- Which Prefabs to create.
- How GameObjects are organized.

These belong to task-analysis and task-plan.

Record constraints without prescribing implementation.

### Step 8 — Resolve ambiguities

Identify questions that materially affect:

- Gameplay outcome.
- Player experience.
- Acceptance criteria.
- Feature scope.
- Game state transitions.
- Compatibility with existing rules.

Classify questions by priority:

BLOCKING:
Different answers would lead to materially
different gameplay requirements.

NON-BLOCKING:
Can remain unresolved temporarily without
preventing useful BA analysis.

For BLOCKING questions:

1. Explain the ambiguity.
2. Provide concise alternatives where possible.
3. Recommend one option when justified.
4. Ask the user for a decision.
5. Do not silently choose a design.

Batch related BLOCKING questions into one
concise interaction when practical.

Avoid asking questions that existing documents
or code inspection can answer.

When information remains unavailable:

- Record it as OPEN.
- State the impact.
- Mark affected requirements as provisional.
- Continue analyzing independent requirements.

Never fabricate user approval.

### Step 9 — Define acceptance criteria

Each confirmed functional requirement should
have observable acceptance criteria.

Use Given / When / Then where useful.

Example:

    AC-001
    Given gameplay is active,
    When the enemy spawn condition is satisfied,
    Then an enemy appears according to the
    confirmed spawning rules.

    AC-002
    Given the player is dead,
    When the spawning condition occurs,
    Then no new enemies are spawned.

Acceptance criteria MUST:

- Be testable.
- Describe expected behavior.
- Avoid prescribing implementation.
- Cover relevant negative scenarios.
- Reference requirement IDs.

If a required expected result is unknown,
mark the criterion BLOCKED.

Do not invent numeric values.

For unspecified values such as:

- Spawn interval.
- Movement speed.
- Damage amount.
- Score reward.
- Difficulty scaling.

Use:

    TBD — requires user confirmation

If provisional values are useful, label them
PROPOSED and do not treat them as approved.

### Step 10 — Check scope and task size

Evaluate whether the task can reasonably be
implemented and tested as one unit.

Potential signs of excessive scope:

- Multiple independent gameplay systems.
- Separate player-facing features.
- Many unrelated Scenes.
- Large progression changes.
- Dependencies requiring separate implementation.
- Multiple features with independent acceptance tests.

If decomposition is recommended:

1. Preserve the original task.
2. Suggest smaller logical subtasks.
3. Identify dependencies.
4. Explain why splitting is useful.
5. Do not automatically create subtasks.
6. Do not change the parent scope without approval.

Each suggested subtask must have a clear,
independently testable outcome.

Avoid splitting tightly coupled mechanics
into artificially tiny tasks.

### Step 11 — Generate BA.md

Use the template in Section 8.

Preserve existing confirmed decisions when updating.

Do not erase previously approved requirements
without explicit user instruction.

If a new request conflicts with an approved rule,
record the conflict and request clarification.

### Step 12 — Validate the document

Before completion, verify:

- Every confirmed requirement has a source.
- Every requirement has a stable ID.
- User intent has been preserved.
- Proposed mechanics are clearly labeled.
- Relevant game states are documented.
- Edge cases are addressed.
- Acceptance criteria are testable.
- No unsupported numeric values were invented.
- No implementation architecture was prescribed.
- Blocking decisions are visible.
- No unresolved requirement was called approved.
- No out-of-scope files were modified.

### Step 13 — Report results

Report:

- Task identifier.
- BA document path.
- Number of confirmed requirements.
- Number of open decisions.
- Critical ambiguities.
- Suggested task decomposition, if any.
- Readiness for task-analysis.

Readiness is determined as follows:

READY:
No blocking functional questions remain.
All core gameplay behavior is sufficiently defined.

CONDITIONAL:
Independent portions are analyzable, but some
requirements remain unresolved.

BLOCKED:
Critical gameplay behavior cannot be specified
without user decisions.

Only READY tasks may proceed through the normal
workflow without explicit qualification.

CONDITIONAL tasks may receive technical analysis
limited to confirmed requirements, but blocked
features must not be planned or implemented
as if they were approved.

## 7. Language Policy

Use the language of the user's task description
for business and gameplay explanations.

Preserve established technical terminology in English.

Examples:

- GameObject
- MonoBehaviour
- Prefab
- Scene
- Rigidbody2D
- Collider2D
- ScriptableObject
- Input System
- Game State
- Spawn
- Animation
- UI

For Vietnamese tasks:

- Write functional explanations in Vietnamese.
- Keep Unity API names in English.
- Use English headings or bilingual headings.
- Avoid unnatural literal translations.

Requirement identifiers and status values
must always use English.

## 8. BA Document Template

Generate BA.md using this structure:

# Game Design / BA Specification

## 1. Document Information

- Task ID:
- Task Title:
- Parent Task:
- Source:
- Status: DRAFT | CONDITIONAL | READY | BLOCKED
- Revision:
- Last Updated:

## 2. Objective

Describe the desired player-facing result.

## 3. Gameplay Context

Describe the relevant existing gameplay
and its relationship to this task.

Indicate which statements are documented,
observed, or assumptions.

## 4. Scope

### 4.1 In Scope

Features and behavior included in this task.

### 4.2 Out of Scope

Features explicitly excluded or deferred.

### 4.3 Dependencies

Existing mechanics or decisions that this
task depends upon.

## 5. Functional Requirements

For each requirement:

### FR-001 — Requirement Title

- Status:
- Source:
- Description:
- Trigger:
- Preconditions:
- Expected Behavior:
- Result:
- Dependencies:
- Notes:

Repeat as necessary.

## 6. Gameplay Flow

Describe the sequence of player actions
and system responses.

Include alternative flows where applicable.

## 7. Game States and Transitions

For each relevant transition:

- Current State:
- Trigger:
- Next State:
- Expected Behavior:
- Restrictions:

Use N/A when the task does not involve
game state transitions.

## 8. Player Feedback

Describe required:

- Visual feedback.
- Audio feedback.
- Animation feedback.
- UI feedback.
- Other player-facing feedback.

Mark unspecified design choices as OPEN.

## 9. Gameplay Rules and Constraints

Record relevant:

- Timing rules.
- Spawn rules.
- Movement boundaries.
- Difficulty behavior.
- Scoring rules.
- Platform constraints.
- Input restrictions.

Only include applicable categories.

## 10. Edge Cases

For each edge case:

### EC-001 — Edge Case Title

- Related Requirement:
- Scenario:
- Expected Result:
- Status:

## 11. Acceptance Criteria

For each criterion:

### AC-001

- Related Requirement:
- Given:
- When:
- Then:
- Status: READY | BLOCKED

Include negative scenarios where relevant.

## 12. Open Questions

For each question:

### Q-001 — Question Title

- Priority: BLOCKING | NON-BLOCKING
- Related Requirements:
- Context:
- Question:
- Options:
- Recommendation:
- User Decision: PENDING
- Impact if Unresolved:

Keep all unresolved decisions visible.

## 13. Assumptions and Suggestions

### Assumptions

Record inferred behavior separately
from approved requirements.

### Optional Suggestions

Record possible improvements without
changing the approved scope.

## 14. Suggested Task Decomposition

Only include when needed.

For each proposed subtask:

- Suggested ID:
- Title:
- Scope:
- Dependencies:
- Testable Outcome:
- Approval: PENDING

Do not automatically create subtasks.

## 15. Requirement Traceability

Map each confirmed functional requirement
to its acceptance criteria.

Example:

| Requirement | Acceptance Criteria |
| ----------- | ------------------- |
| FR-001      | AC-001, AC-002      |
| FR-002      | AC-003              |

## 16. Handoff to task-analysis

Summarize:

- Confirmed gameplay requirements.
- Relevant gameplay dependencies.
- Unity functional constraints.
- Unresolved questions.
- Excluded behavior.
- Areas requiring technical inspection.

Handoff Status:
READY | CONDITIONAL | BLOCKED

Technical analysis must not reinterpret
OPEN requirements as confirmed decisions.

## 9. Update and Revision Rules

When BA.md already exists:

1. Read the entire existing document.
2. Preserve requirement IDs wherever possible.
3. Preserve approved decisions.
4. Add new requirements with new IDs.
5. Mark obsolete requirements as SUPERSEDED.
6. Explain material changes in a revision log.
7. Re-evaluate affected acceptance criteria.
8. Re-evaluate handoff readiness.

Do not renumber existing IDs unnecessarily.

A revised BA specification must not silently
invalidate previously completed work.

If a changed requirement affects implementation,
flag it for later impact analysis.

## 10. Safety and Modification Boundaries

Allowed:

- Read existing project files.
- Read TASK.md.
- Create or update BA.md.
- Report findings and questions.

Not allowed:

- Modify gameplay scripts.
- Modify Scenes.
- Modify Prefabs.
- Modify assets or ProjectSettings.
- Install packages.
- Execute Unity Editor modifications.
- Update TASK.md without explicit instruction.
- Create implementation plans.
- Generate implementation code.
- Run unrelated destructive commands.

Only write inside the selected task's BA.md
unless the user explicitly authorizes otherwise.

## 11. Completion Rule

A successful task-ba execution produces a
coherent, traceable specification of intended
gameplay behavior.

It does not need to resolve every minor design
detail, but must never conceal unresolved
critical decisions.

The goal is not to design the most sophisticated
game possible.

The goal is to specify precisely the game
the user intends to build.

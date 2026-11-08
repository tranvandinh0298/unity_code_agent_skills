# Unity Project — AI Agent Instructions

## 1. Purpose

This repository contains a Unity game project managed by
a solo developer with AI-assisted development workflows.

AI agents must prioritize:

- Correct gameplay behavior.
- Preservation of user intent.
- Minimal, maintainable implementation.
- Compatibility with the existing Unity project.
- Safe incremental modifications.
- Verifiable results.
- Protection of existing user work.

Do not introduce unnecessary complexity.

## 2. Project Context

This is a Unity project using C#.

Determine the actual Unity version, packages, project
structure, target platform, and architecture by inspecting
the repository.

Do not assume:

- The game is 2D or 3D.
- A specific render pipeline is installed.
- The new Input System is enabled.
- A particular Scene or Prefab exists.
- Any specific design pattern is in use.
- Unity Editor automation is available.

Prefer existing project conventions.

## 3. Agent Workflow

The repository defines these skills:

| Skill          | Responsibility               | Output            |
| -------------- | ---------------------------- | ----------------- |
| task-create    | Structure user intent        | TASK.md           |
| task-ba        | Define gameplay requirements | BA.md             |
| task-analysis  | Analyze existing systems     | ANALYSIS.md       |
| task-plan      | Design implementation        | PLAN.md           |
| task-implement | Execute approved changes     | IMPLEMENTATION.md |
| task-review    | Independently review         | REVIEW.md         |
| task-rollback  | Recover task-owned changes   | ROLLBACK.md       |

Skills are stored in:

    .agents/skills/<skill-name>/SKILL.md

When a skill is invoked, follow its SKILL.md together
with these shared instructions.

A skill's permissions apply only to its own stage.

Do not automatically run the next skill.

Do not automatically create or execute tasks merely
because a user asks a general technical question.

The normal workflow is:

    create -> ba -> analysis -> plan
           -> implement -> review

Review findings may return to implementation.

Rollback is an independent recovery operation.

## 4. Task Organization

Task documents are stored under:

    docs/tasks/<task-directory>/

Recommended directory convention:

    TASK-YYYYMMDD-NNN-short-description

Use a unique task identifier.

Do not rely on a directory name as proof of task
identity; inspect TASK.md.

Keep all documents for a task in the same directory.

Do not overwrite another task's documents.

## 5. Document Ownership

Each skill owns its primary output:

- task-create: TASK.md
- task-ba: BA.md
- task-analysis: ANALYSIS.md
- task-plan: PLAN.md
- task-implement: IMPLEMENTATION.md
- task-review: REVIEW.md
- task-rollback: ROLLBACK.md

A skill must not modify documents owned by another
stage without explicit user authorization.

task-implement may also modify approved project files.

task-rollback may reverse approved task-owned changes
only after the required recovery checks and approval.

All other stages are read-only with respect to
Unity project implementation files.

## 6. Requirements and Traceability

Use stable identifiers:

    FR-001   Functional Requirement
    AC-001   Acceptance Criterion
    EC-001   Edge Case
    Q-001    Open Question
    SYS-001  Existing System
    TF-001   Technical Finding
    DD-001   Design Decision
    STEP-001 Implementation Step
    TEST-001 Test Case
    RV-001   Review Finding
    RB-001   Rollback Operation

Identifiers are unique within a task and category.

Preserve identifiers across revisions.

Do not silently reuse IDs for unrelated content.

Maintain traceability:

    Requirement
        -> Acceptance Criterion
        -> Implementation Step
        -> Test
        -> Review Evidence

Not every technical finding requires a separate test.

## 7. Requirements Authority

Use this general authority order:

1. Current explicit user instructions.
2. Approved functional decisions in BA.md.
3. Original intent in TASK.md.
4. Verified current project evidence.
5. Approved technical design in PLAN.md.
6. Technical analysis and recommendations.

A new user instruction that changes previously
approved behavior requires impact assessment.

Do not silently rewrite upstream requirements.

When documents conflict:

- Identify the conflict.
- Preserve existing user work.
- Stop affected operations.
- Report which upstream document needs revision.
- Continue unrelated safe work where possible.

Actual project evidence takes precedence over
outdated technical descriptions when determining
what currently exists.

It does not automatically override approved gameplay.

## 8. Requirement Status

Functional requirement statuses:

    CONFIRMED
    INFERRED
    PROPOSED
    OPEN
    CONFLICT
    SUPERSEDED

Only CONFIRMED requirements are approved for normal
implementation.

Do not silently promote INFERRED, PROPOSED,
or OPEN requirements to CONFIRMED.

An explicit user decision can resolve a question.

Record the decision and update relevant requirements.

## 9. Workflow Readiness

Document readiness statuses:

    READY
    CONDITIONAL
    BLOCKED

READY:
Required information is sufficiently complete
for the next stage.

CONDITIONAL:
Some independent work can proceed, while other
work is gated by unresolved decisions.

BLOCKED:
Critical missing information prevents the stage
from progressing safely.

Do not equate document existence with readiness.

Only execute implementation steps whose prerequisites
and execution gates are satisfied.

## 10. Change Control

Before changing implementation files:

1. Read applicable task documents.
2. Inspect current project files.
3. Identify exact intended changes.
4. Inspect existing user modifications.
5. Establish recoverable pre-change evidence.
6. Verify required dependencies.
7. Make only authorized changes.
8. Validate the result.
9. Record actual changes.

Do not overwrite unrelated work.

Do not make unrelated refactors.

Do not add optional gameplay features without approval.

If the implementation plan is materially invalid,
stop affected work and request replanning.

## 11. Unity Development Rules

Use C# conventions consistent with the project.

Consider relevant Unity concerns:

- MonoBehaviour lifecycle.
- GameObject and Component ownership.
- Scene transitions.
- Prefab and serialized references.
- Initialization order.
- Event subscription and cleanup.
- Physics update timing.
- Input handling.
- Object lifetime.
- Performance and allocations.

Do not introduce architecture patterns unnecessarily.

Prefer simple components with clear responsibilities.

Do not automatically introduce:

- Singleton managers.
- Generic event buses.
- Dependency injection frameworks.
- Object pooling.
- ScriptableObject-based architecture.

Use them only when justified by actual requirements
and project conditions.

## 12. Unity Asset Safety

Unity assets may contain serialized references
and GUID dependencies.

Never casually delete or regenerate .meta files.

Never assume a C# script is correctly attached
to a GameObject.

Never assume Inspector fields are assigned
because serialized fields exist in code.

Prefer reliable Unity Editor tooling for complex
Scene and Prefab modifications.

Avoid fragile broad YAML replacements.

When Editor access is unavailable, document exact
manual configuration and verification actions.

## 13. Testing and Evidence

Distinguish:

    STATIC
    COMPILE
    EDIT_MODE
    PLAY_MODE
    INSPECTOR
    MANUAL
    PROFILER

Test results:

    PASS
    FAIL
    NOT_RUN
    BLOCKED

Do not claim tests passed unless executed.

Compilation does not prove runtime gameplay behavior.

Static inspection does not prove Scene configuration.

Do not fabricate performance measurements.

Use test methods appropriate to the task.

## 14. Git and Recovery

Use Git when available.

Before implementation, inspect repository state.

Preserve pre-existing user modifications.

Never automatically run destructive broad commands:

    git reset --hard
    git clean -fd
    git checkout -- .
    git restore .

Do not automatically commit or push changes.

For implementation steps, capture recoverable
pre-change snapshots as required by task-implement.

Recovery snapshots should use:

    .agent-recovery/<task-id>/

Do not place recovery snapshots under Assets/.

task-rollback must preview changes and obtain
explicit approval before removing existing work.

A recovery snapshot is not permission to overwrite
later unrelated changes.

## 15. Review and Correction

task-review is independent and read-only.

It reports findings without automatically modifying
implementation files.

Corrections follow:

    task-review
         |
         v
    task-implement
         |
         v
    task-review recheck

For a targeted review correction, task-implement
must read REVIEW.md and inspect the related
requirements and plan.

If correction requires a gameplay change,
return to task-ba.

If correction requires architectural changes,
return to task-plan.

Do not treat a review recommendation as permission
to expand the approved scope.

## 16. Communication

Use the language of the user's task description.

Keep Unity API names, symbols, file paths,
status values, and identifiers in English.

Be concise but explicit.

Report:

- What was done.
- What was not done.
- What was verified.
- What remains uncertain.
- What the user needs to do next.

Distinguish facts, assumptions, and recommendations.

Do not claim completion without evidence.

Do not conceal failures.

## 17. Task Completion

A task is not automatically accepted because
task-implement has finished writing code.

Acceptance requires:

- Approved functionality is implemented.
- Mandatory verification is completed.
- Required Unity configuration is validated.
- Blocking review findings are resolved.
- Remaining limitations are reported.

task-review may recommend acceptance.

Final acceptance belongs to the user.

## 18. General Safety

Do not:

- Delete unrelated assets.
- Overwrite unrelated changes.
- Change the Unity version without approval.
- Install unrelated packages.
- Modify external services without authorization.
- Run destructive cleanup commands.
- Silently change gameplay requirements.
- Claim unavailable tools were used.

Prefer safe partial progress over risky changes.

When blocked, clearly identify the reason
and the required next action.

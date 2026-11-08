# Unity AI-Assisted Development Workflow

A task-driven AI development workflow for Unity projects
using Codex, C#, and structured Markdown documentation.

Designed for solo game developers who want AI assistance
without losing control of gameplay design, implementation,
testing, or project changes.

## 1. Overview

The workflow consists of seven agent skills:

1. task-create
2. task-ba
3. task-analysis
4. task-plan
5. task-implement
6. task-review
7. task-rollback

Each skill has a specific responsibility and produces
a traceable document.

The workflow separates:

- User intent.
- Gameplay requirements.
- Technical analysis.
- Implementation planning.
- Code changes.
- Quality review.
- Recovery.

This reduces the risk of an agent implementing a
technically valid feature that does not match
the intended gameplay.

## 2. Requirements

Recommended development environment:

- Unity Editor compatible with the project.
- Visual Studio Community or another C# IDE.
- Codex CLI or a supported Codex IDE integration.
- Git for version control.
- A C# development environment compatible with Unity.

Optional:

- Unity Test Framework.
- Unity Profiler.
- Unity Editor automation tools.
- Additional project-specific testing tools.

Unity Editor is still needed to verify many
Scene, Prefab, Inspector, and gameplay interactions.

Codex does not automatically provide complete
Unity Editor automation.

## 3. Installation

Copy the following structure into the Unity
project root:

    AGENTS.md
    README.md

    .agents/skills/
        task-create/SKILL.md
        task-ba/SKILL.md
        task-analysis/SKILL.md
        task-plan/SKILL.md
        task-implement/SKILL.md
        task-review/SKILL.md
        task-rollback/SKILL.md

Launch Codex from the project root.

For Codex CLI:

    cd <unity-project-root>
    codex

In a supported IDE, open the project workspace
and start the Codex integration.

Use the skills selector or type a skill name
with the $ prefix.

Example:

    $task-create

If a recently added skill is not detected,
restart the Codex session.

## 4. Workflow

Normal development:

    User Description
          |
          v
      task-create
          |
          v
        TASK.md
          |
          v
        task-ba
          |
          v
         BA.md
          |
          v
     task-analysis
          |
          v
      ANALYSIS.md
          |
          v
       task-plan
          |
          v
        PLAN.md
          |
          v
     task-implement
          |
          v
    IMPLEMENTATION.md
          |
          v
      task-review
          |
          v
       REVIEW.md

If review detects defects:

    task-review
         |
         v
    task-implement
         |
         v
    task-review recheck

If changes need to be undone:

    task-rollback preview
         |
         v
    Inspect recovery plan
         |
         v
    Approve selected operations
         |
         v
    task-rollback step/task

Rollback is optional and independent from
the normal development sequence.

## 5. Skill Commands

### task-create

Create a structured task from a natural-language
description.

Example:

    $task-create Build an enemy spawning system
    for my 2D game. Enemies should appear outside
    the camera and move toward the player.

Output:

    TASK.md

Use when:

- Starting a new feature.
- Describing a new gameplay mechanic.
- Preparing a bug-fix task.
- Defining a significant change.

### task-ba

Transform the task into detailed gameplay
requirements and acceptance criteria.

Example:

    $task-ba TASK-20261108-001

Output:

    BA.md

Use when:

- Gameplay rules need clarification.
- Player interactions must be defined.
- Edge cases need analysis.
- Acceptance criteria are missing.

Resolve blocking gameplay questions before
planning affected implementation steps.

### task-analysis

Investigate existing Unity project systems.

Example:

    $task-analysis TASK-20261108-001

Output:

    ANALYSIS.md

Use when:

- Identifying existing reusable code.
- Investigating Scene and Prefab dependencies.
- Evaluating technical impact.
- Discovering implementation risks.

This skill does not modify the project.

### task-plan

Create an ordered implementation blueprint.

Example:

    $task-plan TASK-20261108-001

Output:

    PLAN.md

The plan should define:

- Architecture decisions.
- Components and responsibilities.
- File change manifest.
- Implementation steps.
- Unity Editor operations.
- Test procedures.
- Dependencies.
- Recovery approach.

The plan must not implement code.

### task-implement

Execute approved implementation steps.

Example:

    $task-implement TASK-20261108-001

Execute a selected range:

    $task-implement TASK-20261108-001 STEP-001..STEP-003

Resume incomplete implementation:

    $task-implement TASK-20261108-001 resume

Output:

    IMPLEMENTATION.md
    Approved project changes

The agent must:

- Protect existing user changes.
- Follow the approved plan.
- Record actual modifications.
- Capture recovery evidence.
- Validate each step.
- Report manual Unity Editor work.

### task-review

Independently inspect implementation correctness.

Example:

    $task-review TASK-20261108-001

Recheck after corrections:

    $task-review TASK-20261108-001 recheck

Review an individual step:

    $task-review TASK-20261108-001 STEP-003

Output:

    REVIEW.md

The reviewer must not modify implementation files.

### task-rollback

Preview or reverse task-owned modifications.

Preview:

    $task-rollback TASK-20261108-001 preview

Step-level rollback:

    $task-rollback TASK-20261108-001 step STEP-003

Full task rollback:

    $task-rollback TASK-20261108-001 task

Check previous recovery state:

    $task-rollback TASK-20261108-001 status

Output:

    ROLLBACK.md

Rollback requires reliable recovery evidence
and explicit approval of destructive operations.

## 6. Task Directory

Task documents are stored under:

    docs/tasks/

Example:

    docs/tasks/
      TASK-20261108-001-enemy-spawning/
        TASK.md
        BA.md
        ANALYSIS.md
        PLAN.md
        IMPLEMENTATION.md
        REVIEW.md
        ROLLBACK.md

Documents are created as the corresponding
skills execute.

Not every task requires a ROLLBACK.md.

## 7. Document Responsibilities

| Document          | Responsibility                      |
| ----------------- | ----------------------------------- |
| TASK.md           | Original user intent                |
| BA.md             | Functional requirements             |
| ANALYSIS.md       | Technical findings                  |
| PLAN.md           | Executable implementation blueprint |
| IMPLEMENTATION.md | Actual execution record             |
| REVIEW.md         | Review findings and verification    |
| ROLLBACK.md       | Recovery plan and results           |

Every document must preserve the distinction between
approved behavior and unverified assumptions.

## 8. Traceability

The workflow uses stable identifiers.

Example:

    FR-001 — Spawn enemies outside camera

    AC-001 — Validate spawn position

    STEP-002 — Implement random spawning

    TEST-001 — Verify off-camera spawning

    RV-001 — Incorrect spawn position

These identifiers connect requirements, code changes,
tests, and review findings.

Do not renumber existing identifiers unnecessarily.

## 9. Execution and Approval

Not all workflow stages modify code.

| Skill          | Changes Project Code?  |
| -------------- | ---------------------- |
| task-create    | No                     |
| task-ba        | No                     |
| task-analysis  | No                     |
| task-plan      | No                     |
| task-implement | Yes                    |
| task-review    | No                     |
| task-rollback  | With explicit approval |

Planning does not automatically authorize
implementation.

Review does not automatically authorize fixes.

Rollback previews do not authorize file deletion.

## 10. Implementation Status

Implementation-level status:

    COMPLETED
    PARTIAL
    BLOCKED
    FAILED

Step-level status:

    NOT_STARTED
    IN_PROGRESS
    COMPLETED
    IMPLEMENTED_UNVERIFIED
    MANUAL_REQUIRED
    BLOCKED
    FAILED
    SKIPPED
    SUPERSEDED

A step is not COMPLETED merely because
its C# code has been written.

Unity Scene configuration and mandatory validation
must also be completed where applicable.

## 11. Review Results

Review verdicts:

    PASS
    PASS_WITH_WARNINGS
    CHANGES_REQUIRED
    INCOMPLETE
    BLOCKED

If review finds defects:

1. Read REVIEW.md.
2. Identify findings requiring correction.
3. Return to task-implement.
4. Apply corrections within approved scope.
5. Run task-review recheck.
6. Accept only after required validation.

For targeted fixes, supply the relevant RV IDs
and explicitly instruct task-implement to address
the review findings.

If a correction changes gameplay requirements,
return to task-ba.

If it changes the technical architecture,
return to task-plan.

## 12. Git and Backup Recommendations

Initialize Git before AI implementation:

    git init

Use a Unity-appropriate .gitignore.

Commit a known-good baseline before beginning
large AI-driven changes.

Avoid committing generated Unity directories.

Recommended ignored locations include:

    Library/
    Temp/
    Obj/
    Logs/
    Builds/
    UserSettings/
    .vs/
    .agent-recovery/

Recovery snapshots should stay outside Assets/.

Do not depend only on Git for unfinished
or uncommitted work.

task-implement records pre-change recovery evidence.

task-rollback uses that evidence to identify
recoverable task-owned changes.

Never perform broad destructive Git operations
to recover a single AI task.

## 13. Unity Editor Responsibilities

AI-generated C# files may require manual setup.

Examples:

- Attach a MonoBehaviour to a GameObject.
- Assign Prefab references.
- Configure serialized fields.
- Set Collider or Rigidbody parameters.
- Configure Input Actions.
- Configure Scene references.
- Verify animation transitions.
- Run the game in Play Mode.

task-implement must document any required manual work.

task-review must not assume this work was completed
unless verified.

## 14. Common Issues

### Codex cannot find a skill

Check:

- The skill directory exists.
- SKILL.md has YAML frontmatter.
- The skill name is correct.
- Codex started inside the intended project.
- The Codex session has been refreshed.

### Implementation succeeds but gameplay fails

Check:

- Unity Console errors.
- Missing components.
- Inspector assignments.
- Scene configuration.
- Prefab references.
- Game-state transitions.
- Play Mode behavior.

Then run task-review.

### Agent implements the wrong feature

Check:

- Original TASK.md.
- Confirmed BA requirements.
- Open BA questions.
- Plan scope.
- Actual implementation deviations.

Return to the earliest incorrect workflow stage.

### Agent cannot undo changes

Check:

- Git availability.
- Pre-task Git state.
- IMPLEMENTATION.md.
- Recovery snapshots.
- task-rollback preview.

If no valid recovery source exists,
complete restoration may be impossible.

### Review finds defects

Use task-implement for corrections.

Then run task-review recheck.

Do not edit REVIEW.md merely to mark
an unresolved defect as fixed.

## 15. Best Practices

For solo development:

- Keep tasks reasonably small.
- Build one playable mechanic at a time.
- Maintain a working gameplay baseline.
- Use Git before major implementation.
- Review important AI-generated changes.
- Validate Scenes and Prefabs in Unity Editor.
- Test actual gameplay, not only compilation.
- Keep recovery snapshots until work is accepted.
- Do not run unrelated tasks concurrently on
  the same Scene or Prefab without coordination.

A small verified implementation is preferable
to a large untested feature.

## 16. Workflow Maintenance

When editing a skill:

1. Update the corresponding SKILL.md.
2. Check compatibility with AGENTS.md.
3. Check related handoff documents.
4. Update README.md if commands change.
5. Test the revised skill on a small task.

Shared conventions belong in AGENTS.md.

Detailed procedures belong in SKILL.md.

Developer-facing instructions belong in README.md.

## 17. Acceptance

A feature is ready for acceptance when:

- Approved functionality is implemented.
- Required tests are complete.
- Required Unity configuration is verified.
- Blocking review findings are resolved.
- Known limitations are documented.
- The developer accepts the result.

Final acceptance belongs to the developer.

The objective is reliable AI-assisted development,
not fully autonomous changes without oversight.

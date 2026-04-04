# Basic workflow

The basic building block is an epic. An epic is the smallest deliverable that makes sense.

## Branches

- `devel` — working branch
- `epic/<slug>` — epic branch
- `main` — release branch

**Always** check that you are on the right branch.

## Phases

All phase documents live under `doc/epic-<slug>/`.

### Phase 1 — Epic

**Owner:** Human (AI assists)

Input: `SPEC.md`

Human writes the epic.

AI:

- Checks spelling and grammar. Corrects without confirmation.
- Checks style. Does not accept ambiguity. Proposes readability improvements.
- Checks against INVEST criteria.
- Checks that each story has a clear user role, action, and outcome (who / what / why).
- Flags stories that are too large to deliver in a single epic.

Phase gate:

- All checks passed.
- Human acceptance.
- Commit means acceptance.

> [!WARNING]
> If SPEC changes after Phase 2 or later, restart from Phase 2. DESIGN and PLAN are invalidated.

### Phase 2 — Design

**Owner:** AI + Human

Input: `SPEC.md`
Output: `DESIGN.md` — AI-drafted with human oversight.

Each story from SPEC must be traceable to at least one feature. Multiple stories may merge into one feature; one story may split into several. Document the mapping.

Contains:

- **System context.** How the new feature fits into the existing system.
- **Dependencies.** Upstream and downstream. Focus on clear boundaries between system components and proper dependency directions according to Uncle Bob's The Clean Architecture blog post.
- **Interfaces.** API, CLI, and so on.
- **Introduced data structures.**
- **Feature details.** Including:
  - Technical decisions with rationales.
  - Test cases.
- **NFRs.** Such as security and observability needs.

Phase gate:

- All stories traceable to at least one feature. Mapping documented.
- All dependencies conform to Clean Architecture.
- No open decisions remain.
- All technical notes from SPEC reflected in feature details or NFRs.
- Human acceptance.
- Commit means acceptance.

### Phase 3 — Plan

**Owner:** AI + Human

Input: `DESIGN.md`
Output: `PLAN.md` — AI-drafted with human oversight.

Contains:

- **Task list.** Every feature becomes a task. Each task description includes:
  - Feature name.
  - Parallelism constraints.
  - List of changed files with a brief description of planned changes.
  - List of tests to be created, derived from design test cases.
- **Execution order.** Tasks may be grouped by dependencies.

Phase gate:

- Each feature covered.
- Each test case covered.
- No task larger than a single working session.
- Human acceptance.
- Commit means acceptance.

### Phase 4 — Execution

**Owner:** AI (human consulted when necessary)

Input: `PLAN.md`
Progress tracked in: `EXECUTION-PROGRESS.md`

```text
Pick next task
    → Code
    → Run relevant tests
    → Tests pass?
        Yes → Commit
                → More tasks?
                    Yes → (repeat from top)
                    No  → Done
        No  → Fix and report
                → Run tests again
                → Tests pass?
                    Yes → Commit
                    No  → Stop — report to human
```

#### Boundaries

**Always:**

- Stay within the task's file scope.
- Run tests after each task.
- Commit after task is complete.

**Ask first:**

- Changing files not listed in the task.
- Adding new dependencies.

**Never:**

- Skip a failing test.
- Modify SPEC or PLAN without human approval.
- Reorder Execution Order without human approval.

#### Phase gate

- All tests successful.
- All tasks finished.

> [!NOTE]
> No human acceptance required. Passing tests are the acceptance criterion.

#### Cleanup

After a successful cycle, AI:

- Merges to `devel`.
- Deletes the epic branch.

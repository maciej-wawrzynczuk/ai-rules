# Basic workflow

The basic building block is an epic. An epic is the smallest deliverable that makes sense.

## Branches

`devel` — the working branch
`epic/<slug>` — epic branch
`main` — release branch

**Always** check if we're on the right branch.

## Phases

### 1 - Epic: Human-created

Human creates branch.

Input file: `doc/epic-<slug>/SPEC.md`

Human: Writes the epic.

AI:

- Check spelling and grammar. Correct without confirmation.
- Check style. Do not accept ambiguity. Propose readability improvements.
- Check against INVEST criteria.
- Check that each story has a clear user role, action, and outcome (who / what / why).
- Flag stories that are too large to deliver in a single epic.

Phase gate:

- All checks passed.
- Human acceptance.
- Commit means acceptance.

### 2 - Design: AI + Human

Input file: `doc/epic-<slug>/SPEC.md`
Output file: `doc/epic-<slug>/DESIGN.md`. AI-drafted with human oversight.

Each story from SPEC must be traceable to at least one feature. Multiple stories may merge into one feature; one story may split into several. Document the mapping.

Contains:

- System context. How the new feature fits into the existing system.
- Dependencies. Upstream and downstream. Focus on clear boundaries between system components and proper dependency directions according to Uncle Bob's The Clean Architecture blog post.
- Interfaces. API, CLI, and so on.
- Introduced data structures.
- Feature details. Including:
  - Technical decisions with rationales.
  - Test cases.
- NFRs. Like security and observability needs.

Phase gate:

- All stories traceable to at least one feature. Mapping documented.
- All dependencies conform to Clean Architecture.
- No open decisions remain.
- All technical notes from SPEC reflected in feature details or NFRs.
- Human acceptance.
- Commit means acceptance.

### 3 - Plan: AI + Human

Input file: `doc/epic-<slug>/DESIGN.md`
Output file: `doc/epic-<slug>/PLAN.md`. AI-drafted with human oversight.

Contains:

- Tasks list. Every feature becomes a task. A task description includes:
  - Feature name
  - Parallelism constraints.
  - List of changed files.
  - Exact list of changes.
  - List of tests to be created. From test cases from design.
- Execution order. Tasks may be grouped according to dependencies.

Phase gate:

- Each feature covered.
- Each test case covered.
- No task larger than single working session.
- Human acceptance.
- Commit means acceptance.


### 4 - Execution: AI, human interaction when necessary

Run in loop:

Input file: `doc/epic-<slug>/PLAN.md`
Track progress in `doc/epic-<slug>/EXECUTION-PROGRESS.md`

1. Pick task.
2. Code.
3. Run all relevant tests.
4. If tests fail:
   1. Fix. Report changes.
   2. If still failing after one retry — stop and report to human.
5. Report task complete. Commit. Go to 1.

#### Boundaries

- **Always:** stay within the task's file scope
- **Always:** run tests after each task
- **Always:** commit after task complete
- **Ask first:** changing files not listed in the task
- **Ask first:** adding new dependencies
- **Never:** skip a failing test
- **Never:** modify SPEC or PLAN without human approval
- **Never:** reorder Execution Order without human approval

#### Phase gate

- All tests successful.
- All tasks finished.

> No human acceptance required. Passing tests are the acceptance criterion.

#### Cleanup

After a successful cycle, AI:

- Merges to `devel`
- Deletes the epic branch.
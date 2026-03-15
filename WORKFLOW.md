# Basic workflow

The basic building block is an epic. An epic is the smallest deliverable that makes sense.

## Phases

### 1 - Epic: Human-created

Input file: `doc/epic-<slug>/SPEC.md`

Human: Writes the epic.

AI:

- Check spelling and grammar. Correct without confirmation.
- Check style. Do not accept ambiguity. Propose readability improvements.
- Check against INVEST criteria.

Phase gate: Human acceptance.

### 2 - Design: AI + Human

Input file: `doc/epic-<slug>/SPEC.md`
Output file: `doc/epic-<slug>/DESIGN.md`. AI-drafted with human oversight.

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

- All stories covered.
- All dependencies conform to Clean Architecture.
- No open decisions remain.
- All technical notes from SPEC reflected in feature details or NFRs.
- Human acceptance.

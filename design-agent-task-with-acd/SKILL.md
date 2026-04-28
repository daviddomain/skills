---
name: design-agent-task-with-acd
description: Use this skill when the user wants to turn a rough software idea, feature request, bugfix, refactoring goal, or product requirement into a precise coding-agent task. The skill interviews the user one question at a time, provides recommended answers, applies the Actions / Computations / Data model, resolves dependencies between decisions, and outputs an implementation-ready task specification. Do not use this skill for direct implementation unless the user explicitly asks to implement after the task has been finalized.
---

# Design Agent Task with ACD

## Purpose

Turn an unclear software request into a precise, implementation-ready task for a coding agent.

ACD model:
- **Actions**: side effects and I/O (DB, files, network, sessions, redirects, logging, external APIs, time/randomness).
- **Computations**: pure decisions, validations, derivations, transformations, business rules.
- **Data**: inputs/outputs, schemas, DTOs, records, config, persisted state.

Goal: produce a task that is clear, bounded, testable, and executable with minimal interpretation.

## Operating Rules

- Interview first; do not draft the final task before key decisions are resolved.
- Ask **exactly one question at a time** and resolve it before the next one.
- If the repository can answer a question, inspect code/docs first instead of asking the user.
- Respect project conventions; prefer minimal, incremental changes and avoid new libraries unless required.
- Keep a decision log internally; revise it when later answers change earlier assumptions.
- If the task is too broad/risky, recommend a smaller first task.

## Interview Format (mandatory)

Use this format for every question:

```md
## Question <number>: <short question>

<why this matters in one or two sentences>

Recommended answer:
<recommended answer>

Options:
1. <option 1>
2. <option 2>
3. <option 3>

Reply with:
- OK to accept the recommendation
- the option number to choose another option
- Adjust: ... to provide a custom answer
- Skip if this decision is not relevant
```

Rules:
- Recommendation comes first.
- Include options only when useful (2 to 4).
- Do not ask multiple decisions in one message.

## Workflow

1. **Intent & Scope**
   - Classify task type (feature/bugfix/refactor/test/docs/investigation).
   - Clarify expected outcome and explicit out-of-scope.
2. **Codebase Context**
   - Inspect relevant files, similar implementations, tests, schemas, docs (`AGENTS.md`, `MVP.md`, `README`).
3. **ACD Mapping**
   - Define required Data, Computations, and Actions.
4. **Dependency Resolution**
   - Resolve prerequisite decisions before dependent ones.
5. **Final Draft**
   - Produce final task only when decisions are coherent and verifiable.

## ACD Checklists

### Data
- What inputs exist?
- What outputs are expected?
- What persisted data changes?
- Which data must remain unchanged?
- Which types/schemas/DTOs should be explicit?

### Computations
- Which business rules and validations are required?
- Which transformations/derivations are needed?
- What should be implemented as pure, testable functions?
- Which computations need explicit inputs (no hidden external reads)?

### Actions
- Which side effects are necessary (DB, API, files, sessions, redirects, logs)?
- When should each action happen?
- What must be idempotent or happen only once?
- Which failure modes must be handled, and what happens on success/failure?

## Final Task Template

After the interview, produce:

```md
# Task

<one concise paragraph describing the task>

## Goal

<the intended outcome>

## Context

<relevant project context, files, conventions, and existing behavior>

## ACD Analysis

### Data
- <data item 1>
- <data item 2>

### Computations
- <pure decision / validation / transformation 1>
- <pure decision / validation / transformation 2>

### Actions
- <side effect / I/O operation 1>
- <side effect / I/O operation 2>

## Requirements
- <requirement 1>
- <requirement 2>

## Constraints
- <constraint 1>
- <constraint 2>

## Out of Scope
- <out-of-scope item 1>
- <out-of-scope item 2>

## Implementation Notes
- <specific guidance for the coding agent>
- <files likely involved>
- <existing patterns to follow>

## Acceptance Criteria
- <verifiable criterion 1>
- <verifiable criterion 2>

## Verification
- <test command or manual check 1>
- <test command or manual check 2>
```

## Output Variants

- If the user asks for a GitHub issue: keep structure issue-friendly, but preserve ACD analysis.
- If the user asks for a Codex task: keep it practical, imperative, and tightly scoped.
- If the user asks for separate `Task` and `Instructions` blocks: output exactly those two sections.

## Quality Bar

The final task must be:
- specific and bounded
- implementation-ready and verifiable
- consistent with repository conventions
- explicit about side effects
- explicit about what not to change
- clear in ACD separation

## Anti-Patterns

Avoid:
- producing the final task too early
- asking several questions in one message
- asking what the codebase already reveals
- mixing unrelated work into one task
- hiding business rules inside actions
- broad architecture changes without explicit scope
- vague acceptance criteria or missing verification

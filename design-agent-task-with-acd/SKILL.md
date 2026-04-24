---
name: design-agent-task-with-acd
description: Use this skill when the user wants to turn a rough software idea, feature request, bugfix, refactoring goal, or product requirement into a precise coding-agent task. The skill interviews the user one question at a time, provides recommended answers, applies the Actions / Computations / Data model, resolves dependencies between decisions, and outputs an implementation-ready task specification. Do not use this skill for direct implementation unless the user explicitly asks to implement after the task has been finalized.
---

# Design Agent Task with ACD

## Purpose

This skill turns an unclear or rough software task into a precise, implementation-ready task for a coding agent.

It uses the ACD model:

- **Actions**: side effects, I/O, database access, network requests, file writes, cookies, sessions, redirects, logging, mutations, external APIs, time, randomness.
- **Computations**: pure decisions, validations, derivations, transformations, business rules, mappings from input data to output data.
- **Data**: facts, records, DTOs, schemas, configuration, state, inputs, outputs, serialized structures, plans, command objects.

The goal is to create a task that is clear, bounded, testable, and executable with minimal interpretation.

## Core Behavior

Before producing the final task, interview the user until the task is sufficiently clear.

Ask exactly one question at a time.

For every question:

1. Explain briefly why the question matters.
2. Provide one recommended answer.
3. When useful, provide 2 to 4 selectable options.
4. Tell the user how to answer.

Use this response format for every interview question:

```
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

If the question can be answered by inspecting the codebase, inspect the codebase instead of asking the user.

Do not ask the user for information that is already available in the repository, existing docs, issue text, README files, AGENTS.md, MVP.md, tests, routes, schemas, or nearby code.

Do not ask multiple questions in one message.

Do not proceed to the next decision until the current one is resolved.

## Interview Strategy

Move through the task as a decision tree.

Resolve dependencies before dependent decisions.

Prefer concrete implementation boundaries over abstract discussion.

If a decision has an obvious safe default, recommend it clearly.

If a task is too large, risky, or vague, recommend splitting it into smaller tasks.

Maintain a decision log internally while interviewing the user. Use it to avoid asking duplicate questions and to keep later recommendations consistent with earlier answers.

If a later answer changes an earlier assumption, explicitly revise the decision log before continuing.

## Phase 1: Task Intent

Clarify the basic intent:

- Is this a new feature, bugfix, refactor, cleanup, test task, documentation task, architecture task, or investigation task?
- What user-visible or developer-visible outcome is expected?
- What should be explicitly out of scope?
- What files, modules, routes, components, APIs, or workflows are likely involved?
- Is the task intended for implementation now, planning only, issue creation, or review preparation?

Recommended default:
If the user's request is about implementation, create a small implementation task with explicit acceptance criteria and verification steps.

## Phase 2: Codebase Context

Inspect relevant project files when available.

Look for:

- existing conventions
- similar implementations
- related components
- schemas and validations
- server actions, API routes, services, repositories, utilities
- tests
- documentation such as AGENTS.md, MVP.md, README files, or project-specific docs

Respect existing project conventions.

Do not introduce new libraries, frameworks, patterns, or abstractions unless the task explicitly requires them.

Prefer minimal, incremental, reviewable changes.

## Phase 3: ACD Mapping

Identify the task using the ACD model.

### Data

Clarify:

- What input data exists?
- What output data is expected?
- What persisted data is affected?
- What types, schemas, DTOs, database columns, form values, route params, or config values are involved?
- What data shape should be explicit?
- What data must remain unchanged?
- Can a behavior or decision be represented as data, such as a plan, command, event, config object, result object, or validation result?

Prefer explicit data structures over implicit assumptions.

### Computations

Clarify:

- What decisions must be made?
- What validations are required?
- What derivations or transformations are needed?
- What business rules apply?
- What can be implemented as pure functions?
- What should be testable without I/O?
- Which computations should receive all required inputs explicitly instead of reading external state?

Prefer extracting business decisions into computations instead of hiding them inside actions.

A computation should be safe to run repeatedly with the same input and should produce the same output without changing the outside world.

### Actions

Clarify:

- What side effects are necessary?
- What database reads or writes are required?
- What APIs, files, sessions, cookies, redirects, logs, emails, or external services are involved?
- When should each action happen?
- What must happen only once?
- What must be idempotent?
- What error cases must be handled?
- What should happen after an action succeeds?
- What should happen after an action fails?

Keep actions small, explicit, and close to system boundaries.

Avoid hidden actions inside utility functions, validation functions, mapping functions, or UI components when those actions can be isolated.

## Phase 4: Dependency Resolution

Resolve dependencies between decisions.

Examples:

- Do not ask about UI confirmation behavior before the destructive action is defined.
- Do not ask about validation messages before validation rules are clear.
- Do not ask about tests before expected behavior and edge cases are clear.
- Do not ask about persistence before the affected data model is clear.
- Do not ask about implementation files before the target workflow is clear.
- Do not ask about error handling before the possible failing actions are known.

When a dependency is unresolved, ask about the dependency first.

When the user asks for a broad task, identify the smallest useful first task and recommend it.

## Phase 5: Scope Control

Before drafting the final task, check:

- Is the task small enough for one coding-agent run?
- Are there hidden follow-up tasks?
- Are there risky migrations or broad refactors?
- Are there ambiguous business rules?
- Are there unnecessary technical changes?
- Are there parts that should be explicitly out of scope?
- Is the final task verifiable through tests, commands, or manual checks?

If needed, propose a smaller first task.

Prefer one clearly reviewable task over one large multi-purpose task.

## Phase 6: Final Task Drafting

After the interview is complete, produce the final task.

The final task must include:

- a concise task summary
- goal
- context
- ACD analysis
- requirements
- constraints
- out of scope
- implementation notes
- acceptance criteria
- verification steps

Use this structure:

```
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

## Output Rules

Do not produce the final task until the key decisions are resolved.

Do not implement code as part of this skill unless the user explicitly asks for implementation after the final task has been accepted.

If the user asks for a GitHub issue, adapt the final output into an issue-friendly structure while preserving the ACD analysis.

If the user asks for a Codex task, keep the final output practical, imperative, and scoped for a coding agent.

If the user asks for separate "Task" and "Instructions" blocks, produce exactly those two sections after the interview is complete.

## Quality Rules

The final task must be:

- specific
- bounded
- implementation-ready
- testable
- consistent with existing project conventions
- minimal in scope
- explicit about side effects
- clear about what not to change
- clear about which decisions are computations
- clear about which operations are actions
- clear about which data shapes, schemas, or records are involved

## Recommended Defaults

Use these defaults unless the user or repository context suggests otherwise:

- Prefer existing project conventions over new abstractions.
- Prefer pure computations for business rules and validations.
- Prefer explicit result objects for complex decisions.
- Prefer small actions at system boundaries.
- Prefer incremental changes over broad refactors.
- Prefer tests for computations when test infrastructure exists.
- Prefer manual verification steps when automated tests are not available.
- Prefer asking one decisive question over asking many broad questions.
- Prefer recommending a safe default instead of presenting all possible alternatives.

## Anti-Patterns to Avoid

Avoid:

- producing a task too early
- asking several questions at once
- asking questions the codebase can answer
- mixing unrelated feature work into one task
- hiding business rules inside actions
- introducing new libraries without necessity
- creating broad architecture changes without explicit scope
- writing vague acceptance criteria
- omitting verification steps
- treating ACD as theory instead of using it to clarify implementation boundaries

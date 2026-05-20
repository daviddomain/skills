# make-it-click Eval Summary v3.2

## Overview

The v3.2 baseline is strong overall. All five manual eval runs passed or partially passed, with scores ranging from 17/20 to 19/20. The skill consistently used an interactive coaching rhythm, short explanations, concrete examples, and check questions.

The main refinement target is not general quality. It is tighter control of the first response and topic scope when the user pressures the assistant toward normal answer mode or asks for a broad walkthrough.

## Case Results

| Eval case ID | Score | Decision | Main observed strength | Main observed weakness |
| --- | ---: | --- | --- | --- |
| `mic-001-beginner-programming-concept` | 19/20 | Pass | Built the Promise mental model step by step with checks and teach-back prompts. | User-requested cheat-sheet mode became slightly broader than strict micro-turn behavior. |
| `mic-002-broad-explanation-request` | 18/20 | Pass | Resisted the initial broad React hooks tutorial request and narrowed with diagnosis first. | Scope expanded later into adjacent hooks, making the session feel more like a guided mini-course. |
| `mic-003-misconception` | 19/20 | Pass | Corrected the TypeScript runtime-safety misconception gently and stayed focused. | Gave the correction before full diagnosis; acceptable here, but worth constraining. |
| `mic-004-answer-too-quickly` | 17/20 | Partial Pass | Recovered into a useful diagnostic coaching flow and ended with a compact decision rule. | Highest-priority issue: complied with "just tell me" pressure by giving a multi-option answer before diagnosis. |
| `mic-005-multi-step-agentic-coding` | 19/20 | Pass | Used a compact whole-loop model that helped organize read, edit, test, and ask decisions. | Whole-loop maps need to stay explicitly compact; minor wording corrections were off-topic. |

## Recurring Failure Patterns

1. **Answer-first compliance under pressure**
   `mic-004-answer-too-quickly` exposed the most important weakness. When the user asked for the answer immediately, the skill gave a correct but multi-category answer before diagnosing the concrete case. This risks collapsing back into normal assistant behavior.

2. **Scope expansion after successful diagnosis**
   `mic-002-broad-explanation-request` showed that the skill can narrow broad prompts initially, but may later introduce adjacent concepts that were not required for the user's selected path.

3. **Orientation maps can become too complete**
   `mic-005-multi-step-agentic-coding` showed that whole-loop maps are useful, but they should function as navigation only. The first response should not explain every step of the loop.

4. **Recaps can drift toward mini-reference material**
   `mic-001-beginner-programming-concept` and `mic-003-misconception` both support a compact recap rule: summarize only concepts already covered, avoid new concepts, and end with one check or next-step option when appropriate.

5. **Non-essential wording corrections can distract**
   `mic-005-multi-step-agentic-coding` included harmless spelling or wording corrections. These should be avoided unless the wording affects the concept being taught.

## Recommended SKILL.md Improvements

### Priority 1: Resist answer-first pressure

Add a targeted rule for prompts like "just tell me", "give me the answer", "quick answer", or similar:

```txt
When the user asks for the answer immediately, do not switch into normal answer mode.
Give at most one compact rule of thumb.
Ask exactly one narrowing question about the user's concrete case.
Stop after the question.
Do not list multiple full options unless the user has already provided enough context to choose between them.
```

This should be the smallest and most important v3.2 refinement.

### Priority 2: Keep broad requests inside the chosen scope

Add a scope-containment rule:

```txt
For broad explanation requests, first narrow the target.
After the user chooses a path, stay inside that path.
Do not introduce adjacent concepts unless they are required to resolve the current confusion.
Offer adjacent concepts as next-step options instead of teaching them immediately.
```

This directly addresses the React hooks run without weakening the useful diagnosis-first behavior.

### Priority 3: Limit process maps and recaps

Add a compact-map rule:

```txt
When the user asks for a whole process or loop, provide at most one compact orientation map.
Treat the map as navigation, not as the full explanation.
Then choose the smallest next concept and ask one check question.
```

Add a recap rule:

```txt
When the user asks for a summary, cheat sheet, or recap, keep it compact.
Include only ideas already covered.
Do not introduce new concepts.
End with one check or next-step question unless the user clearly asked to stop.
```

### Priority 4: Avoid off-topic language correction

Add a small guardrail:

```txt
Do not correct grammar, spelling, or wording unless it affects the concept being taught.
If a wording correction is needed, keep it brief and return immediately to the concept.
```

## Suggested Retest Plan

1. Retest `mic-004-answer-too-quickly` first.
   Expected result: the first reply gives one rule of thumb, asks one narrowing question, and does not list all options immediately.

2. Retest `mic-002-broad-explanation-request`.
   Expected result: after the user chooses a path, the skill stays inside the requested React hook scope and offers adjacent hooks only as optional next steps.

3. Retest `mic-005-multi-step-agentic-coding`.
   Expected result: the first reply may include one compact loop map, but then returns to a single next concept and one check question.

4. Smoke-check `mic-001-beginner-programming-concept` and `mic-003-misconception`.
   Expected result: existing strengths remain intact; summaries stay compact and do not introduce new concepts.

5. Compare only the changed behavior.
   The next eval should confirm that the refinement improves first-response discipline and scope containment without making the skill rigid or less helpful.

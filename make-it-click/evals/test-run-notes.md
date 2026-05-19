# make-it-click Manual Test Run Notes

## Run Metadata

- Date:
- Skill version:
- Codex version or environment, if known:
- Reviewer:

## Eval Result

- Eval case ID:
- Prompt used:

### Observed Behavior

-

### Score Summary

- Diagnosis first:
- One tiny idea per reply:
- One compact example maximum:
- One check question:
- Stops after the check question:
- No full tutorial:
- No premature solution:
- Maintains coach role:
- Responds to the user's actual confusion:
- Keeps language concise and concrete:
- Total:

### Failure Pattern

-

### Possible SKILL.md Improvement

-

### Decision

- Keep baseline:
- Needs skill change:
- Retest needed:
- Notes:

---

## Test Run: mic-001-beginner-programming-concept

### Date

2026-05-18

### Skill Version

v3.2 baseline

### Codex Version / Environment

Not recorded.

### Eval Case ID

`mic-001-beginner-programming-concept`

### Prompt Used

```txt
I am new to JavaScript and promises make no sense to me. Why does my code keep running before the data is ready?
```

### Observed Behavior

The skill performed well and followed the intended interactive coaching rhythm.

It correctly identified the likely confusion as an execution-order problem and started with a small diagnostic multiple-choice question. The following turns stayed focused on one concept at a time:

1. A Promise is not the data, but a placeholder/future result.
2. JavaScript starts async work and continues running.
3. Starting async work and finishing async work are different moments.
4. `await` pauses only the current `async` function.
5. Code that needs the result should run after the relevant `await`.

The skill used short examples, asked check questions after each small explanation and waited for user responses. It also used teach-back prompts, which helped confirm whether the concept was actually clicking.

A useful correction happened when the user incorrectly answered that `await` pauses the whole JavaScript program. The skill corrected this gently and narrowed the focus to the key beginner trap.

The final “Follow-up Capsule” was helpful and captured the current understanding, remaining weak spot and next practice question.

### Score Summary

| Criterion                               | Score | Notes                                                                                                                                                                                                                      |
| --------------------------------------- | ----: | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Diagnosis first                         |     2 | Started with a clear hypothesis and diagnostic options.                                                                                                                                                                    |
| One tiny idea per reply                 |     2 | Mostly excellent. Each turn focused on one small concept.                                                                                                                                                                  |
| One compact example maximum             |     1 | Mostly good, but the cheat-sheet section included multiple reinforcing forms: code, mental image, trap, usage rule. Acceptable because the user asked for a cheat sheet, but slightly broader than strict micro-turn mode. |
| One check question                      |     2 | Consistently used check questions.                                                                                                                                                                                         |
| Stops after the check question          |     2 | Generally stopped and waited after checks.                                                                                                                                                                                 |
| No full tutorial                        |     2 | Did not dump a full Promise tutorial. Built understanding step by step.                                                                                                                                                    |
| No premature solution                   |     2 | Did not jump straight to `async/await` syntax before establishing the mental model.                                                                                                                                        |
| Maintains coach role                    |     2 | Stayed in coach mode across many turns.                                                                                                                                                                                    |
| Responds to the user’s actual confusion |     2 | Stayed tightly focused on “why does my code keep running before data is ready?”                                                                                                                                            |
| Keeps language concise and concrete     |     2 | Clear, compact and beginner-friendly.                                                                                                                                                                                      |

**Total:** 19 / 20

### Failure Pattern

No major failure.

Minor pattern observed:

When the user selected “summarize as cheat sheet,” the skill became slightly more explanation-like and introduced several reinforcing elements in one reply. This was still useful and contextually appropriate, but it is the one place where the strict “one tiny idea” rule softened.

### Possible SKILL.md Improvement

Consider adding a small rule for user-requested summaries or cheat sheets:

```txt
When the user asks for a summary, cheat sheet, or recap, keep it compact.
A summary may include several already-covered ideas, but must not introduce new concepts.
After the recap, ask one check or next-step question and stop.
```

This would preserve flexibility for summaries while preventing the skill from sliding into normal explanation mode.

### Decision

Pass.

The current `make-it-click` skill behavior is strong for this eval case. No immediate SKILL.md change is required based on this single run, but the summary/cheat-sheet behavior should be watched in future evals.

---

## Test Run: mic-002-broad-explanation-request

### Date

2026-05-18

### Skill Version

v3.2 baseline

### Codex Version / Environment

Not recorded.

### Eval Case ID

`mic-002-broad-explanation-request`

### Prompt Used

```txt
Can you explain React hooks, state, effects, context, memoization, and when to use all of them?
```

### Observed Behavior

The skill handled the broad explanation request well at the beginning.

Instead of immediately dumping a full React hooks tutorial, it identified the likely confusion as a relationship problem between `state`, `effects`, `context` and `memoization`. It then asked a diagnostic multiple-choice question and let the user choose a narrower goal.

After the user selected the practical decision-guide path, the skill provided a compact overview table and then proceeded in small interactive steps:

1. `useState` for local remembered UI data.
2. `useEffect` for syncing with outside React.
3. `useContext` for shared values from a provider.
4. `useMemo` for cached calculated values.
5. `useCallback` for stable function references.
6. Derived data vs source data.
7. `useReducer` for event-like state transitions.
8. `useRef` for values that should persist without causing renders.

The strongest behavior was the repeated use of small checks and teach-back prompts. The skill corrected misconceptions gently, especially around `useMemo` not directly preventing re-renders and around derived values such as `isEmpty`.

The main weakness: the session gradually expanded beyond the original scope. `useReducer` and `useRef` were introduced even though the prompt did not explicitly ask for them. This was still useful, but it made the run feel more like a guided mini-course than a strict micro-turn coaching session.

The final “What Clicked”, “Practical Default” and “Follow-Up Capsule” were helpful and captured a useful mental model. However, the summary became fairly broad and covered more concepts than the initial eval case required.

### Score Summary

| Criterion                               | Score | Notes                                                                                                 |
| --------------------------------------- | ----: | ----------------------------------------------------------------------------------------------------- |
| Diagnosis first                         |     2 | Started with a useful hypothesis and diagnostic options instead of explaining everything immediately. |
| One tiny idea per reply                 |     1 | Mostly followed micro-turns, but some turns introduced broader maps or multiple concepts at once.     |
| One compact example maximum             |     2 | Generally used at most one example per concept.                                                       |
| One check question                      |     2 | Consistently asked one focused check question.                                                        |
| Stops after the check question          |     2 | Generally stopped and waited after each check.                                                        |
| No full tutorial                        |     1 | Avoided a first-response tutorial, but the full session gradually became a broad guided tutorial.     |
| No premature solution                   |     2 | Did not jump straight to a complete decision guide without diagnosis.                                 |
| Maintains coach role                    |     2 | Stayed in coach mode throughout the interaction.                                                      |
| Responds to the user’s actual confusion |     2 | Stayed mostly focused on when to use which hook.                                                      |
| Keeps language concise and concrete     |     2 | Individual turns were clear and practical.                                                            |

**Total:** 18 / 20

### Failure Pattern

Minor scope expansion.

The skill successfully resisted the initial broad-explanation request, but later expanded the topic beyond the prompt by introducing additional hooks such as `useReducer` and `useRef`.

This suggests the skill can control breadth at the start, but may still broaden during longer sessions when it tries to create a complete conceptual map.

### Possible SKILL.md Improvement

Consider adding a rule for broad requests:

```txt
When the user asks a broad explanation question, first narrow the target.
After the user chooses a path, stay inside that chosen scope.
Do not introduce adjacent concepts unless they are required to resolve the user's current confusion.
If an adjacent concept may be useful, offer it as a next option instead of teaching it immediately.
```

Possible additional rule:

```txt
For broad concept-map topics, distinguish between:
- concepts explicitly requested by the user
- adjacent concepts that may be useful later

Teach only the explicitly requested concepts first.
Put adjacent concepts into the next-step options.
```

### Decision

Pass.

The skill behaved well enough for this eval case. No urgent change is required, but this run reveals a useful improvement area: stronger scope containment after the initial diagnosis.

---

## Test Run: mic-003-misconception

### Date

2026-05-19

### Skill Version

v3.2 baseline

### Codex Version / Environment

Not recorded.

### Eval Case ID

`mic-003-misconception`

### Prompt Used

```txt
I think TypeScript makes my app safe at runtime because it checks the props before React renders them. Is that right?
```

### Observed Behavior

The skill handled the misconception very well.

It correctly identified the core confusion as the difference between TypeScript compile-time checking and JavaScript runtime behavior. It directly corrected the misconception, but did so in a small and controlled way instead of turning the response into a full TypeScript tutorial.

The interaction progressed through a clean sequence:

1. TypeScript does not check React props at runtime.
2. TypeScript types are gone by the time React runs.
3. React receives ordinary JavaScript values.
4. Type annotations are removed rather than compiled into runtime checks.
5. Runtime validation must come from explicit checks or validation libraries.
6. External data boundaries need runtime validation.

The skill used short examples and repeatedly checked understanding with multiple-choice questions or teach-back prompts. The teach-back moment was especially useful because it confirmed that the user had internalized the main distinction.

A strong point was the gentle refinement after the user said TypeScript is “compiled to regular JavaScript.” The skill clarified that type annotations are removed, without overcorrecting or expanding into unrelated compiler details.

The final summary was compact and useful. It captured the core distinction clearly:

- TypeScript checks your code before runtime.
- React renders with plain JavaScript values.
- Runtime safety for external data comes from explicit checks, schemas, or validation libraries.

Minor issue: the first reply directly stated the correct answer before the diagnostic choice. For a misconception case, this is acceptable and probably useful, but it slightly weakens the pure “diagnosis first” rhythm. Also, the final summary ended without a final check or next-step question, though that is not a serious problem because the concept had already been confirmed.

### Score Summary

| Criterion                               | Score | Notes                                                                                                                                                               |
| --------------------------------------- | ----: | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Diagnosis first                         |     1 | It identified the likely confusion and asked a diagnostic question, but also gave the correction before the diagnostic answer. Acceptable for a misconception case. |
| One tiny idea per reply                 |     2 | Each turn focused on one small conceptual step.                                                                                                                     |
| One compact example maximum             |     2 | Examples were short and directly relevant.                                                                                                                          |
| One check question                      |     2 | Used clear checks throughout the interaction.                                                                                                                       |
| Stops after the check question          |     2 | Generally stopped and waited after each check.                                                                                                                      |
| No full tutorial                        |     2 | Avoided a broad TypeScript or React runtime explanation.                                                                                                            |
| No premature solution                   |     2 | Did not jump straight to Zod, validation libraries or implementation advice.                                                                                        |
| Maintains coach role                    |     2 | Stayed in interactive coach mode throughout.                                                                                                                        |
| Responds to the user’s actual confusion |     2 | Stayed tightly focused on TypeScript vs runtime safety.                                                                                                             |
| Keeps language concise and concrete     |     2 | Very clear, beginner-friendly and concrete.                                                                                                                         |

**Total:** 19 / 20

### Failure Pattern

No major failure.

Minor pattern observed:

For misconception prompts, the skill may directly correct the misconception before completing diagnosis. This worked well here, but should be watched in future evals. Direct correction is useful when the user asks “Is that right?”, but it should remain small and should not become a full explanation.

The final summary also did not include a final check question or next-step question. This is acceptable in this run, but the strict micro-turn rhythm would be slightly stronger if summaries ended with a compact next-step option or check.

### Possible SKILL.md Improvement

Consider adding a specific rule for misconception cases:

```txt
When the user states a misconception and asks whether it is right, give a brief correction first if needed.
Keep the correction to one tiny idea.
Then ask a diagnostic or teach-back question before adding the next concept.
Do not expand into a full explanation just because the misconception touches a broad topic.
```

Optional summary rule:

```txt
When closing with a recap, summarize only concepts already covered.
Do not introduce new concepts in the recap.
End with one small next-step option or check question, unless the user clearly asked to stop.
```

### Decision

Pass.

The skill behaved very well for this eval case. No urgent SKILL.md change is required. The run supports keeping the current misconception-handling style, with only a possible small clarification rule for direct corrections.

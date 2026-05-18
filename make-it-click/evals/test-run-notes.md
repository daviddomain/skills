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

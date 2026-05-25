---
name: context-relay
description: Use this skill when the user wants to compress a long existing conversation into a clean, portable handoff prompt for starting a new chat with the essential context preserved. Use it when the user asks to summarize a chat for a new thread, transfer context, create a continuation prompt, produce a compact project handoff, or preserve the important state from a long conversation without carrying all prior messages forward.
---

# Context Relay

## Purpose

Create a concise handoff prompt that lets a new chat continue from the useful state of a long conversation without importing all of its noise.

The output is not a generic summary. It is a ready-to-use first prompt for the next chat.

## Operating Rules

- Preserve only context that helps the next chat act correctly.
- Prefer concrete facts, decisions, constraints, filenames, commands, links, terminology, and unresolved questions.
- Remove small talk, repeated explanations, abandoned branches, and already-resolved troubleshooting details unless they explain an important decision.
- Keep uncertainty explicit. Mark details that are inferred, stale, unverified, or reconstructed from memory.
- Respect the user's language unless they request another language.
- Do not invent missing project state. If the source conversation is incomplete, say what is missing.
- Never include secrets, private tokens, credentials, or sensitive personal data unless the user explicitly confirms they are safe to carry forward.

## Workflow

1. Identify the next chat's likely task.
2. Extract the durable context:
   - goal and current state,
   - important decisions and rationale,
   - relevant files, systems, tools, commands, links, or identifiers,
   - constraints, preferences, and non-goals,
   - open questions and next steps.
3. Drop transient context:
   - conversation logistics,
   - false starts that no longer matter,
   - verbose explanations already converted into decisions,
   - command output that can be summarized as a result.
4. Produce one copy-ready handoff prompt.
5. If useful, add a short "Omitted" note after the prompt to explain what was intentionally left out.

## Output Format

Default output:

```md
Use this as the first prompt in the new chat:

---
<copy-ready handoff prompt>
---
```

The handoff prompt should normally use this structure:

```md
I want to continue work from a previous conversation.

Goal:
<what we are trying to accomplish>

Current state:
<what is already known or done>

Important context:
- <fact, decision, constraint, file, command, or identifier>
- <fact, decision, constraint, file, command, or identifier>

Preferences and constraints:
- <how the assistant should behave>
- <what not to change or assume>

Open questions:
- <question or uncertainty>

Next step:
<the most useful next action for the new chat>
```

## Compression Guidance

Adjust the level of detail to the likely next use:

- **Quick continuation**: 150-300 words.
- **Project handoff**: 300-700 words.
- **Technical debugging relay**: include exact errors, commands, environment, changed files, and what has already been ruled out.
- **Concept-learning relay**: include the user's current mental model, what clicked, what remains confusing, and the preferred teaching style.

If the conversation is very long, optimize for decision density over narrative completeness.

## Quality Bar

A good relay prompt is:

- self-contained enough for a new chat to act without asking for the entire old conversation,
- short enough that the user would actually paste it,
- explicit about constraints and next steps,
- honest about uncertainty,
- free of unnecessary transcript detail.

## Anti-Patterns

Avoid:

- writing a chronological transcript recap,
- preserving every intermediate attempt,
- hiding open questions as facts,
- producing a vague summary with no next action,
- including secrets or sensitive data by default,
- making the new chat depend on unavailable prior messages.

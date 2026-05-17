# Agent Skills

This repository contains custom skills for Codex and other AI coding agents. A skill is a small, reusable instruction package that gives an agent a clear workflow, quality bar, and output format for a specific kind of task.

## Available Skills

| Skill                        | Purpose                                                                                                                                                                                                               | When to use it                                                                                                                                                                                                           |
| ---------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `design-agent-task-with-acd` | Turns a rough idea, feature request, bugfix, or refactoring goal into a precise, implementation-ready agent task. It uses the **Actions / Computations / Data (ACD)** model and resolves open decisions step by step. | Use it when a vague requirement should become a clear Codex task, GitHub issue, or implementation brief. It is not meant for direct implementation unless the user explicitly asks for that after the task is finalized. |
| `make-it-click`              | Helps users truly understand a topic by finding knowledge gaps, building a mental model, using examples, and checking understanding through teach-back.                                                               | Use it when a concept is unclear, hard to picture, or when the user wants an interactive explanation instead of a long lecture.                                                                                          |

## Installation

The recommended way to install a skill is through the `skills` CLI:

```bash
npx skills@latest add daviddomain/<skill-name>
```

Replace `<skill-name>` with the skill you want to install:

```bash
npx skills@latest add daviddomain/design-agent-task-with-acd
npx skills@latest add daviddomain/make-it-click
```

Restart Codex or reload the skill list after installation so the new skills are detected.

## Usage

Skills are activated when they are explicitly mentioned or when a request clearly matches their description.

Examples:

```text
Use the design-agent-task-with-acd skill and turn this feature idea into a Codex task: ...
```

```text
Use make-it-click to explain event sourcing until I can apply it.
```

## Skill Structure

Each skill lives in its own folder and contains at least one `SKILL.md` file:

```text
skill-name/
  SKILL.md
```

The `SKILL.md` frontmatter defines the skill name and a short description. The body contains the actual workflow, rules, quality criteria, and output formats.

## Maintenance Notes

- Keep skill folders small and focused.
- Write the frontmatter description so it is clear when the skill should activate.
- Keep workflows concrete: clear steps, clear boundaries, clear output.
- Test new skills locally before using them broadly.

# SuperDev

SuperDev is a spec / plan driven development standard for AI-assisted engineering.

The core rule is simple:

**Clarify the target architecture before substantial implementation.**

Every long-lived repository or module should keep its architecture and execution state visible through:

- `SPEC.md`: what the system is, where its boundaries are, and how it should be built.
- `PLAN.md`: what is done, what is next, who owns it, what risks remain, and what evidence proves progress.

Every substantial `SPEC.md` must contain two Mermaid diagrams:

- `Current Architecture`: the architecture the current code actually implements.
- `Target Architecture`: the architecture the current work is moving toward.

If the target Mermaid diagram is missing, vague, or inconsistent with the requested work, SuperDev says: stop and clarify the architecture before writing production code.

## What Is In This Repo

- `SKILL.md`: the reusable Codex skill. Invoke it as `$superdev`.
- `AGENTS.md`: instructions for Codex and other general coding agents.
- `CLAUDE.md`: instructions for Claude Code.
- `agents/openai.yaml`: UI metadata for Codex skill discovery.

## Why Is There An `agents/` Folder?

The `agents/` folder is not an implementation of multiple agents.

It exists because Codex skills can include product-facing metadata under `agents/openai.yaml`. That file tells Codex how to display the skill in the UI:

- display name
- short description
- default prompt

So the folder is part of the skill packaging format. The actual SuperDev behavior lives in `SKILL.md`, `AGENTS.md`, and `CLAUDE.md`.

## How To Use

In a Codex environment, install or reference this repository as a skill and invoke:

```text
$superdev
```

For repositories that should always follow this standard, copy or adapt:

- `AGENTS.md` for Codex/general agents
- `CLAUDE.md` for Claude Code

Then maintain root and module-level `SPEC.md` / `PLAN.md` files as the project grows.

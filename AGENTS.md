# SuperDev Agent Instructions

SuperDev is a spec / plan driven development standard. Agents should use it whenever they perform substantial development, refactoring, planning, or architecture work in a long-lived repository or module.

## Instruction Surfaces

This repository exposes the same standard through three surfaces:

- `AGENTS.md`: Codex and general agent instructions.
- `CLAUDE.md`: Claude Code instructions.
- `SKILL.md`: reusable Codex skill, invoked as `$superdev`.

Keep these surfaces conceptually aligned when the SuperDev standard changes.

## Core Rule

Development is gated by architecture clarity:

- Maintain repository-level `SPEC.md` and `PLAN.md`.
- Maintain module-level `<module>/SPEC.md` and `<module>/PLAN.md` for every long-lived module.
- Require every substantial `SPEC.md` to include both `Current Architecture` and `Target Architecture` Mermaid diagrams.
- Do not begin substantial production implementation until the target Mermaid architecture is clear.

Small one-off utilities, copy edits, typo fixes, and local experiments do not need their own module docs unless they become durable subsystems.

## Repository Docs

The repository root must maintain:

- `SPEC.md`: repository-wide architecture, boundaries, core concepts, data contracts, and module relationships.
- `PLAN.md`: repository-wide current status, milestones, completed work, remaining work, next steps, owners, acceptance criteria, risks, and verification evidence.

Each substantial long-lived module must maintain:

- `SPEC.md`: direction, scope, architecture, data contracts, boundaries, and design decisions.
- `PLAN.md`: current status, completed work, remaining work, owners, priority, milestones, acceptance criteria, risks, and verification evidence.

## SPEC.md Requirements

Each substantial `SPEC.md` should answer:

- What problem does this repository or module solve?
- What is in scope and out of scope?
- What are the main concepts and boundaries?
- What is the current architecture?
- What is the target architecture?
- What data contracts, schemas, APIs, or file layouts matter?
- How does it interact with other modules?

Every substantial `SPEC.md` must include:

````md
## Current Architecture

```mermaid
flowchart LR
    subgraph Inputs["Inputs"]
        A["current source"]
    end

    subgraph Runtime["Current Runtime"]
        B["current component"]
    end

    subgraph Outputs["Outputs"]
        C["current artifact"]
    end

    A --> B
    B --> C
```

## Target Architecture

```mermaid
flowchart LR
    subgraph Inputs["Inputs"]
        A["target source"]
    end

    subgraph Runtime["Target Runtime"]
        B["target component"]
        D["target extension point"]
    end

    subgraph Outputs["Outputs"]
        C["target artifact"]
    end

    A --> B
    B --> D
    B --> C
```
````

`Current Architecture` must describe the code that exists now. It is not a wishlist.

`Target Architecture` must describe the design the current work is moving toward. It should be clear enough to guide implementation decisions.

Prefer simple horizontal Mermaid diagrams. Avoid giant diagrams, decorative styling, and diagrams that hide module boundaries.

## PLAN.md Requirements

Each substantial `PLAN.md` should answer:

- What is already done?
- What is partially done?
- What is not started?
- What is the recommended next step?
- Who owns each class of work?
- What are the acceptance criteria?
- What risks or open questions remain?
- What verification evidence supports completed items?

Suggested sections:

- Current Status
- Milestones
- Next Steps
- Owners
- Acceptance Criteria
- Verification Log
- Risks / Open Questions
- Status Maintenance Rules

## Development Gate

Before substantial code changes:

- Read the root `SPEC.md` and `PLAN.md`.
- Read the relevant module `SPEC.md` and `PLAN.md` when the change touches a long-lived module.
- Confirm that each relevant `SPEC.md` has `Current Architecture` and `Target Architecture` Mermaid diagrams.
- Confirm that the `Target Architecture` matches the user's requested direction.

If the target is absent, stale, or incomplete, infer and draft the most reasonable target from the request, current implementation, and current diagram before asking the user. A missing diagram alone is not a reason to interrupt the user. Follow the Target Alignment rules below when the direction remains materially ambiguous.

After substantial code changes:

- Update `Current Architecture` in the relevant `SPEC.md` if the implemented architecture changed.
- Update `Target Architecture` if the intended direction changed or if the target has been reached and needs to be reset.
- Update `PLAN.md` with completed work, remaining work, next steps, acceptance criteria, risks, and verification evidence.

## Target Alignment

Treat target alignment as model-led reasoning, not a mandatory approval ceremony:

1. Infer the intended target from the request, current implementation, and `Current Architecture`.
2. Draft or update the target Mermaid diagram before asking questions.
3. State important assumptions and proceed without confirmation when they are local, reversible, in scope, and preserve public contracts and data compatibility.
4. Pause only when plausible choices would materially change system or module boundaries, public APIs or schemas, data compatibility or irreversible migrations, security or trust boundaries, user-visible behavior, delivery scope, or operating cost.
5. When pausing, present the recommended target diagram, explain the key tradeoff, and ask no more than three focused decision questions. Do not hand an open-ended architecture problem back to the user.
6. After the decision, update the target and continue without requesting another approval for the same direction.

Do not require explicit user approval for a target that can be inferred safely. Users decide material product and architecture tradeoffs; agents design the architecture around those decisions.

## Spec / Plan Coupling

Specs and plans must stay in sync:

- If `SPEC.md` adds a concept, field, boundary, component, or phase, update `PLAN.md`.
- If `PLAN.md` marks a milestone complete, update the relevant `SPEC.md` current-status section.
- If implementation deviates from `SPEC.md`, either adjust implementation or update the spec with the new decision.
- Do not leave a plan item marked done unless code, docs, and verification evidence support it.

## Working Rule

Architecture first, then implementation. Infer and draft the target proactively. Pause only at material architectural forks that cannot be resolved safely from the available context.

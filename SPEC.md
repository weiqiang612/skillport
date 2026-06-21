# SkillPort Spec

Status: draft MVP

## Repository Roles

SkillPort separates the tool repository from user-owned SOP repositories.

### SkillPort tool repository

The public `skillport` repository contains the tool, specification, templates, and examples:

```text
README.md
README.zh-CN.md
SPEC.md
LICENSE
tools/
templates/
examples/
```

It should not contain a user's live skills as root-level `skills/` content.

### User SOP repository

Users maintain their own portable skills in a separate repository:

```text
my-agent-sops/
  skillport.yaml
  skills/
    <skill-name>/
      skill.yaml
      sop.md
      references/
      scripts/
      assets/
      maintenance.md
      migration.md
      agents/
        codex.md
        claude-code.md
        antigravity.md
```

## Portable Skill

A Portable Skill is a platform-neutral SOP source. It separates shared workflow knowledge from agent runtime details.

The source SOP is authoritative. Local agent skills are derived views maintained by the specific agent runtime.

## Required Files

### `skill.yaml`

Holds portable metadata.

```yaml
name: init-harness
description: Generates a complete Harness scaffold for a project.
version: 0.1.0
```

### `sop.md`

Contains agent-neutral workflow knowledge:

- task phases
- decision rules
- required outputs
- validation criteria
- shared references

It should avoid agent-only tool names, install paths, and runtime assumptions.

### `maintenance.md`

Defines how agents should maintain the shared SOP when editing their own local skill.

Minimum protocol:

```text
Shared workflow change -> update sop.md.
Agent-specific runtime change -> update local agent skill or agents/<agent>.md.
Unsure -> classify the change before editing.
```

### `agents/<agent>.md`

Optional notes for agent-specific local skill generation and maintenance.

These are not authoritative runtime packages. They are guidance for the target agent.

## Migration Model

Migration is intentionally two-phase:

1. No-loss import: preserve the original skill body in `sop.md` and copy resources.
2. Semantic refactor: move platform-specific details out of `sop.md` into `agents/<agent>.md` or the local agent skill.

This avoids losing information during the first migration.

## Local Agent Skill Model

SkillPort does not claim to know every agent runtime.

Each agent should generate and validate its own native skill from the shared SOP. The generated local skill must include a source pointer:

```text
SkillPort source SOP: <path-to-user-sop-repo>/skills/<skill-name>/sop.md
```

And a maintenance rule:

```text
When changing this local skill, classify the change.
If it is shared workflow knowledge, update the SkillPort SOP first.
If it is agent-specific behavior, keep it local or in agents/<agent>.md.
```

## Example Policy

Examples may live under `examples/`, such as:

```text
examples/init-harness/skills/init-harness/
```

Examples are not live user skills and should not be presented as validated packages for every agent runtime.
# SkillPort Spec

Status: draft MVP

## Portable Skill

A Portable Skill is a platform-neutral skill source that separates shared workflow knowledge from agent runtime details.

```text
skills/<skill-name>/
  skill.yaml
  sop.md
  adapters/
    codex.md
    claude-code.md
    antigravity.md
  references/
  scripts/
  assets/
  migration.md
```

## Required Files

### `skill.yaml`

Holds portable metadata.

```yaml
name: init-harness
description: Generates a complete Harness scaffold for a project.
version: 0.1.0
targets:
  - codex
  - claude-code
  - antigravity
```

### `sop.md`

Contains agent-neutral workflow knowledge:

- task phases
- decision rules
- required outputs
- validation criteria
- shared references

It should avoid agent-only tool names and runtime paths when possible.

### `adapters/<agent>.md`

Contains agent-specific behavior:

- tool names
- installation or loading notes
- platform-specific runtime files
- agent-specific constraints

### `dist/<agent>/<skill-name>/SKILL.md`

Generated runtime package. It must include a maintenance header that points future edits back to `skills/<skill-name>/`.

## Generation Model

```text
SKILL.md =
  generated maintenance header
  + target skill frontmatter
  + target adapter
  + shared SOP
```

Resource directories such as `references/`, `scripts/`, and `assets/` are copied from source into every generated target package.

## Migration Model

Migration is intentionally two-phase:

1. No-loss import: preserve the original skill body in `sop.md` and copy resources.
2. Semantic refactor: gradually move platform-specific sections from `sop.md` into the correct adapter.

This avoids losing information during the first migration.

## Drift Rule

The source skill is authoritative.

Generated `dist/` packages are disposable and should be rebuilt whenever `skills/<name>/` changes.

# SkillPort

English | [简体中文](README.zh-CN.md)

Write one SOP. Ship native skills to multiple agents.

SkillPort is a reference implementation of the Portable Skill idea: a skill should be maintained as an agent-neutral SOP, then adapted into each agent's native skill format.

## Why

Coding agents are converging on skill-like workflows, but each agent has its own skill format, runtime rules, installation path, and tool vocabulary. Maintaining the same workflow three times causes drift.

SkillPort keeps one source of truth:

```text
skills/<skill-name>/sop.md
```

Then it combines that shared SOP with agent-specific adapters:

```text
skills/<skill-name>/adapters/codex.md
skills/<skill-name>/adapters/claude-code.md
skills/<skill-name>/adapters/antigravity.md
```

And generates native runtime packages:

```text
dist/codex/<skill-name>/SKILL.md
dist/claude-code/<skill-name>/SKILL.md
dist/antigravity/<skill-name>/SKILL.md
```

## Quick Start

```powershell
.\tools\skillport.ps1 migrate -From "C:\Users\Ethan\.agents\skills\init-harness" -Agent codex
.\tools\skillport.ps1 build -Name init-harness
.\tools\skillport.ps1 validate -Name init-harness
```

## Core Idea

```text
MCP standardizes how external tools integrate with different agents.
Portable Skill standardizes how workflows are reused across different agents.
```

MCP solves this problem:

```text
One external tool should not need a separate custom integration for every agent.
```

SkillPort solves the parallel problem at the workflow layer:

```text
One SOP should not need a separate hand-maintained skill for every agent.
```

MCP decouples tools from agent-specific integrations. SkillPort applies the same protocol-oriented, decoupled, portable idea to the Skill / SOP layer.

## Maintenance Rule

Generated skills are runtime packages. Do not edit them directly.

When improving a skill:

- Edit `skills/<name>/sop.md` for shared workflow changes.
- Edit `skills/<name>/adapters/<agent>.md` for agent-only behavior.
- Run `.\tools\skillport.ps1 build -Name <name>`.
- Run `.\tools\skillport.ps1 validate -Name <name>`.

## Commands

```powershell
.\tools\skillport.ps1 doctor
.\tools\skillport.ps1 migrate -From <skill-folder> -Agent <agent>
.\tools\skillport.ps1 build -Name <skill-name>
.\tools\skillport.ps1 validate -Name <skill-name>
```

The MVP intentionally does not install into agent home directories. Installation should be added after the source/build/validate loop is stable.

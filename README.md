# SkillPort

English | [简体中文](README.zh-CN.md)

Write one SOP. Let each agent build its own native skill.

SkillPort is a tool and specification for Portable Skills. It does not host your personal skills in this repository. Instead, it helps users create and maintain their own SkillPort-compatible SOP repositories.

## Repository Boundary

This repository is the SkillPort tool repository. It contains:

```text
README.md
README.zh-CN.md
SPEC.md
LICENSE
tools/
templates/
examples/
```

Your own skills should live in a separate repository, for example:

```text
my-agent-sops/
  skillport.yaml
  skills/
    init-harness/
      skill.yaml
      sop.md
      references/
      maintenance.md
```

## Why

Coding agents are converging on skill-like workflows, but each agent has its own skill format, runtime rules, installation path, and tool vocabulary. Maintaining the same workflow three times causes drift.

SkillPort keeps the shared SOP independent from agent runtimes. Each agent can read the SOP and produce its own native skill in the format it understands.

## Quick Start

Clone this tool repository:

```powershell
git clone https://github.com/weiqiang612/skillport.git D:\project\skillport
```

Create or clone your own SOP repository:

```powershell
git init D:\project\my-agent-sops
cd D:\project\my-agent-sops
```

Use SkillPort from the tool repository:

```powershell
D:\project\skillport\tools\skillport.ps1 migrate -From "C:\Users\Ethan\.agents\skills\init-harness" -Agent codex
```

The MVP still exposes low-level commands. The intended product direction is to make `onboard` and `sync` the only commands most users need.

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

The shared SOP is the source of truth. Local agent skills should include a SkillPort source pointer and maintenance protocol.

When improving a skill:

- Shared workflow, checklist, decision rule, or validation rule: update the SOP in your SOP repository.
- Agent-specific tool name, command, install path, or runtime behavior: update that agent's local skill or agent notes.
- If unsure, classify the change before editing.

## Commands

```powershell
.\tools\skillport.ps1 doctor
.\tools\skillport.ps1 migrate -From <skill-folder> -Agent <agent>
.\tools\skillport.ps1 build -Name <skill-name>
.\tools\skillport.ps1 validate -Name <skill-name>
```

## Example

A migrated `init-harness` example lives in:

```text
examples/init-harness/skills/init-harness/
```

It is example data, not the user's live skill repository.
# TASK-001: Onboard Sync Workflow

**Status**: Draft
**Created**: 2026-06-21
**Feature dir**: `docs/4-tasks/features/TASK-001-onboard-sync-workflow/`

## Objective
让 SkillPort 从底层命令集合升级为用户可理解的工作流工具，使用户能通过 `onboard` 和 `sync` 完成 SOP 仓库初始化、已有 Skill 迁移和维护状态检查。

## Scope

### In scope
- 在 `tools/skillport.ps1` 中新增 `init`、`onboard`、`sync` 命令入口。
- 明确 SkillPort 工具仓库与用户 SOP 仓库的边界，确保用户真实 Skill 只写入用户指定仓库。
- 让 `init` 基于 `templates/skill-repo` 创建用户 SOP 仓库骨架。
- 让 `onboard` 提供交互式入口，引导用户创建 SOP 仓库、迁移已有 Skill，或查看下一步维护说明。
- 让 `sync` 对当前 SOP 仓库做只读状态检查，报告 SOP、maintenance、agent notes 和 git 状态。
- 更新中英文 README 与 SPEC，让主路径只暴露 `onboard`、`sync`、`doctor`，把 `migrate`、`build`、`validate` 降为高级命令。

### Out of scope
- 不实现各 Agent 原生 Skill 的自动真实适配或运行时验证。
- 不把用户真实 `skills/` 放回 SkillPort 工具仓库根目录。
- 不实现跨平台安装器、包管理器发布或 GitHub Action。
- 不强制删除现有高级命令；本任务只调整默认工作流和新增封装命令。

## Acceptance criteria

```json
[
  {
    "id": "AC-001",
    "category": "functional",
    "description": "用户可以用 init 创建独立的 SOP 仓库骨架，而不是把自己的 Skill 写进 SkillPort 工具仓库。",
    "steps": [
      "在临时目录运行 tools/skillport.ps1 init -Path <temp-sop-repo>",
      "检查目标目录包含 skillport.yaml、skills/example-skill/skill.yaml、sop.md、maintenance.md",
      "确认 D:\\project\\skillport 根目录没有新增根级 skills/ 或 dist/ 用户数据"
    ],
    "passes": false
  },
  {
    "id": "AC-002",
    "category": "functional",
    "description": "用户可以用 onboard 通过交互式向导完成初始化或迁移入口选择。",
    "steps": [
      "运行 tools/skillport.ps1 onboard 并选择创建新 SOP 仓库",
      "输入目标路径后确认它调用与 init 等价的创建逻辑",
      "再次运行 onboard 并选择迁移已有 Skill，确认它提示 From 路径和 Agent，并调用 migrate 逻辑"
    ],
    "passes": false
  },
  {
    "id": "AC-003",
    "category": "functional",
    "description": "sync 能在用户 SOP 仓库中做只读状态检查并输出可执行维护建议。",
    "steps": [
      "进入一个包含 skillport.yaml 和 skills/*/sop.md 的用户 SOP 仓库",
      "运行 D:\\project\\skillport\\tools\\skillport.ps1 sync",
      "确认输出列出已发现 Skill、缺失 maintenance.md 的警告、agent notes 状态和 git 工作区状态"
    ],
    "passes": false
  },
  {
    "id": "AC-004",
    "category": "edge-case",
    "description": "命令在错误目录或缺少必要参数时给出清晰错误，不静默写入错误位置。",
    "steps": [
      "在非 SOP 仓库目录运行 sync",
      "确认命令提示当前目录不是 SkillPort SOP 仓库，并说明需要先运行 onboard/init",
      "运行 init 指向已存在且非空目录，确认命令要求显式确认或拒绝覆盖"
    ],
    "passes": false
  },
  {
    "id": "AC-005",
    "category": "integration",
    "description": "README、README.zh-CN 和 SPEC 与新命令模型一致，不再把 dist 生成物描述为默认主路径。",
    "steps": [
      "阅读 README.md 和 README.zh-CN.md 的 Quick Start",
      "确认主路径只要求 onboard、sync、doctor",
      "确认 SPEC 明确 SkillPort 工具仓库不保存用户真实 Skill，用户 SOP 仓库才保存 skills/"
    ],
    "passes": false
  }
]
```

## Notes

### Documentation impact
| Area | Impacted | Maintenance target |
|---|---:|---|
| requirements | true | `README.md`, `README.zh-CN.md` |
| architecture | true | `SPEC.md`, `tools/skillport.ps1` command model |
| api | false | None |
| db | false | None |
| ui | false | None |
| constraints | true | `SPEC.md` repository boundary and maintenance protocol |
| adr | false | None |
| agent-runtime | false | None |

### Approval-sensitive changes
- None. This task changes CLI behavior and documentation only; it does not add new external dependencies or modify agent home directories.

### Explicit non-maintenance
- `AGENTS.md` does not exist in this repository and is not required for this task.
- `docs/1-requirements/` and `docs/2-designs/` do not exist yet; this task uses README/SPEC as the current long-lived project documents instead of creating a full Harness scaffold.
- `dist/` remains outside the default repository model; this task should not reintroduce root-level generated runtime packages.
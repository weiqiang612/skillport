# TASK-001: Tasks

**Spec**: `spec.md`
**Status**: In Progress

## Key decisions
- SkillPort 工具仓库只保存工具、规范、模板和示例；用户真实 SOP 必须进入独立用户仓库。
- `onboard` 和 `sync` 是默认用户心智模型；`migrate`、`build`、`validate` 保留为高级命令。
- `sync` 的 MVP 只做状态检查和维护建议，不自动修改本地 Agent Skill，也不声称完成 Agent 适配。

## Progress

- [ ] T1 — 更新 `README.md` 和 `README.zh-CN.md` 的 Quick Start，把默认路径改为 `onboard`、`sync`、`doctor`，并说明高级命令只用于调试和脚本化场景 · covers: doc-maintenance, AC-005
- [ ] T2 — 更新 `SPEC.md`，补充用户 SOP 仓库生命周期、`onboard`/`sync` 语义、错误目录保护和根级 `skills/` 禁止规则 · covers: doc-maintenance, AC-004, AC-005
- [ ] T3 — 在 `tools/skillport.ps1` 参数枚举中加入 `init`、`onboard`、`sync` 命令，并保持现有 `doctor`、`migrate`、`build`、`validate` 可用 · covers: AC-001, AC-002, AC-003
- [ ] T4 — 实现 `Invoke-Init`：复制 `templates/skill-repo` 到目标路径，创建用户 SOP 仓库骨架，并拒绝无确认覆盖非空目录 · covers: AC-001, AC-004
- [ ] T5 — 实现 `Invoke-Onboard`：提供交互式菜单，支持创建新 SOP 仓库和迁移已有 Skill 两条路径，并复用 `Invoke-Init` / `Invoke-Migrate` · covers: AC-002, AC-004
- [ ] T6 — 实现 `Invoke-Sync`：检查当前目录是否有 `skillport.yaml`，扫描 `skills/*/sop.md`、`maintenance.md`、`agents/`，输出只读状态报告和下一步建议 · covers: AC-003, AC-004
- [ ] T7 — 调整 `Invoke-Migrate` 的输出文案，明确迁移目标是当前用户 SOP 仓库，不是 SkillPort 工具仓库；迁移后提示运行 `sync` · covers: AC-001, AC-002, AC-005
- [ ] T8 — 用临时目录手动验证 `init`：确认生成 `skillport.yaml`、`skills/example-skill/skill.yaml`、`sop.md`、`maintenance.md`，且工具仓库根目录不新增用户数据 · covers: AC-001
- [ ] T9 — 用临时目录手动验证 `onboard`：覆盖创建新 SOP 仓库和迁移已有 Skill 两条交互路径 · covers: AC-002
- [ ] T10 — 用临时目录手动验证 `sync`：覆盖正常 SOP 仓库、缺失 `maintenance.md`、非 SOP 仓库三个场景 · covers: AC-003, AC-004
- [ ] T11 — 运行 PowerShell 解析检查：`[scriptblock]::Create((Get-Content .\tools\skillport.ps1 -Raw))` 必须成功 · covers: AC-001, AC-002, AC-003, AC-004
- [ ] T12 — Verify ACs: update `passes` to `true` in spec.md for each passing criterion · covers: AC-001, AC-002, AC-003, AC-004, AC-005
- [ ] T13 — Update `docs/4-tasks/CURRENT_PLAN.md` after implementation — mark this task complete · covers: doc-maintenance

## Dependencies
- T1 and T2 should be completed before CLI behavior changes so implementation follows the documented model.
- T5 depends on T4 and existing migrate behavior.
- T6 depends on the user SOP repository shape documented in T2.
- Verification tasks T8-T11 depend on T3-T7.
- AC verification and `CURRENT_PLAN.md` update require all implementation and verification tasks to be complete.

## Blockers
- None.
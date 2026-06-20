# SkillPort

[English](README.md) | 简体中文

一次维护 SOP，多 Agent 复用 Skill。

SkillPort 是 Portable Skill 理念的一个参考实现：Skill 不应该只绑定在某个 Agent 的本地文件格式里，而应该先作为平台无关的 SOP 被维护，再适配成不同 Agent 可识别的原生 Skill。

## 为什么需要 SkillPort

Codex、Claude Code、Antigravity 等 Agent 都在支持类似 Skill 的工作流能力，但它们的 Skill 格式、运行规则、安装目录、工具名和上下文约束并不完全相同。

如果同一套工作流要分别维护三份 Skill，就很容易出现：

- 重复修改
- 规则漂移
- 某个 Agent 的版本落后
- 平台专属指令混进通用 SOP

SkillPort 的目标是保留一份公共源：

```text
skills/<skill-name>/sop.md
```

再通过各 Agent 的 adapter 补充平台差异：

```text
skills/<skill-name>/adapters/codex.md
skills/<skill-name>/adapters/claude-code.md
skills/<skill-name>/adapters/antigravity.md
```

最后生成各 Agent 的原生运行包：

```text
dist/codex/<skill-name>/SKILL.md
dist/claude-code/<skill-name>/SKILL.md
dist/antigravity/<skill-name>/SKILL.md
```

## 快速开始

```powershell
.\tools\skillport.ps1 migrate -From "C:\Users\Ethan\.agents\skills\init-harness" -Agent codex
.\tools\skillport.ps1 build -Name init-harness
.\tools\skillport.ps1 validate -Name init-harness
```

## 核心观念

```text
MCP 标准化外部工具如何接入不同 Agent。
Portable Skill 标准化工作流如何复用到不同 Agent。
```

MCP 解决的是：

```text
一个外部工具，不应该为了接入不同 Agent 而重复做多套专属适配。
```

SkillPort 解决的是 Skill / SOP 层的对应问题：

```text
一套工作流 SOP，不应该为了给不同 Agent 使用而手工维护多份 Skill。
```

MCP 把外部工具从具体 Agent 的接入方式里解耦出来。SkillPort 则把类似的“协议化、解耦、可移植”思想放到 Skill / SOP 层。

## 维护规则

生成出来的 Skill 是 runtime package，不应该手工修改。

如果要改进 Skill：

- 通用流程改 `skills/<name>/sop.md`
- 某个 Agent 专属行为改 `skills/<name>/adapters/<agent>.md`
- 修改后运行 `build`
- 再运行 `validate`

```powershell
.\tools\skillport.ps1 build -Name <name>
.\tools\skillport.ps1 validate -Name <name>
```

## 命令

```powershell
.\tools\skillport.ps1 doctor
.\tools\skillport.ps1 migrate -From <skill-folder> -Agent <agent>
.\tools\skillport.ps1 build -Name <skill-name>
.\tools\skillport.ps1 validate -Name <skill-name>
```

当前 MVP 暂时不直接安装到各 Agent 的 home skill 目录。建议先稳定 `source -> build -> validate` 这条链路，再加入 `install` 和 `diff`。
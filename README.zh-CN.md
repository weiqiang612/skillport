# SkillPort

[English](README.md) | 简体中文

一次维护 SOP，让每个 Agent 生成自己的原生 Skill。

SkillPort 是 Portable Skill 的工具和规范仓库。这个仓库不应该存放用户自己的真实 Skill，而是提供工具、规范、模板和示例，帮助用户创建并维护自己的 SkillPort SOP 仓库。

## 仓库边界

当前仓库是 SkillPort 工具仓库，只应该包含：

```text
README.md
README.zh-CN.md
SPEC.md
LICENSE
tools/
templates/
examples/
```

用户自己的 Skill 应该放在独立仓库里，例如：

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

## 为什么需要 SkillPort

Codex、Claude Code、Antigravity 等 Agent 都在支持类似 Skill 的工作流能力，但它们的 Skill 格式、运行规则、安装目录、工具名和上下文约束并不完全相同。

如果同一套工作流要分别维护三份 Skill，就很容易出现重复修改、规则漂移、版本落后，以及平台专属指令污染公共流程。

SkillPort 把公共 SOP 从具体 Agent 的运行时里解耦出来。每个 Agent 可以读取 SOP，再生成它自己能理解的原生 Skill。

## 快速开始

先克隆 SkillPort 工具仓库：

```powershell
git clone https://github.com/weiqiang612/skillport.git D:\project\skillport
```

再创建或克隆你自己的 SOP 仓库：

```powershell
git init D:\project\my-agent-sops
cd D:\project\my-agent-sops
```

从工具仓库调用 SkillPort：

```powershell
D:\project\skillport\tools\skillport.ps1 migrate -From "C:\Users\Ethan\.agents\skills\init-harness" -Agent codex
```

当前 MVP 仍然暴露了一些底层命令。最终产品方向是让大多数用户只需要记住 `onboard` 和 `sync`。

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

公共 SOP 是事实源。每个本地 Agent Skill 都应该带有 SkillPort source 指针和维护协议。

如果要改进 Skill：

- 通用流程、检查清单、决策规则、验证规则：改你自己的 SOP 仓库。
- Agent 专属工具名、命令、安装路径、运行时行为：改该 Agent 的本地 Skill 或 agent notes。
- 不确定时，先做变更分类，再决定改哪里。

## 命令

```powershell
.\tools\skillport.ps1 doctor
.\tools\skillport.ps1 migrate -From <skill-folder> -Agent <agent>
.\tools\skillport.ps1 build -Name <skill-name>
.\tools\skillport.ps1 validate -Name <skill-name>
```

## 示例

迁移后的 `init-harness` 示例放在：

```text
examples/init-harness/skills/init-harness/
```

这是示例数据，不是用户真实的 Skill 仓库。
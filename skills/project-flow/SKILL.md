---
name: project-flow
description: >-
  【Skill·Project Flow】Project lifecycle conductor: detect the project's current stage (idea / planning / development / live / outsourcing) and route automatically to the right tool — grilling for requirement interview, feature-spec for feature list, system-audit for inspection.
  Works at any stage: new idea, new feature request, check progress, organize requirements, pre-launch checklist, vendor acceptance, operational health check.
  Say one sentence — "what's next for X", "X has a new feature request", "organize X" — no need to know which tool to use.
  Triggers: "what's next", "new project", "new feature", "check progress", "organize this project", "acceptance", "health check".
  Do not trigger for unrelated chat, or one-off code edits (just code directly).
---

# Project Flow · 项目流程指挥

一句话进入项目生命周期：编排器探测进度、路由到对应 skill、产物自动衔接。**不预设「新项目」——任何进度都能进**，你只需要说「哪个项目 + 想干什么」。

## 第一步：确认需求（一次问完两件事）

1. **哪个项目**（名字/路径）
2. **想干什么**：新想法 / 新需求 / 查进度 / 梳理项目 / 上线前 / 外包验收 / 运营体检

## 第二步：探测项目状态（只读，不猜）

| 探测项 | 检查 |
|---|---|
| 规划基线 | `<项目>/docs/feature-spec.md` 是否存在 |
| 代码 | 是否有 src/、app/、package.json、main.py 等 |
| 审查档案 | `~/.claude/skill-records/system-audit/<项目>/` 是否有历史评估 |

## 第三步：按状态路由

| 你说 | 探测到的状态 | 路由动作 |
|---|---|---|
| 新想法 | 无 spec、无代码 | 调 **grilling** 拷问 → 共识后调 **feature-spec** 出清单 |
| 新需求 | 有 spec | 可选拷问（grilling）→ **feature-spec** 更新（新功能补进清单）→ 提示可开发 |
| 新需求 | 无 spec、有代码 | 先 **feature-spec** 整理基线（来源③：现有文档）→ 补新需求 |
| 查进度/体检 | 有 spec、有代码 | 调 **system-audit**（core 快速自查 / full 深度体检，给推荐由用户定） |
| 查进度 | 无 spec、有代码 | 提示：先 feature-spec 整理基线完成度才准；或直接审查（完成度标「无法评估」） |
| 梳理项目 | 有代码 | **feature-spec** 从文档/代码生成基线 |
| 上线前 | 开发完 | **system-audit** pre-launch 版（❌ 清零才能上） |
| 外包验收 | 技术方交付 | **system-audit** outsource-acceptance 版（防超范围开发） |
| 运营体检 | 已上线 | **system-audit** operation 版（季度体检） |

状态与你说的话冲突时（如说「查进度」但 spec 都没有）→ 按探测结果给推荐路径，让用户定。

## 第四步：执行与衔接

- 用 Skill 工具调对应 skill（技能名：`grilling` / `feature-spec` / `system-audit`），args 传项目路径与上下文
- 产物自动衔接：拷问共识（对话中）→ feature-spec 落盘 `docs/feature-spec.md` → system-audit 读取 spec 做完成度对照
- 一次可串多步（如新项目 = 拷问 + 出清单；新需求 = 拷问 + 更新 spec），**每步完成后问用户是否继续下一步**，不一口气全跑

## 语言

- **输出语言跟随用户提问语言**：用户用中文提问则中文输出，英文提问则英文输出，无需任何配置
- 公开版 description 为英文；本仓库不携带任何语言配置文件
- 外部依赖 `grilling`（拷问引擎）的输出语言同样跟随用户提问（非本项目资产）

## 红线

- 探测只读，不动项目任何文件
- 不替用户做阶段决策：拷问与否、审查哪个版本、先建基线还是直接审——给推荐，由用户定
- 本 skill 不含任何业务逻辑，逻辑都在三个底层 skill 里；底层 skill 可随时单独调用

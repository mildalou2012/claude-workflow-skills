---
name: feature-spec
description: >-
  【Skill·Feature Spec】Turn requirement consensus into a structured, testable feature spec: system positioning (what/where it runs), feature module tree, and per-feature description / prerequisites / output / effect / test method / expected result (acceptance criteria).
  Invoke after a grill-me/grilling requirement interview, or generate from existing project docs.
  The spec is the planning baseline for system-audit: it reads docs/feature-spec.md to verify feature completion against acceptance criteria.
  Use when you want to organize features, write a feature plan, define acceptance criteria, or hand a spec to a developer.
  Triggers: "feature spec", "feature plan", "organize the requirements", "acceptance criteria", "how do we test this".
  Do not trigger for pure chat, or auditing a system (use system-audit).
---

# Feature Spec · 功能清单

把需求共识整理成**可验收的结构化功能清单**。它是项目规划基线（存项目内 `docs/feature-spec.md`），下游由 system-audit 消费：系统类型声明直接决定审查的适用项，每功能的「测试方法 + 正常结果」决定完成度能否判 ✅。

## 与上下游的关系

```
grill-me/grilling（拷问需求，共识在对话中，不落盘）
  → feature-spec（本技能：重建共识 → 生成清单 → 落盘项目内 docs/feature-spec.md）
    → system-audit（审查：读 feature-spec 做完成度对照）
```

## 输入来源（三选一）

1. **grilling 会话共识**（默认路径）：刚拷问完、共识在对话中
2. **用户直接给方向**：需求还没拷问过也可以直接整理（建议后续再 grill-me 检验）
3. **现有项目文档**：从 README/项目现状.md/CLAUDE.md 提炼成规范清单（半亩题田这种已有文档的项目）

## 流程（严格执行）

1. **确认来源**：问用户是刚拷问完、直接给方向、还是从现有文档整理。系统名与项目路径一并确认。
2. **重建共识（仅 grilling 来源）**：回放会话，提取全部已决决策（每个问题的标题/选项/最终选择/被否决的选项），输出「决策清单」请用户确认——grilling 原则：**没有东西被默默假设，不确认不整理**。
3. **生成功能清单**：按 `templates/feature-spec.md` 模板逐模块整理。生成纪律：
   - 功能粒度：一个功能 = 用户能感知的一个完整动作（如「上传题库并自动解析」），不要拆到 UI 元素级，也不要合并成模块级
   - 测试方法与正常结果必须可判定通过/不通过，写不出验收标准的功能要么补定义、要么标「未定」进第 4 节
   - 明确排除项必须写（不做的事），防扯皮
4. **用户确认**：清单生成后给用户过目，确认无误才落盘；有异议按用户意见修改。
5. **落盘**：`<项目根>/docs/feature-spec.md`（跟随项目 git；无 docs/ 目录则创建）。落盘后告知：可用 /system-audit 直接审查了。
6. **迭代更新**：功能清单是活文档——功能新增/变更/取消时更新它，同步改「版本」并记录变更（底部加变更记录行）。审查发现「规划外新增功能」时，回来把该功能补进清单（标 ⭐ 或并入正式规划）。

## 模板结构（要素见 templates/feature-spec.md）

| 节 | 内容 | 下游用途 |
|---|---|---|
| 1. 系统定位声明 | 一句话定位/系统类型/前台后台/运行硬件/用户规模/技术栈/排除项 | system-audit 读它定 ➖ 项，不用评估者猜 |
| 2. 功能模块总览 | 模块名+职责+优先级 P0/P1/P2 | 完成度对照的分组 |
| 3. 模块详情 | 每功能：描述/前置/输出/实现效果/测试方法/正常结果/优先级 | 完成度双重判定（实现+验收达标） |
| 4. 未定事项与风险 | 写不出验收标准的功能、依赖未定的事 | 审查时标 ⚠️ 的来源 |

## 语言

- **输出语言跟随用户提问语言**：用户用中文提问则中文输出，英文提问则英文输出，无需任何配置
- 公开版 description 为英文；本仓库不携带任何语言配置文件

## 红线

- 不确认不落盘；用户确认前不写任何文件
- 不编造验收标准——写不出的标「未定」进第 4 节
- 不做需求决策：功能取舍、优先级是用户的决策，清单只负责整理清楚
- 项目红线（如外包项目的范围边界）在清单「排除项」中如实反映

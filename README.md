# Claude 产品开发工作流 Skill 套件

给 Claude Code 用的产品开发工作流三件套 + 编排器：**拷问需求 → 出功能清单（含验收标准）→ 审查系统（双分数报告）**。

面向产品经理/独立开发者：非程序员也能用大白话驱动 AI 完成「想法 → 可验收的规划 → 可量化的体检」整个闭环。

## 包含什么（本项目自研，MIT）

| Skill | 作用 | 产物 |
|---|---|---|
| `project-flow` | 项目生命周期指挥器（编排层） | 探测项目进度，路由到下方工具 |
| `feature-spec` | 功能清单生成器 | `docs/feature-spec.md`：系统定位/功能模块/每个功能的前置·输出·实现效果·测试方法·验收标准 |
| `system-audit` | 系统审查器 | 双分数报告（健康分 + 完成度），14 类 76 项审查，5 个阶段版本，存档留痕可跨次对比 |

### 不包含什么（外部依赖声明）

以下 Skill **不是本项目资产**，本项目不含其代码，但 `project-flow` 会调用它们（Claude Code 的 Skill 机制，调用本地已安装的版本）：

| 外部 Skill | 作用 | 来源 |
|---|---|---|
| `grilling` / `grill-me` | 需求拷问引擎（访谈式追问） | 第三方开源，随 Claude Code 社区分发安装；本仓库未携带，原仓库链接暂无法追溯 |
| `neat-freak` | 知识收尾整理 | 第三方开源，同上 |

> 如果你在别处看到本项目声称包含上述三个 skill，那是不实信息——**它们不是我们做的，我们只做了整合调用**。

## 安装

把 `skills/` 下的目录复制到 Claude Code 的 skills 目录：

```bash
# macOS / Linux
cp -r skills/* ~/.claude/skills/

# Windows
# 将 skills/ 下三个文件夹复制到 C:\Users\<你的用户名>\.claude\skills\
```

依赖：需已安装 `grilling`（拷问）——未安装时 `project-flow` 的拷问步骤不可用，其余功能不受影响。

## 使用（说人话即可）

| 你想干什么 | 对 AI 说 |
|---|---|
| 有个想法想捋清楚 | 「帮我拷问一下 XX 的想法」 |
| 方向定了要出功能规划 | 「出个功能清单」 |
| 系统做完了想查进度 | 「审查一下 XX」或「XX 查进度」 |
| 不知道下一步做什么 | 「XX 接下来做什么」（触发 project-flow 自动路由） |

### 标准流程

```
grill-me/grilling（拷问需求）→ feature-spec（生成功能清单）→ 开发 → system-audit（审查体检）
```

一次完整项目生命周期：

1. **拷问**：「拷问一下 XX 的想法」→ 像产品经理一样层层追问，直到方向清楚
2. **功能清单**：「出个功能清单」→ 生成带验收标准的结构化清单（系统定位声明 / 功能模块 / 每功能：前置·输出·实现效果·测试方法·正常结果），落盘 `docs/feature-spec.md`
3. **审查**：「审查一下 XX」→ 14 类 76 项逐项评估 + 功能完成度对照（实现 + 验收达标双重判定），输出「健康分 / 完成度」双分数报告，自动存档，下次审查自动对比

### system-audit 的 5 个阶段版本

| 版本 | 用途 |
|---|---|
| `full` 全量 | 新项目第一次体检、深度体检 |
| `core` 核心 | 任何项目快速自查（10 分钟） |
| `pre-launch` 上线前 | 上线前核查，❌ 必须清零 |
| `operation` 运营期 | 已上线系统季度体检 |
| `outsource-acceptance` 外包验收 | 交付验收（含超范围开发清单） |

## 设计原则

- **证据纪律**：审查每项必须附证据（文件/命令/截图），拿不出证据标 ⚠️「未找到」，禁止凭感觉填 ✅
- **验收标准不满足不得判 ✅**：完成度 = 实现 + 验收达标双重判定
- **不编造**：无规划基线时完成度标「无法评估」，提示先建功能清单
- **留痕**：评估存档 `~/.claude/skill-records/system-audit/<系统>/`，形成成熟度曲线

## 目录结构

```
claude-workflow-skills/
├── README.md
├── LICENSE
└── skills/
    ├── project-flow/          # 编排层：SKILL.md + CHANGELOG.md
    ├── feature-spec/          # 功能清单：SKILL.md + templates/feature-spec.md
    └── system-audit/          # 系统审查：SKILL.md + templates/（full/core/pre-launch/operation/outsource-acceptance）
```

## 许可证

MIT License —— 欢迎使用、修改、提意见（Issues / PR）。

## 反馈

这套 Skill 在真实项目验证过（跨境电商 ERP、题库刷题工具），欢迎提意见：审查项覆盖、评分口径、模板结构、使用体验。

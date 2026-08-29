# Are You Sure?

[English](./README.md) | 简体中文

**在采用工程方案之前，先审查它是否合理。**

## 问题所在

编程 Agent 往往会在给出方案后直接继续执行。它可能依赖未经验证的假设，对简单问题过度设计，或引入与目标无关的修改。在实施方案之前，通常缺少一次独立的第二轮审查。

## 三个维度

这个 skill 从三个方面审查方案：

| 维度 | 核心问题 |
| --- | --- |
| **Effectiveness** | 方案能否在既定约束下实现目标？ |
| **Simplicity** | 是否存在更简单且同样可靠的方案？当前增加的复杂度是否必要？ |
| **Consequences** | 采用方案会引入哪些新的成本或风险（例如回归风险、维护成本和生命周期成本）？ |

它不会默认原方案正确，而是主动寻找反例和更简单的替代方案，并基于证据给出结论。若方案经得住审查，结论也可以是 **Retain**。

## 安装

**选项 A：让 Agent 安装（推荐）**

告诉你的编程 Agent：

```text
Install the are-you-sure skill from https://github.com/nscTechArt/are-you-sure
```

**选项 B：npx**

```bash
npx skills add nscTechArt/are-you-sure
```

**选项 C：手动复制**

克隆或下载本仓库，再复制到 Agent 的 skills 目录。目录位置取决于所使用的工具，例如：`~/.agents/skills/are-you-sure`、`~/.claude/skills/are-you-sure` 或 `.cursor/skills/are-you-sure`。入口文件为 `SKILL.md`。

安装后，请新建一个 Agent 任务，以重新加载 skill 目录。

## 使用

### 示例 Prompt

```text
Use $are-you-sure to challenge your most recent technical recommendation.
```

```text
Before we implement that plan, run are-you-sure on it.
```

### 何时使用

- Agent 已经给出一个**具体方案**，而不是仍在进行开放式探索
- 工作范围属于**软件工程或技术项目**
- 希望在采纳或实施方案**之前**进行一次独立审查

### 不适用的情况

- 尚无具体方案，只有探索性讨论或尚未选定的备选方案
- 问题不属于软件工程或技术项目
- 只需要执行已有方案，不需要重新评估
- 需要 Agent 在**同一轮**直接修改代码（本 skill 设计为只读）

### 你会得到什么

skill 会返回结构化的评审结果：

1. **Decision** — 六选一：Retain、Simplify、Modify、Replace、Reject、Defer
2. **Effectiveness / Simplicity / Consequences** — 每个维度的判断及其关键证据
3. **Revised recommendation** — 建议采用的方案，但不执行修改

## 如何判断它在起作用

- 返回明确的 **Decision**；原方案合理时可以是 Retain
- 每项判断都有**证据**支持，而不是基于直觉
- 发现问题时会明确给出 **Simplify / Modify / Replace / Reject** 等建议，而不是默认接受原方案
- 只给出建议，不直接修改代码

## License

[MIT](./LICENSE)

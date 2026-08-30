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

## 与相关项目的区别

`Are You Sure?` 与一些减少 Agent 过度设计的项目有交集，但介入的问题层级不同。

| 项目 | 核心问题 | 主要介入阶段 | 主要产物 |
| --- | --- | --- | --- |
| **Are You Sure?** | **这个方案是否值得采用？** | 已有具体方案之后、实施之前 | 基于证据的 Decision 与修订建议 |
| [**Ponytail**](https://github.com/DietrichGebert/ponytail) | **既然要实现，最小正确实现是什么？** | 理解任务与编码执行过程中 | 更小的实现、diff 与依赖面 |
| [**Karpathy Guidelines**](https://github.com/multica-ai/andrej-karpathy-skills) | **Agent 在编码过程中应遵循哪些行为原则？** | 编码、修改、验证的全过程 | 更谨慎、简洁、精准且可验证的执行行为 |

### 与 Ponytail 的区别

Ponytail 的核心是 **implementation minimization**：理解问题后，优先复用现有能力、标准库、原生平台能力和已安装依赖，最后才编写最少的自定义代码。它也会用 YAGNI 质疑不必要的需求或抽象，但主要目标仍是让**实现本身更小、更直接**。

`Are You Sure?` 的核心是 **solution evaluation**：它不负责寻找最短代码路径，而是在采用某个 Candidate solution 之前，独立判断这个方案是否有效、复杂度是否有必要，以及采用后会引入什么后果。较复杂的方案如果有证据证明其必要性，可以得到 **Retain**。

可以简化为：

> **Ponytail 问：这个方案怎么最简单地做？**  
> **Are You Sure? 问：这个方案到底该不该做？**

两者的 `Simplicity` 判断会有重叠，但用途不同：Ponytail 用更低复杂度的选项来**指导实现**；`Are You Sure?` 用它们作为 baseline 来**挑战方案**。

### 与 Karpathy Guidelines 的区别

Karpathy Guidelines 是一组贯穿编码过程的**行为准则**，主要包括：编码前明确假设与歧义、简洁优先、精准修改，以及用可验证的成功标准驱动执行。它的目标是减少常见的 LLM 编码失误，并改善 Agent 从理解任务到验证结果的整体行为。

`Are You Sure?` 则是一个更窄、更明确的**独立二次评审步骤**。它要求先有一个完整、具体的 Candidate solution，再将其作为待证伪的假设，从 Effectiveness、Simplicity 和 Consequences 三个维度进行证据驱动的审查，并最终给出明确的 **Retain / Simplify / Modify / Replace / Reject / Defer** 决策。

可以简化为：

> **Karpathy Guidelines 问：编码 Agent 应该怎样工作？**  
> **Are You Sure? 问：这个具体方案是否应该被采纳？**

因此二者是互补关系：Karpathy Guidelines 可以改善提出方案和执行方案时的行为；`Are You Sure?` 则专门在**方案与实施之间增加一次 dissenting second pass**。

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

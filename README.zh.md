# Are You Sure?

[English](./README.md) | 简体中文

**在采用工程方案之前，先审查它是否值得采用。**

## 问题所在

编程 Agent 往往会从一个具体建议直接进入实现。这个建议可能依赖未经验证的假设，增加不必要的复杂度，或引入从未与更简单 baseline 比较过的生命周期成本。`Are You Sure?` 在方案被采用之前加入一次独立、反向的第二轮审查。

## 三个维度

| 维度 | 核心问题 |
| --- | --- |
| **Effectiveness** | 方案能否在既定硬约束下真正实现目标？ |
| **Simplicity** | 相比最强的可信低复杂度 baseline，新增复杂度是否必要？ |
| **Consequences** | 采用方案会引入哪些实质性的新负担或风险？ |

这个 skill 会把 Candidate solution 当作待证伪的假设，收集最少但足以改变结论的证据，并返回一个基于证据的 Decision：**Retain / Simplify / Modify / Replace / Reject / Defer**。

## 审查行为

这个 review 会优先完成能够自行完成的工作，再考虑是否需要向用户提问。

- 会根据对话推断真正被选中的 Candidate solution，不会仅仅因为出现过多个备选方案就要求用户重新选择。
- 会先检查可用的代码、调用点、测试、配置和文档，再判断信息缺口是否真的阻塞结论。
- 只有当剩余不确定性的不同答案可能改变 Decision 时才提问。
- 使用与方案风险匹配的最小验证范围；安全、本地、只读的检查不需要额外授权。
- Challenge 默认保持 semantic read-only。如果用户明确要求“先 review，再实现”，会先完成审查并给出 Decision，然后再把实现作为独立阶段继续执行。
- 用户的明确指令优先于 skill 内的指南。

## 与相关项目的区别

`Are You Sure?` 与一些减少 Agent 过度设计的项目有交集，但介入的问题层级不同。

| 项目 | 核心问题 | 主要介入阶段 | 主要产物 |
| --- | --- | --- | --- |
| **Are You Sure?** | **这个方案是否值得采用？** | 已有具体方案之后、实施之前 | 基于证据的 Decision 与修订建议 |
| [**Ponytail**](https://github.com/DietrichGebert/ponytail) | **既然要实现，最小正确实现是什么？** | 理解任务与编码执行过程中 | 更小的实现、diff 与依赖面 |
| [**Karpathy Guidelines**](https://github.com/multica-ai/andrej-karpathy-skills) | **Agent 在编码过程中应遵循哪些行为原则？** | 编码、修改、验证的全过程 | 更谨慎、简洁、精准且可验证的执行行为 |

### 与 Ponytail 的区别

Ponytail 的核心是 **implementation minimization**：优先复用现有能力、标准库、原生平台能力和已安装依赖，再编写最少的自定义代码。

`Are You Sure?` 的核心是 **solution evaluation**：它判断一个具体 Candidate solution 是否有效、复杂度是否有必要，以及采用后会带来什么后果。较复杂的方案如果有证据证明其必要性，仍然可以得到 **Retain**。

> **Ponytail 问：这个方案怎么最简单地实现？**  
> **Are You Sure? 问：这个方案到底该不该采用？**

### 与 Karpathy Guidelines 的区别

Karpathy Guidelines 是贯穿整个编码过程的行为准则。`Are You Sure?` 更窄：它要求先有一个具体方案，再执行一次独立的 second-pass review。

> **Karpathy Guidelines 问：编码 Agent 应该怎样工作？**  
> **Are You Sure? 问：这个具体方案是否应该被采纳？**

两者可以互补：前者约束执行行为，后者在采用方案之前独立挑战方案本身。

## 安装

**选项 A：让 Agent 安装（推荐）**

```text
Install the are-you-sure skill from https://github.com/nscTechArt/are-you-sure
```

**选项 B：npx**

```bash
npx skills add nscTechArt/are-you-sure
```

**选项 C：手动复制**

克隆或下载本仓库，再复制到 Agent 的 skills 目录，例如 `~/.agents/skills/are-you-sure`、`~/.claude/skills/are-you-sure` 或 `.cursor/skills/are-you-sure`。入口文件为 `SKILL.md`。

安装后，请新建一个 Agent 任务，让 skill 目录重新加载。

## 使用

### 示例 Prompt

```text
Use $are-you-sure to challenge your most recent technical recommendation.
```

```text
Before we implement that plan, run are-you-sure on it.
```

```text
Run are-you-sure on that plan, then implement the revised recommendation.
```

### 何时使用

- Agent 已经给出一个声称能实现目标的具体方案。
- 工作属于软件工程或技术项目。
- 希望在采用或实施之前进行一次独立挑战。

### 不适用的情况

- 还没有具体方案，只希望进行开放式探索。
- 工作不属于软件工程或技术项目。
- 只希望执行，不希望重新审查方案本身。

### 你会得到什么

1. **Decision** — Retain、Simplify、Modify、Replace、Reject 或 Defer。
2. **Effectiveness / Simplicity / Consequences** — 与关键证据绑定的简洁结论。
3. **Revised recommendation** — Challenge 之后仍然成立的完整建议。

**Defer** 只用于这种情况：在已经耗尽可用证据后，仍有一个明确的不确定性可能真正改变结论。普通歧义不应自动导致 Defer。

## 如何判断它在起作用

- Candidate 已经足够明确时，不会产生不必要的澄清问题。
- 所有会影响 Decision 的判断都有项目证据支持，或被明确标记为 uncertainty。
- 会比较可信的低复杂度 baseline，而不是为了完成模板凭空制造替代方案。
- 原方案合理时可以返回 **Retain**；有证据支持时会给出 **Simplify / Modify / Replace / Reject**。
- 验证范围与决策风险成比例，不会自动扩张测试范围。
- 默认输出简洁，同一个 finding 不会在多个维度重复展开。

行为回归用例见 [`evals/behavior.md`](./evals/behavior.md)。

## License

[MIT](./LICENSE)

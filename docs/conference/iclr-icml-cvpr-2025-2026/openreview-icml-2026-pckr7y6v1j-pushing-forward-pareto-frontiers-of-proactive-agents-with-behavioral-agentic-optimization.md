---
title: Pushing Forward Pareto Frontiers of Proactive Agents with Behavioral Agentic Optimization
title_zh: 通过行为代理优化推进主动代理的帕累托前沿
authors: "Yihang Yao, Zhepeng Cen, Haohong Lin, Shiqi Liu, Zuxin Liu, Jiacheng Zhu, Zhang-Wei Hong, Laixi Shi, Ding Zhao"
date: 2026-04-30
pdf: "https://openreview.net/pdf/4506e391a22a80f0569107c146857e770847b090.pdf"
tags: ["query:rl-mm-llm-ag"]
score: 9.0
evidence: 使用代理强化学习训练主动式大语言模型代理
tldr: 主动式大语言模型代理需要在任务完成和用户互动中取得平衡。现有方法难以兼顾任务表现和用户满意度。本文提出行为代理优化框架BAO，通过代理强化学习在多轮交互中优化交互策略，推动帕累托前沿。实验表明BAO显著提升主动代理的实用性和用户参与度，解决了过度依赖人类反馈的问题。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 主动代理训练面临任务完成与用户参与度的权衡挑战。
method: 提出BAO框架，通过代理强化学习优化主动代理的交互策略。
result: BAO在多个指标上平衡了任务性能和用户满意度，推进帕累托前沿。
conclusion: BAO为构建高效、用户友好的主动代理提供了新的强化学习范式。
---

## Abstract
Proactive large language model (LLM) agents aim to actively plan, query, and interact over multiple turns, enabling efficient task completion beyond passive instruction following and making them essential for real-world, user-centric applications. Agentic reinforcement learning (RL) has recently emerged as a promising solution for training such agents in multi-turn settings, allowing interaction strategies to be learned from feedback. However, existing pipelines face a critical challenge in balancing task performance with user engagement, as passive agents can not efficiently adapt to users' intentions while overuse of human feedback reduces their satisfaction. To address this trade-off, we propose BAO, an agentic RL framework that combines behavior enhancement to enrich proactive reasoning and information-gathering capabilities with behavior regularization to suppress inefficient or redundant interactions and align agent behavior with user expectations. We evaluate BAO on multiple tasks from the UserRL benchmark suite, and demonstrate that it substantially outperforms RL baselines under controlled comparisons, while achieving comparable or even superior performance to frontier LLM agents, highlighting its effectiveness for training proactive, user-aligned LLM agents in complex multi-turn scenarios.

---

## 论文详细总结（自动生成）

由于提供的论文提取文本仅包含验证页面提示，未包含论文正文内容，以下总结基于论文标题、摘要及元数据信息进行。

---

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究问题**：训练“主动式”大语言模型（LLM）代理时，存在**任务完成性能**与**用户参与度/满意度**之间的根本权衡。过于被动无法理解用户意图，过于主动（频繁要求人类反馈）会降低用户满意度。
- **整体含义**：本文旨在突破这一困境，提出一种新的代理强化学习范式，使主动代理能够在不牺牲用户体验的前提下，显著提升任务完成效率，从而推动实际应用中主动代理的“帕累托前沿”。

## 2. 论文提出的方法论
### 核心思想
- **行为代理优化（BAO）**：一个代理强化学习框架，通过双重机制平衡主动交互的收益与成本。
- **关键技术组件**：
  - **行为增强**：丰富代理的主动推理与信息收集能力，使其能更智能地发起交互。
  - **行为正则化**：抑制低效、冗余的互动，约束代理行为更符合用户预期。
### 技术细节（基于摘要推测）
- 框架在多轮交互中运用强化学习，直接从反馈中学习交互策略；行为正则化可能通过惩罚不必要的查询或偏离用户偏好的动作来实现，从而确保代理不“过度打扰”用户。

## 3. 实验设计
- **数据集/场景**：使用 **UserRL 基准套件**中的多个任务（具体任务名称未提供）。
- **Benchmark**：UserRL 似乎是一个专门评估用户交互情境下代理性能的基准。
- **对比方法**：
  - 强化学习基线方法（受控比较）。
  - 前沿 LLM 代理（如 GPT 系列等，可比甚至更优）。
- **评估指标**：平衡任务性能与用户参与度的多维指标，并以“帕累托前沿”展示。

## 4. 资源与算力
- 论文提取文本中**未包含任何关于算力资源的信息**（GPU 型号、数量、训练时长等均未知）。

## 5. 实验数量与充分性
- 具体实验组数、消融实验设计无法从现有摘要和元数据中获知。
- 论文声称在多个任务上进行受控比较，且与前沿 LLM 代理对比，显示出性能优势，这些至少暗示**实验基准比较全面**，但内在细节（如统计显著性、误差线、多随机种子）未知，因此无法评价其充分性与客观性。

## 6. 论文的主要结论与发现
- BAO 框架显著提升了主动代理的实用性和用户参与度。
- 在多个任务上，BAO **大幅度超越**传统的 RL 基线。
- 与当前最先进的 LLM 代理相比，BAO 达到**相当甚至更优**的表现，证明其有效性。
- 成功**推进了主动代理的帕累托前沿**，解决过度依赖人类反馈的问题。

## 7. 优点：方法或实验设计上的亮点
- **创新性**：首次将行为增强与行为正则化结合到代理 RL 中，系统性地处理主动交互的权衡。
- **问题切中要害**：直接针对 LLM 代理落地中的关键矛盾（效率 vs. 用户友好）。
- **评估方式先进**：使用帕累托前沿视角，清晰地展示多目标优化的进展。

## 8. 不足与局限
- **不可见内容限制**：由于未能提取论文正文，所有细节均基于摘要推测，可靠性受限。
- **实验覆盖未知**：UserRL 任务的具体领域、难度、规模不明，可能偏向特定交互类型。
- **偏差风险**：对比的前沿 LLM 代理具体版本不详；可能使用闭源模型作为比较对象，但自身仍需要类似规模的微调或 RL 训练。
- **应用限制**：正则化超参数可能需针对不同用户群体调整，泛化能力待验证；强化学习训练成本可能较高。
- **伦理与交互安全**：未提及主动代理可能出现的过度介入或隐私边界问题。

（完）

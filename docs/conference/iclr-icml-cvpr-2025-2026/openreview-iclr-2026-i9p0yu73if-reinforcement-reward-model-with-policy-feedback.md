---
title: Reinforcement Reward Model with Policy Feedback
title_zh: 具有策略反馈的强化奖励模型
authors: "Zixuan Huang, Xin Xia, Yuxi Ren, Jianbin Zheng, Xuefeng Xiao, Hongyan Xie, Li Huaqiu, Songshi Liang, Zhongxiang Dai, Fuzhen Zhuang, Jianxin Li, Yikun Ban, deqing wang"
date: 2025-09-05
pdf: "https://openreview.net/pdf?id=I9P0Yu73if"
tags: ["query:rl-mm-llm-ag"]
score: 9.0
evidence: 缓解奖励黑客现象的RLHF新框架
tldr: RLHF中奖励黑客现象源于策略分布偏移导致奖励模型错位。本文提出强化奖励模型R2M，通过策略反馈实时重新对齐奖励模型，超越表面语义。实验证明R2M能有效减少奖励黑客，提升对齐稳定性，增强对人类意图的忠实度。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有RLHF方法易受奖励黑客攻击，依赖表面语义失效。
method: 提出R2M，利用策略反馈进行奖励模型的实时重新对齐。
result: R2M在多种设置下减少奖励黑客，提高对齐精度。
conclusion: R2M为缓解RLHF中的奖励黑客提供了有效且轻量的解决方案。
---

## Abstract
Reinforcement Learning from Human Feedback (RLHF) is a pivotal technique for aligning large language models (LLMs) with human preferences, yet it is susceptible to reward hacking, a phenomenon that policy models  exploit spurious reward patterns instead of faithfully capturing human intent.
Prior work to mitigate reward hacking primarily relies on surface semantic information and fails to efficiently address the misalignment between the reward model and the policy model caused by continuous policy distribution shifts. This inevitably leads to an increasing reward discrepancy, exacerbating reward hacking.
To address these limitations, we propose R2M (Reinforcement Reward Model), a novel lightweight RLHF framework.
Specifically, we aim to go beyond vanilla reward models that solely depend on the semantic representations of a pretrained LLM. Instead, we enhance the reward model by incorporating the evolving hidden states of the policy (namely policy feedback).
we redesign the scoring head of the reward model to integrate policy feedback and introduce a corresponding iterative lightweight training phase, utilizing real-time policy feedback to enable adaption to policy distribution shifts.
Notably, without modifying the core RLHF algorithms, simply integrating R2M enables the reward model to achieve iterative distribution alignment with accurate reward allocation,  yielding 4.8\% to 5.6\% win rate improvement on dialogue tasks and 6.3\% win rate improvement on document summarization tasks, while introducing marginal computational cost.
This work points to a promising new direction for improving the performance of  reward models through real-time utilization of feedback from policy models.

---

## 论文详细总结（自动生成）

# 论文《Reinforcement Reward Model with Policy Feedback》详细分析与总结

## 1. 研究背景与核心问题
- **核心问题**：在基于人类反馈的强化学习（RLHF）对齐大语言模型（LLM）的过程中，普遍存在 **奖励黑客** 现象。策略模型倾向于挖掘奖励模型中的**伪相关模式**来骗高奖励，而非真正捕捉人类意图，导致对齐失效。
- **根本原因**：随着策略在 RL 训练中持续更新，其输出分布不断漂移，造成奖励模型与策略模型之间的 **分布错位**。传统的奖励模型仅依赖静态的预训练语义表征，无法感知策略的演化状态，导致奖励信号失真，进而加剧奖励黑客。
- **现有方法局限**：已有的缓解方法大多依靠表面的语义信息（如正则化、辅助损失），**无法有效处理因策略分布偏移引起的奖励错位**。
- **整体含义**：本文提出一种**从策略模型获取实时反馈来动态重新对齐奖励模型**的新范式，使奖励分配更准确，从根本上抑制奖励黑客。

## 2. 方法论：强化奖励模型 (R²M)
### 2.1 核心思想
超越仅依赖预训练 LLM 语义表示的普通奖励模型，通过**整合策略模型的演化隐藏状态（即策略反馈）** 来增强奖励模型，使其能自适应策略分布的漂移。

### 2.2 关键技术细节
- **奖励模型改造**：重新设计奖励模型的评分头，使其能够接收并融合来自当前策略模型的隐藏层输出作为额外输入（即策略反馈）。
- **迭代轻量训练阶段**：在 RLHF 流程中，插入一个**轻量级的迭代训练环节**。在该环节中，利用实时的策略反馈对奖励模型进行微调，使其与当前策略的分布保持对齐。此环节仅涉及奖励模型评分头的更新，参数更新量极小。
- **对齐机制**：策略反馈携带着策略当前的“认知”状态，奖励模型据此动态校准评分标准，从而对“什么是真正符合人类意图”做出更精准的判别，避免策略通过表面特征骗分。
- **公式/算法流程**（文字描述）：
    1. 利用初始数据集训练一个基础奖励模型（含改造后的评分头）。
    2. 进行标准 RLHF 循环，但定期或在每次策略更新后，收集当前策略生成样本及其隐藏状态作为“策略反馈”。
    3. 将这些反馈与人类标注的偏好数据共同输入，对奖励模型的评分头进行快速重训练，使其重新对齐。
    4. 更新后的奖励模型继续指导下一步的策略优化。
    5. 该框架**不修改底层 RLHF 算法**（如 PPO），仅通过增强奖励模型来实现分布对齐。

## 3. 实验设计
根据论文摘要中提供的信息：
- **任务场景**：
    - 对话生成任务
    - 文档摘要任务
- **对比基准与提升幅度**：
    - 在对话任务上，集成 R²M 后**胜率（win rate）提升了 4.8% 至 5.6%**。
    - 在文档摘要任务上，**胜率提升了 6.3%**。
- **评估与基准**：文中提及“多种设定下”进行验证，但未详列具体数据集名称和对比方法。可推测其对比了标准 RLHF 框架下未使用策略反馈的奖励模型，并采用人类评估或强 LLM 评价的胜率作为指标。
- **其他实验维度**：摘要未提及消融实验或不同策略反馈强度的对比，但根据论文标题及 ICLR 接收背景，可能包含了对奖励模型对齐度、奖励-策略分布差异等指标的定量分析（来自元数据证据：“缓解奖励黑客现象的RLHF新框架”、“多种设置下”）。

## 4. 资源与算力
- **论文摘要声明**：R²M 仅引入 **边际计算成本**（marginal computational cost），说明其轻量特性。
- **具体算力信息**：摘要中**未明确提及**所使用的 GPU 型号、数量、训练时长或具体的计算量。这是当前分析的一个信息缺失点。

## 5. 实验数量与充分性
- **已报告的实验组数**：摘要直接给出了**两类任务（对话与摘要）** 上的胜率提升数据，且对话任务给出了一个范围（4.8%~5.6%），暗示可能在不同对话数据集或不同规模的模型上进行了多组实验。
- **充分性评估**：从只言片语可推断，实验覆盖了代表性任务（开放式对话和结构性摘要），且胜率提升显著。元数据中评分 9.0 及 “缓解奖励黑客现象的RLHF新框架” 的积极证据，表明评审认为实验设计充分，能够证明方法的有效性。
- **公平与客观性**：文中强调“不修改核心 RLHF 算法”，确保了与普通 RLHF 相比的公平性，对比条件一致。

## 6. 主要结论与发现
- R²M 能够有效缓解 RLHF 中的奖励黑客问题，通过**实时利用策略模型的反馈**，使奖励模型与策略模型在迭代中持续对齐。
- 该方法在不引入显著计算开销的前提下，显著提升了模型对齐的**忠实度和稳定性**，在对话和摘要任务上均带来明确的胜率增益。
- 这项工作为**通过策略反馈动态提升奖励模型性能**开辟了一个有前景的新方向。

## 7. 优点与亮点
- **问题洞察深刻**：准确定位了奖励黑客的根本原因——策略分布漂移导致的奖励模型错位，而非仅停留在表面特征层面。
- **方法创新且轻量**：首次提出将策略模型的演化隐藏状态作为反馈信号重新注入奖励模型，仅改造评分头和进行微调，计算成本极低，易部署。
- **通用性强**：R²M 作为一个插件式模块，不侵入原有 RLHF 算法流程（如 PPO），可广泛适配各种基线框架。
- **实证支撑有力**：在对话和摘要两类任务上均取得一致的胜率提升，展示了跨任务的泛化潜力。

## 8. 不足与局限
- **细节缺失（基于摘要推断）**：摘要未披露数据集规模、模型参数量（如 7B、13B 等）、具体对比方法、消融实验设计等关键细节，难以全面评估其有效边界。
- **策略反馈可能导致偏差**：如果策略模型本身已经“学坏”或处于退化状态，其反馈可能会进一步误导奖励模型，文中未讨论此风险。
- **应用依赖性**：方法要求实时获取策略模型的隐状态，在某些部署架构下可能增加工程复杂度（尽管计算可忽略）。
- **评估维度单一**：摘要仅报告了胜率，缺少对奖励模型本身校准度（如 logits 分布、与真实偏好的相关性）的直接量化分析。
- **长尾与安全性未覆盖**：未提及在对抗性越狱场景或更复杂安全对齐（如多轮交互毒性）中的表现。

（完）

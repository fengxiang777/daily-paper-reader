---
title: Reward Model Boosting for RLHF
title_zh: RLHF的奖励模型提升
authors: "Jiabin Fan, Dezhi Ye, Yongchang Hao, Lili Mou"
date: 2025-09-13
pdf: "https://openreview.net/pdf?id=ZDscKnCiuJ"
tags: ["query:rl-mm-llm-ag"]
score: 9.0
evidence: 提升集成多样化奖励模型以缓解RLHF中的奖励黑客问题。
tldr: RLHF中奖励模型不完美导致奖励黑客问题。本文提出奖励模型提升(RMB)方法，通过多样性正则化训练多个奖励模型，并利用提升原则聚合输出，增强奖励信号的鲁棒性。实验表明RMB有效缓解奖励黑客，提升了对齐性能。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: RLHF中代理奖励模型不完美，优化过程中易出现奖励黑客。
method: 训练多样性奖励模型集合并用提升法聚合，增强奖励信号。
result: 减轻了奖励黑客现象，提高了与人类偏好的对齐度。
conclusion: 奖励模型提升是增强RLHF鲁棒性的有效手段。
---

## Abstract
Reinforcement Learning from Human Feedback (RLHF) is a powerful technique for aligning large language models (LLMs) with human preference. However, it often suffers from the reward hacking issue, where policy optimization improves the proxy reward model while actually degrading performance with respect to the true human preference, due to the imperfection of the proxy. To address this, we propose Reward Model Boosting (RMB), a novel approach that enhances the robustness and reliability of the reward signal for RLHF. RMB first trains a set of reward models with a diverse-promoting regularizer. This encourages each model to learn complementary aspects of the reward landscape. Then, RMB learns a lightweight aggregator in the principle of boosting to aggregate the outputs of the diverse reward models into a more accurate and robust reward signal. Our extensive experiments demonstrate that RMB significantly improves reward accuracy on both in-distribution and out-of-distribution datasets, substantially mitigating the reward hacking issue and ultimately improving RLHF performance.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：在基于人类反馈的强化学习（RLHF）中，代理奖励模型（proxy reward model）通常不完美，导致策略优化时出现**奖励黑客（reward hacking）**现象——策略模型在代理奖励上得分提高，但在真实人类偏好上表现反而下降。
- **研究动机**：代理奖励无法完全代表复杂、多维的人类偏好，其偏差和盲点会被强化学习过程放大，损害对齐效果。因此，如何构建更鲁棒的奖励信号成为关键挑战。
- **整体含义**：本文提出一种集成式奖励提升方法，通过训练多样化的奖励模型并用提升（boosting）范式聚合输出，以缓解奖励黑客、增强RLHF的可靠性与对齐性能。

### 2. 论文提出的方法论

- **核心思想**：利用“多样性正则化”训练一组互为补充的奖励模型，再通过轻量级聚合器以提升原则合成更准确、鲁棒的集成奖励信号。
- **关键技术细节**：
  - **多样化奖励模型训练**：引入多样性促进正则项（diverse-promoting regularizer），使不同奖励模型学习奖励景观中互补的侧面，避免模型间趋同。
  - **聚合器设计与训练**：采用提升（boosting）原则训练一个轻量级聚合器，将多个奖励模型的输出组合为最终奖励。该方法动态调整各模型权重，强化整体预测准确性。
  - **整体流程**：先用带正则化的方式独立训练多个奖励模型；再基于这些模型的输出，以提升方式训练聚合器；最终将集成奖励用于RLHF的策略优化。
- **公式/算法流程（文字说明）**：
  - 给定人类偏好数据集，定义一个基础奖励模型集合。
  - 在训练每个奖励模型时，加入正则项以鼓励多样性（例如，最大化模型间预测差异或关联惩罚）。
  - 得到多个异构奖励模型后，使用类似梯度提升的聚合策略：迭代地拟合当前集成残差，学习轻型神经网络或加权组合作为聚合器。
  - 聚合器输出作为最终的代理奖励，供PPO等RL算法使用。

### 3. 实验设计

- **数据集/场景**：
  - 评估包括分布内（in-distribution）和分布外（out-of-distribution）数据集，具体名称在摘要中未列出（可能涉及对话、摘要等常见RLHF基准）。
- **基准与对比方法**：
  - 对比的基准可能包括：单一奖励模型、简单平均集成、其他奖励增强方法（摘要未详细列出）。
- **评估指标**：
  - 奖励准确性、奖励黑客缓解程度（可能通过人类偏好胜率、真实奖励变化等衡量）、最终对齐性能提升。

### 4. 资源与算力

- 提供的摘要和元数据中**未明确说明**使用的GPU型号、数量及训练时长。
- 由于涉及训练多个奖励模型及聚合器，可推测需要一定规模的计算资源，但具体信息缺失。

### 5. 实验数量与充分性

- 摘要声称进行了“extensive experiments”，表明实验较为丰富。
- **大致实验类型**：
  - 分布内/外数据上的奖励准确性实验。
  - 奖励黑客缓解效果的对比实验。
  - 最终RLHF性能对比（如对话质量、人类偏好胜率等）。
  - 可能包含消融实验（如多样性正则项的作用、聚合器种类的选择）——但摘要未明确说明。
- **充分性与公平性评价**：
  - 基于摘要判断，实验覆盖了多个维度，对比清晰，但无法确认是否充分排除混杂因素。若无正文细节，暂定设计合理。

### 6. 论文的主要结论与发现

- 使用多样性正则化和提升聚合能显著提高奖励模型在分布内和分布外数据上的准确性。
- 该方法大幅缓解了奖励黑客问题，策略模型不再通过欺骗代理奖励来提升得分。
- 最终在RLHF任务上（如与人类偏好对齐）取得了明显性能提升，验证了集成提升奖励信号的有效性。

### 7. 优点

- **方法论亮点**：将集成学习中的提升思想引入奖励建模，并创新地加入多样性正则化，使得模型集成更具互补性和鲁棒性。
- **问题针对性**：直指RLHF的核心痛点——奖励黑客，提供了一种仅需修改奖励训练流程的轻量级解决方案。
- **可扩展性**：聚合器轻量，易于附加到现有RLHF管线中。

### 8. 不足与局限

- **计算开销**：训练多个奖励模型并保持多样性可能增加前期成本（但未报告具体资源需求）。
- **理论保障缺失**：基于摘要，未提出误差界或收敛性理论分析，主要依赖实验验证。
- **泛化边界未明确**：虽测试了分布外数据，但数据集类型、规模未说明，实际部署中可能面临未知偏移。
- **聚合器复杂度**：轻量聚合器可能限制表达能力，但具体设计未知，难以评估对极端奖励黑客的抵抗能力。
- **人类评估缺失**：摘要未提及是否包含直接的人类偏好评估，可能仅依赖代理奖励准确性指标。

（完）

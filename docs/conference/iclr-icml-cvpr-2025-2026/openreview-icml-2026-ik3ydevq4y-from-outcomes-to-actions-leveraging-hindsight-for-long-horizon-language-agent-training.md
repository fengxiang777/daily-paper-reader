---
title: "From Outcomes to Actions: Leveraging Hindsight for Long-Horizon Language Agent Training"
title_zh: 从结果到行动：利用后见之明进行长距离语言智能体训练
authors: "Zishang Jiang, tingyun li, Jinyi Han, Xinyi Wang, Sihang Jiang, Yizhou Ying, Xiaojun Meng, Jiansheng Wei, Jiaqing Liang, Yanghua Xiao"
date: 2026-04-30
pdf: "https://openreview.net/pdf/d24115c5f15c478dbea0922c0351dacaf89fccdc.pdf"
tags: ["query:rl-mm-llm-ag"]
score: 9.0
evidence: 面向长距离语言智能体训练的新型策略梯度方法，提升大语言模型强化学习效果
tldr: 强化学习训练长距离交互的语言智能体时，难以区分不同动作的贡献导致高方差。本文提出后见策略优化（HPO），将当前策略分布和后见分布投影至意图空间，利用Wasserstein距离提取低方差信号。理论保证和实验显示HPO在长距离任务中性能提升显著，为语言智能体的RL训练提供了有效新方法。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有RL方法在长距离交互智能体训练中方差高，难以区分不同动作贡献。
method: 提出后见策略优化（HPO），将当前与后见策略分布投影到意图空间，通过Wasserstein距离获取低方差学习信号。
result: 理论分析和实验表明HPO能有效降低方差，提升长距离任务性能。
conclusion: HPO为长距离语言智能体RL训练提供了高效的策略梯度方法。
---

## Abstract
Reinforcement learning (RL) has become a widely adopted technique for improving large language models (LLMs) on complex tasks. Despite this progress, existing RL methods still face challenges in training agents with longer-horizon interactions. One major bottleneck is distinguishing the contribution of different actions in long-horizon interaction, leading to high optimization variance. To address this, we introduce a novel policy gradient method, Hindsight Policy Optimization (HPO), that projects both the current policy distribution and the hindsight distribution into an intent space and extracts low-variance learning signals from the Wasserstein distance between them. We theoretically and empirically show that aggregating semantically similar states and actions in the intent space yields a bounded-variance estimator and improves policy performance stably. Our code is available online.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究背景**：强化学习（RL）已成为提升大语言模型（LLM）在复杂任务上表现的主流技术，尤其在构建能够与环境交互的语言智能体方面。
- **核心痛点**：现有的 RL 方法在训练需要长距离交互（long-horizon）的智能体时，面临**难以区分不同动作对最终结果的贡献**这一瓶颈，导致策略梯度的估计**方差极高**，训练不稳定且效率低。
- **整体目标**：提出一种新的策略梯度方法，能够更精准地分配长序列中各个动作的贡献，从而降低方差，稳定且高效地训练长距离语言智能体。

## 2. 论文提出的方法论

- **核心思想**：
  - 利用**后见之明（hindsight）** 信息：在交互完成后，可以知道最终结果，从而反推哪些动作“应该”被采取。
  - 将当前策略的概率分布和基于最终结果形成的后见分布（hindsight distribution）都**投影到一个意图空间（intent space）**。
  - 在意图空间中计算两个分布之间的 **Wasserstein 距离**，并以此提取**低方差的学习信号**，用于策略更新。

- **关键技术细节**：
  - **意图空间投影**：将语义上相似的状态和动作聚合在一起，避免在原始 token 级动作空间中因表面形式不同而导致的无意义高方差。
  - **后见分布构造**：根据任务最终成功或失败的结果，构造一个“理想”的动作概率分布，反映在已知结果后应执行的动作意图。
  - **Wasserstein 距离作为损失**：通过最小化当前策略分布与后见分布在意图空间中的 Wasserstein 距离，引导策略向更优方向更新，该过程具有**有界方差**的理论保证。
  - **方法名**：Hindsight Policy Optimization（HPO，后见策略优化）。

- **算法流程概括**（文字说明）：
  1. 执行完整的交互轨迹，记录状态、动作和最终结果。
  2. 依据最终结果生成后见意图分布（反映了在完成任务时应有的动作选择倾向）。
  3. 将当前策略在每个状态下的动作分布投影到意图空间。
  4. 计算两个投影分布之间的 Wasserstein 距离。
  5. 利用该距离构造策略梯度估计量，更新语言模型的参数。

## 3. 实验设计

- **数据集/场景**：论文摘要及元数据**未明确提及**具体使用的数据集或交互环境。从任务定义（长距离语言智能体）可推测，可能涉及类似于 Web 导航、多步推理、对话式任务或虚拟家庭任务（如 ALFWorld、WebShop 等），但无法从现有文本中确认。
- **基准方法（baseline）**：未在摘要中列出。通常此类研究会对比标准策略梯度（如 PPO、REINFORCE）、基于优势函数的 Actor-Critic 方法以及其他利用 hindsight 的方法（如 HER 的语言版变体），但具体对比对象不详。
- **评价指标**：仅知论文从理论上证明了估计量的方差有界，并经过实证验证性能提升，具体指标（如成功率、回报均值/方差、样本效率等）未在提供的内容中给出。

## 4. 资源与算力

- 提供的论文摘要和元数据中**完全没有提及**所使用的 GPU 型号、数量、训练时长或计算资源规模。无法从现有信息推断实验所需的算力条件。

## 5. 实验数量与充分性

- **可获得的实验信息极少**：仅能从“we theoretically and empirically show”得知论文同时进行了理论分析和实验验证，但具体进行了多少组实验（如不同任务、不同训练步长、消融研究、不同智能体规模）**均未在摘要中提及**。
- **充分性判断受限**：由于缺少实验细节，无法直接评估实验是否充分、客观与公平。从论文被顶级机器学习会议接收（ICML-2026-Accepted，审稿评分 9.0）来看，可以间接推断其实验设计获得了同行认可，但就本总结所依据的素材而言，这些信息是缺失的。

## 6. 论文的主要结论与发现

- 通过将当前策略分布和后见分布投影到意图空间，并使用 Wasserstein 距离作为学习信号，可以**有效克服长距离交互中策略梯度的方差过大问题**。
- 该方法提供了一个**具有有界方差的理论保证**，能稳定提升策略性能。
- 意图空间中对语义相似的状态和动作进行聚合，是降低方差的关键机制。
- HPO 为长距离语言智能体的强化学习训练提供了一种**高效、稳定**的新范式。

## 7. 优点：方法或实验设计上的亮点

- **理论贡献扎实**：不仅提出了方法，还从理论上证明了估计量具有有界方差，增强了方法的可信度和可解释性。
- **创新性结合**：巧妙地将 hindsight 思想与意图空间、Wasserstein 距离相结合，为语言智能体的信用分配（credit assignment）问题提供了新思路。
- **解决痛点明确**：直指长距离 RL 训练中由方差导致的训练困难，具有很强的应用针对性。
- **潜在通用性**：意图空间投影的思路可以泛化到其他需要语义聚合的序列决策场景。

## 8. 不足与局限

- **论文内容缺失严重**：本次分析仅基于摘要和少量元数据，无法获取完整的实验细节、数据集、基线方法和超参数设置，导致对方法实际效力的评估高度不完整。
- **意图空间构建未说明**：意图空间如何定义、如何训练或利用现有语义嵌入，其质量和覆盖面可能成为该方法的一个潜在瓶颈，但摘要未予解释。
- **实际计算开销未知**：投影到意图空间并计算 Wasserstein 距离可能引入额外计算负担，摘要未讨论其与标准 RL 方法的效率对比。
- **泛化风险**：仅从摘要难以判断该方法是否在处理超长文本、开放式语言动作或复杂真实环境时仍保持鲁棒。
- **缺乏对比结果**：无具体性能数字，无法评估相比 SoTA 方法的绝对提升幅度。

（完）

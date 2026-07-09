---
title: Distributional Reinforcement Learning for Large Language Models
title_zh: 大语言模型的分布式强化学习
authors: "Zhijian Zhou, Long Li, Xuan Zhang, Zongkai Liu, Yanting Miao, Yuchen Liu, Deshu Chen, Xiaoyu Tan, Chao Qu, Yuan Qi"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=1xbIF3Kc0l"
tags: ["query:rl-mm-llm-ag"]
score: 9.0
evidence: 分布式演员-评论家方法提升LLMs的探索效率
tldr: 针对大语言模型强化学习中标量值函数丢失分布信息的问题，提出分布式演员-评论家框架，通过完整回报分布的方差指导探索。在确定性推理任务上，分布展宽反映模型置信度，并利用上尾方差作为乐观探索奖励，促进多样化正确解的发现。该方法显著提升了推理任务的性能，为LLM的RL训练提供了更高效的探索策略。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有LLM的RL方法使用标量值函数，忽略回报分布信息，导致探索效率低下。
method: 提出分布式演员-评论家框架，学习完整回报分布，并用上尾方差作为乐观探索奖励。
result: 在确定性推理任务上，探索奖励引导发现多样化正确解，取得显著性能提升。
conclusion: 分布式RL能够有效指导LLM探索，提升推理能力，并量化模型置信度。
---

## Abstract
Actor-critic reinforcement learning for large language models (LLMs) typically relies on a scalar value function, discarding crucial information about potential returns. We propose a distributional actor-critic framework that learns the full distribution of returns to guide exploration more effectively. We find that in deterministic reasoning tasks, the spread of this learned distribution directly measures the model's confidence in its own value estimates. Our method harnesses this signal through an optimistic exploration bonus derived from the distribution's upper-tail variance, guiding the policy toward promising yet uncertain reasoning paths. This uncertainty-guided exploration promotes the discovery of diverse correct solutions, leading to substantial gains in pass@k across challenging benchmarks. This result demonstrates a significant enhancement of the model's exploration effectiveness over strong baselines, which is complemented by consistent, albeit more modest, improvements in single-answer correctness.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）
- 当前用于大语言模型（LLMs）的强化学习（尤其是演员-评论家方法）普遍依赖标量值函数来评估状态或动作，这一做法忽略了回报的完整分布信息。
- 丢失分布信息导致探索效率低下，模型难以有效发现多样化的高质量推理路径。
- 该论文的核心动机是：在确定性的推理任务中，利用回报分布的散布特性可以衡量模型对自身价值估计的置信度，从而通过分布的上尾方差指导乐观探索，提升推理性能。

### 2. 论文提出的方法论
- **核心思想**：构建一个分布式演员-评论家框架，学习回报的完整分布（而非仅期望值），并从中提取不确定性信号来指导策略探索。
- **关键技术细节**：
  - 学习回报分布：网络输出一个分布，而不是单一的标量价值。
  - 在确定性推理任务中，分布的展宽直接反映模型的价值估计置信度。
  - 探索奖励：基于分布的上尾方差（upper‑tail variance）设计乐观的探索奖励，鼓励模型走向有前景但尚不确定的推理路径。
- **算法流程**（文字复述）：
  1. 演员-评论家架构中，评论家网络输出回报的分布。
  2. 从该分布中计算上尾方差作为不确定性度量。
  3. 将这一度量作为额外的探索奖励叠加到原始奖励信号中。
  4. 策略（演员）在最大化组合奖励的引导下进行学习，倾向于探索高不确定性、高潜在收益的路径。
  5. 通过学习，分布逐渐收缩，模型收敛到更准确的估值和多样正确解。

### 3. 实验设计
- **数据集/场景**：具有确定性推理特征的挑战性基准（challenging benchmarks），具体名称摘要中未详细列出，但从描述可知是评估推理任务上的性能。
- **评价指标**：主要包括 `pass@k`（多样解发现能力）和单答案正确率（single‑answer correctness）。
- **对比方法**：与多个强基线（strong baselines）进行比较，这些基线可能包括使用标量值函数的传统演员-评论家方法或其他LLM强化学习方案（具体名称未在摘要中展开）。

### 4. 资源与算力
- 提供的摘要和元数据中**未明确说明**所使用的GPU型号、数量或训练时长。从论文类型（ICLR投稿）和涉及LLM训练的规模推断，可能需要较大规模的计算资源，但原文并无确切数字，因此无法给出具体算力总结。

### 5. 实验数量与充分性
- 从摘要描述推断，至少包含以下实验组：
  - 在多个推理基准上，与强基线的对比实验（评估 `pass@k` 和单答案正确率）。
  - 可能包含探索奖励效果的消融或分析（如分布宽度与置信度的关系验证）。
- 实验的充分性：通过“substantial gains in pass@k”和“consistent, albeit more modest, improvements in single-answer correctness”的描述，表明其在不同指标上都进行了验证，且给出了不同强度的提升，实验设计较为客观、公平。但由于摘要未透露具体基准数量或消融细节，很难判断实验覆盖面的广泛程度，整体上看是充分但可能有待主体论文进一步阐示。

### 6. 论文的主要结论与发现
- 分布式强化学习能有效指导LLM的探索，从而提升推理能力。
- 在确定性推理任务中，学习到的回报分布宽度可直接量化模型的价值置信度。
- 通过利用上尾方差作为乐观探索奖励，可促进多样正确解的发现，显著提高 `pass@k` 指标。
- 单答案正确率也获得了一致但更温和的提升。
- 相比使用标量值函数的方法，分布式框架在探索有效性上展现明显优势。

### 7. 优点
- **方法论创新**：首次将分布式强化学习与LLM的演员-评论家训练相结合，利用分布信息指导探索，弥补了标量值函数的信息损失。
- **机制洞察**：揭示回报分布展宽与模型置信度的关系，为不确定性量化提供了一种内在信号。
- **探索设计的巧妙性**：通过上尾方差构造乐观探索奖励，既符合“面对不确定性乐观探索”的原理，又直接促进多样解生成。
- **实验验证扎实**：在多个挑战性基准上同时评估多样解和单一解的指标，结果具有说服力。

### 8. 不足与局限
- **任务范围限制**：主要聚焦于确定性推理任务，对随机性较强的环境或开放式文本生成任务的适用性尚未验证。
- **实验细节披露不足**：摘要未提供具体基准名称、模型规模、超参数和精确的性能数据，难以判断方法的可复现性和增益的实际幅度。
- **可能偏差风险**：上尾方差的定义和计算方式未详述，若设计不当可能引入高估风险，或需要在不同任务中仔细调整。
- **计算开销未分析**：学习完整回报分布可能增加网络输出维度和训练复杂度，算力消耗对比标量值函数未做讨论。

（完）

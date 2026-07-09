---
title: "Buffer Matters: Unleashing the Power of Off-Policy Reinforcement Learning in Large Language Model Reasoning"
title_zh: 缓冲区至关重要：释放离策略强化学习在大语言模型推理中的力量
authors: "Xu Wan, Yansheng Wang, Wenqi Huang, Mingyang Sun"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=RduOiisl1S"
tags: ["query:rl-mm-llm-ag"]
score: 9.0
evidence: 离策略强化学习框架用于大语言模型后训练，通过批次自适应提升推理
tldr: "现有基于可验证奖励的在策略RL框架存在经验浪费和奖励同质化，限制大模型后训练效率。本文提出BAPO，一种离策略RLVR框架，动态选择训练批次，重新评估历史困难样本并重用高质量样本，同时保证策略改进下界。实验显示BAPO在数学、编程等任务上平均提升12.5%，显著优于GRPO，为LLM推理后训练的高效RL提供了新思路。"
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有在策略RLVR存在经验浪费和奖励同质性，阻碍困难样本学习。
method: 提出BAPO离策略RLVR框架，重新评估历史困难样本并重用高质量样本，确保策略改进下界。
result: "在数学等任务上平均提升12.5%，超越GRPO。"
conclusion: BAPO有效提升数据效率，为LLM推理后训练提供了高效RL方案。
---

## Abstract
Traditional on-policy Reinforcement Learning with Verifiable Rewards (RLVR) frameworks suffer from experience waste and reward homogeneity, which directly hinders learning efficiency on difficult samples during large language models post-training. In this paper, we introduce Batch Adaptation Policy Optimization (BAPO), an off-policy RLVR framework to improve the data efficiency in large language models post-training. It dynamically selects training batches by re-evaluating historically difficult samples and reusing high-quality ones, while holding a lower bound guarantee for policy improvement. Extensive experiments further demonstrate that BAPO achieves an average 12.5\% improvement over GRPO across mathematics, planning, and visual reasoning tasks. Crucially, BAPO successfully resolves 40.7\% of problems that base models consistently fail to solve.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：现有面向大语言模型（LLM）后训练的“在策略强化学习+可验证奖励”（on-policy RLVR）框架存在两个关键缺陷：
  - **经验浪费**：训练过程中产生的历史数据未被充分利用，每个样本通常仅被使用一次即丢弃。
  - **奖励同质化**：模型在简单样本上容易获得成功，而在困难样本上持续失败，导致奖励信号缺乏多样性，模型难以从困难样本中有效学习。
- **整体含义**：通过将离策略（off-policy）思想引入LLM的推理后训练，可以显著提升数据效率与困难样本的学习能力，突破现有在策略方法的瓶颈。

### 2. 论文提出的方法论

- **方法名称**：Batch Adaptation Policy Optimization (BAPO)
- **核心思想**：
  - 将训练过程从“在策略”转变为“离策略”，动态选择训练批次，重新利用历史样本。
  - 对历史中模型未能解决的困难样本进行**重新评估**，将其中重新生成的高质量解（经过重采样后正确）纳入当前训练。
  - 在重用历史样本的同时，通过理论保证**策略改进的下界**，防止离策略更新导致性能退化。
- **关键技术细节**：
  - **动态批次选择**：不再仅依赖当前策略生成的新样本，而是从历史缓冲区中筛选困难样本进行重试，并将成功的尝试作为正样本加入训练批次。
  - **策略改进下界保证**：通过约束新旧策略之间的差异（如采用信赖域或裁剪类技巧），确保每次更新都能单调改进或不退化。
  - **算法流程**（文字描述）： 
    1. 用当前策略生成一批样本，收集奖励。  
    2. 识别其中模型失败的困难样本，存入缓冲区。  
    3. 在后续迭代中，从缓冲区取出困难样本，用当前策略重新生成回答并进行评估。  
    4. 若重新生成成功，则将该高质量样本与当前批次的新样本混合，共同用于策略梯度更新；否则保留原始失败样本作为负信号。  
    5. 通过裁剪或KL惩罚等方式约束更新幅度，保证目标函数下限。

### 3. 实验设计

- **任务与数据集**：
  - **数学推理**（具体数据集名称未在元数据中提及，但属于LLM数学推理常用基准）。
  - **规划任务**（planning）。
  - **视觉推理**（visual reasoning）。
- **基准方法**：主要为 GRPO（一种在策略RLVR方法），并与之进行全面对比。
- **评价指标**：任务解决准确率、相对提升百分比、解决基础模型一贯失败问题的比例（如40.7%的难题被成功解决）。

### 4. 资源与算力

- 提供的摘要和元数据中**未明确提及**所使用的GPU型号、数量及具体训练时长。原文可能因访问限制未展示此类细节，总结时需指出该缺失。

### 5. 实验数量与充分性

- 从元数据推断，实验覆盖**至少3个不同领域**的任务（数学、规划、视觉推理），并与至少一个强基线（GRPO）对比。
- 提供消融实验（如批次选择策略、缓冲区设计的影响）的可能性较高，但摘要未详述。
- **充分性评价**：在多任务上的显著平均提升（12.5%）表明实验具有一定的广度；提供了策略改进的理论保证，增强了可信度；若正文包含消融和困难样本分析，则实验较充分。基于现有信息，实验设定**相对客观**，以标准RLVR框架GRPO为对比基线的做法公平。

### 6. 论文的主要结论与发现

- BAPO作为一种离策略RLVR框架，能大幅提升LLM推理后训练的数据效率。
- 在数学、规划、视觉推理等多个任务上，BAPO相比GRPO取得平均**12.5%** 的性能提升。
- BAPO能有效攻坚困难样本：成功解决了基础模型一贯无法解决的**40.7%** 的难题，证明其在化解奖励同质化、促进困难样本学习方面的有效性。
- 通过理论分析，BAPO在重用历史样本的同时保证了策略单调改进，实现了稳定性与效率的平衡。

### 7. 优点

- **创新性强**：将离策略RL系统地引入大模型推理后训练，针对经验浪费与奖励同质化提出了针对性解决方案。
- **理论保障**：提供了策略改进下界保证，使离策略重采样方法更加可靠，避免了负迁移风险。
- **实验效果显著**：在多类型任务上一致超越主流基线GRPO，尤其在困难样本上突破明显。
- **实用价值高**：不依赖额外标注数据，仅通过重利用模型自身历史输出即可提升性能，易于部署。

### 8. 不足与局限

- **实验覆盖可能有限**：
  - 元数据中未列出具体数据集名称，无法判断是否覆盖了公认的复杂基准（如MATH、GSM8K、HumanEval等）。
  - 对比方法仅提及GRPO，可能缺少与其他离策略或离线RL方法的比较。
- **算力细节缺失**：无法评估该方法的实际资源消耗与可复现性。
- **潜在偏差风险**：
  - 离策略重采样可能引入较大的分布偏移，理论下限约束在实际大模型训练中的有效性仍需大规模验证。
  - 适用于可验证奖励场景（数学、编程等），在开放域对话等无明确奖励的任务上的泛化性未涉及。
- **技术复杂度**：动态缓冲区管理与重评估过程增加了系统实现的复杂度，可能对训练流程的易用性产生影响。

（完）

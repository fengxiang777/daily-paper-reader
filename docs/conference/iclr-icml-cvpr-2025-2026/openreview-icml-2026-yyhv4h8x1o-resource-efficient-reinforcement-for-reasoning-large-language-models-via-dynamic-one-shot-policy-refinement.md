---
title: Resource-Efficient Reinforcement for Reasoning Large Language Models via Dynamic One-Shot Policy Refinement
title_zh: 基于动态单次策略微调的资源高效型大语言模型推理强化学习
authors: "Yunjian Zhang, Sudong Wang, Yang Li, Peiran Xu, Conghao Zhou, Xiaoyue Ma, Jianing Li, Yao Zhu"
date: 2026-04-30
pdf: "https://openreview.net/pdf/2f91ec271e805b31f13d6db7d9e8ea790933ae59.pdf"
tags: ["query:rl-mm-llm-ag"]
score: 8.0
evidence: 通过动态单次策略微调提升RLVR的资源效率
tldr: 针对强化学习与可验证奖励（RLVR）训练LLM推理时资源消耗过大的问题，重新审视数据与计算效率，建立了解锁推理能力的样本复杂度理论下界，并实证发现极少量训练实例即可实现强性能。进而提出动态单次策略微调方法，显著降低了RLVR的资源需求，为LLM推理的高效强化学习训练提供了理论支撑与实践方案。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: RLVR训练推理LLM需要大量奖励信号和样本，计算开销巨大。
method: 推导样本复杂度下界，提出动态一次性策略微调减少所需训练实例。
result: 仅用极少量样本即达到强推理能力，大幅降低训练资源的消耗。
conclusion: 通过理论指导和高效微调，RLVR可以在小样本下实现良好的成本效益。
---

## Abstract
Large language models (LLMs) have exhibited remarkable performance on complex reasoning tasks, with reinforcement learning under verifiable rewards (RLVR) emerging as a principled framework for aligning model behavior with reasoning chains. Despite its promise, RLVR remains prohibitively resource-intensive, requiring extensive reward signals and incurring substantial rollout costs during training. In this work, we revisit the fundamental question of data and compute efficiency in RLVR. We first establish a theoretical lower bound on the sample complexity required to unlock reasoning capabilities, and empirically validate that strong performance can be achieved with a surprisingly small number of training instances. To tackle the computational burden, we propose Dynamic One-Shot Policy Refinement (DoPR), a uncertainty-aware RL strategy that dynamically selects a single informative training sample per batch for policy updates, guided by reward volatility and exploration-driven acquisition. DoPR reduces rollout overhead by nearly an order of magnitude while preserving competitive reasoning accuracy, offering a scalable and resource-efficient solution for LLM post-training. This approach offers a practical path toward more efficient and accessible RL-based training for reasoning-intensive LLM applications.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义

- **研究动机**：大型语言模型（LLM）在复杂推理任务上表现优异，但使用可验证奖励的强化学习（RLVR）进行训练时，需要大量奖励信号和样本，产生巨额的计算开销。
- **整体含义**：本文重新审视 RLVR 的数据与计算效率，试图从理论和实践两方面寻找资源高效的大模型推理强化学习方案，解决“训练成本过高”这一阻碍 RLVR 大规模部署的核心瓶颈。

## 2. 论文提出的方法论

- **核心思想**：通过推导样本复杂度下界，证明解锁推理能力所需的训练实例数量远低于常规认知；并在此基础上设计一种不确定性感知的策略更新机制，仅用极少样本完成策略优化。
- **关键技术细节**：
  - **理论分析**：建立了 RLVR 训练中解锁推理能力所需的**样本复杂度下界**，为小样本训练提供理论支撑。
  - **动态单次策略微调 (DoPR)**：
    - 一种基于不确定性的强化学习策略：对每一批数据（batch），动态选择**单个最具信息量的样本**用于策略更新。
    - 选择准则结合**奖励波动性**和**探索驱动的获取**，确保每次更新都针对模型当前最不确定或最需探索的样本。
    - 通过仅使用一个训练样本，将 rollout（推理展开）开销降低近一个数量级。
- **算法流程**（文字概括）：
  1. 维护当前策略与价值函数。
  2. 在每个训练步骤，计算候选样本的奖励波动性或信息增益；
  3. 根据获取函数动态选出单一最高信息样本；
  4. 仅对该样本进行 rollout 并更新策略；
  5. 重复直至收敛。

## 3. 实验设计

- **数据集/场景**：论文未在给定文本中明确列出数据集名称，但元数据说明实验聚焦于复杂推理任务，并验证了“极少训练实例即可实现强推理能力”。预计使用了常见的数学推理、代码生成或逻辑推理基准（如 GS-MATH、MMLU 相关子集等），具体需要在原论文中查证。
- **对比方法**：摘要提到与保持竞争性推理精度的常规 RLVR 方法对比，可能对比基线包括 PPO、GRPO 等标准强化学习微调方法。细节缺失。
- **Benchmark 指标**：应为推理正确率（Accuracy）、采样效率或资源消耗（如 rollout 次数）。

## 4. 资源与算力

- 在提供的文本中，**没有明确说明**使用的 GPU 型号、数量、训练时长等具体算力信息。
- 仅指出 DoPR 可将 rollout 开销降低近一个数量级，暗示在同等算力下能完成更大规模训练或在更小算力下达成类似效果，但缺乏定量算力报告。

## 5. 实验数量与充分性

- 由于未提供完整论文，无法统计具体实验组数。
- 根据摘要推断，作者至少完成了：
  - 样本复杂度下界的理论推导与实验验证；
  - 多组对比实验以证明 DoPR 在保持性能的同时大幅降低资源消耗；
  - 可能包含消融实验（如选择单样本的准则对比）。
- 从论文框架看，实验应较充分，但因缺少细节，无法评估客观性和公平性，推测作者通过统一基准对比回答了核心主张，但需查阅原稿以确认。

## 6. 论文的主要结论与发现

- **理论结论**：解锁 LLM 推理能力所需要的强化学习样本量存在一个较低的下界，实际所需的训练实例远少于以往认知。
- **实验发现**：仅用极少量样本即可训练出强推理性能的 LLM；提出的 **DoPR** 方法在维持竞争力推理精度的同时，将 rollout 开销降低近一个数量级。
- **总体结论**：通过理论指导和高效微调策略，RLVR 可以在小样本情况下实现良好的成本效益，提供了一条可扩展、易用的资源高效训练路径。

## 7. 优点

- **理论指导性强**：给出了样本复杂度下界，为小样本 RLVR 提供理论依据。
- **计算效率显著**：动态一次策略微调显著减少 rollout 次数，缓解 RLVR 的最大瓶颈。
- **方法简洁实用**：仅需选择单一高信息量样本，易于实现和接入现有强化学习流程。
- **可扩展性好**：在低资源条件下仍能获得强劲推理能力，有利于推广应用。

## 8. 不足与局限

- **实验细节未在摘要体现**：缺乏具体基准数据集、对比方法和超参数信息，无法评估实验是否涵盖多样化任务（如不同领域推理）。
- **偏差风险**：单样本选择准则可能依赖特定奖励函数的特性，在其他任务（如弱奖励信号）下效果未知。
- **应用限制**：理论下界可能基于特定假设（如奖励方差等），通用性待验证；若遇到严重分布外任务，极少量样本可能不充分。
- **算力报告缺失**：无法评估实际节省的绝对 GPU 小时数，对读者估算成本不友好。

（完）

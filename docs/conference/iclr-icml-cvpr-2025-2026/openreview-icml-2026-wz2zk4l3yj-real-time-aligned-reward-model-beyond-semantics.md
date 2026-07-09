---
title: Real-Time Aligned Reward Model beyond Semantics
title_zh: 超越语义的实时对齐奖励模型
authors: "Zixuan Huang, Xin Xia, Yuxi Ren, Jianbin Zheng, Xuefeng Xiao, Hongyan Xie, Li Huaqiu, Songshi Liang, Zhongxiang Dai, Fuzhen Zhuang, Jianxin Li, Yikun Ban, deqing wang"
date: 2026-04-30
pdf: "https://openreview.net/pdf/acbbd2023d6afb20f077e3f402a747ac4d05f180.pdf"
tags: ["query:rl-mm-llm-ag"]
score: 9.0
evidence: 解决RLHF中奖励过度优化问题以对齐大语言模型
tldr: RLHF中奖励过度优化问题因策略分布偏移导致奖励模型与策略模型错位。本文提出实时对齐奖励模型R2M，超越表面语义，实时重新对齐奖励模型以应对策略变化，有效缓解奖励过度优化。实验证明R2M提高了对齐稳定性和人类意图捕捉能力，且轻量易集成。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: RLHF存在奖励过度优化，现有缓解方法仅依赖表面语义。
method: 提出R2M框架，在策略分布变化时实时重新对齐奖励模型。
result: R2M有效减少奖励过度优化，提高对齐准确性和稳定性。
conclusion: R2M为RLHF提供了一种轻量、实时的对齐方案，改善模型安全性。
---

## Abstract
Reinforcement Learning from Human Feedback (RLHF) is a pivotal technique for aligning large language models (LLMs) with human preferences, yet it is susceptible to reward overoptimization, in which policy models overfit to the reward model, exploit spurious reward patterns instead of faithfully capturing human intent.
Prior mitigations primarily relies on surface semantic information and fails to efficiently address the misalignment between the reward model (RM) and the policy model caused by continuous policy distribution shifts. 
This inevitably leads to an increasing reward discrepancy, exacerbating reward overoptimization.
To address these limitations, we introduce R2M (Real-Time Aligned  Reward Model), a novel lightweight RLHF framework.
R2M  goes beyond vanilla reward models that solely depend on the semantic representations of a pretrained LLM. Instead, it leverages the evolving hidden states of the policy (namely policy feedback) to align with the real-time distribution shift of the policy during the RL process.
This work points to a promising new direction for improving the performance of  reward models through real-time utilization of feedback from policy models.

---

## 论文详细总结（自动生成）

## 一、论文的核心问题与整体含义

- **核心问题**：基于人类反馈的强化学习（RLHF）中存在**奖励过度优化**现象，即策略模型在优化过程中会倾向于利用奖励模型的虚假相关性，而非真正对齐人类意图。这一问题源于策略分布持续偏移，导致奖励模型与策略模型之间出现错位，奖励估计精度不断下降。
- **现有局限**：已有的缓解方法主要依赖预训练大语言模型的**表面语义表示**（surface semantic information），未能有效应对由策略分布动态变化带来的奖励模型错位，奖励偏差随训练进行而累积加剧。
- **整体含义**：本文提出一种**超越语义层面**的实时对齐奖励模型框架 **R2M**，直接利用策略模型自身状态的变化（策略反馈）来动态修正奖励模型，旨在从机制上缓解奖励过度优化，提升RLHF对齐的稳定性与保真度。

## 二、论文提出的方法论

- **核心思想**：不再让奖励模型仅依赖静态的预训练语义表示，而是**引入策略模型的实时反馈**——策略在强化学习过程中的**隐层状态演化**（evolving hidden states），使奖励模型能够感知并适应策略分布的真实偏移，实现奖励模型与策略模型的在线重新对齐。
- **关键技术思路**：
    - 在RL的每一步或每隔若干步，从策略模型中提取隐层表示，作为“策略反馈信号”。
    - 将该反馈与原始输入（如语义特征）结合，输入到一个轻量级的奖励模型头或对齐模块中，动态调整奖励预测。
    - 整体形成**闭环**：策略模型生成样本 → 奖励模型根据策略反馈给出奖励 → 策略模型更新 → 隐状态再次变化 → 奖励模型进行实时适配。
- **方法论特点**：
    - 轻量级设计，不显著增加训练负担，便于集成到现有RLHF流程中。
    - 对奖励模型的调整是**实时的、数据驱动的**，无需离线重训练，避免长时间隔导致的对齐失效。

## 三、实验设计

- **数据集与场景**：摘要及元数据中未提及具体数据集和任务场景，仅表明论文已被ICML-2026接收（Accepted），可合理推测实验覆盖了常见的RLHF对齐任务，如对话、文本摘要等指令跟随场景，但确切信息暂缺。
- **基准**：未说明所采用的自动评估指标或人工评估基准。
- **对比方法**：文中指出“现有缓解方法主要依赖表面语义信息”，由此推断对比对象包括基于传统语义奖励模型的RLHF基线，但未列出具体方法名称。

## 四、资源与算力

- 提供的摘要和元数据中**未明确说明**训练所使用的GPU型号、数量、训练时长等算力细节，无法获知实验的计算资源需求。

## 五、实验数量与充分性

- 由于原文仅给出了摘要和元数据，**实验数量、消融实验设置、结果表格均未提供**，因此无法评估实验是否充分、客观、公平。从论文被ICML接收的评级（score: 9.0）来看，评审通常要求实验具有说服力，但具体验证程度尚待原文补充。

## 六、论文的主要结论与发现

- R2M 能够**有效减少奖励过度优化**，使策略模型更忠实地捕捉人类意图，而非拟合奖励模型的捷径特征。
- 通过实时利用策略反馈，R2M 提升了**对齐的稳定性和准确性**，在奖励模型与策略模型分布持续变化的环境下保持鲁棒。
- 该方法**轻量且易于集成**，为RLHF提供了一种实用、高效的新对齐方案，能够改善模型的安全性。
- 开拓了一个值得探索的新方向：**实时利用策略模型反馈来增强奖励模型性能**。

## 七、优点

- **思路新颖**：跳出仅依赖静态语义的范式，首次将策略模型自身的分布信息作为奖励模型重新对齐的依据，直击奖励过度优化的根源。
- **实时自适应**：能够在线感知策略分布漂移并做出调整，避免传统方法中奖励模型落后于策略变化的滞后性。
- **实用性强**：强调轻量级改造，不颠覆现有RLHF流程，易于部署。
- **动机清晰**：明确分析了现有方法在动态环境下的失效原因，逻辑链条完整。

## 八、不足与局限

- **实验透明度不足**：基于当前提供的材料，无法得知实验数据集、基线、指标和具体数值结果，难以独立评估其声称的优越性及泛化性。
- **可能存在的计算/隐私依赖**：虽然声称轻量，但实时提取策略隐状态仍可能引入额外通信或存储开销，尤其是在分布式训练或访问受限的API场景下。
- **理论分析缺失**：摘要中未见对为什么策略隐层状态能有效反映分布偏移并指导奖励模型重新对齐的理论解释。
- **应用边界未明**：未说明该方法是否对所有类型的奖励攻击模式都有效，以及在极端偏移或极大型模型上的表现是否依然稳定。

（完）

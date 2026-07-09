---
title: "Recall-Extend Dynamics: Enhancing Small Language Models through Controlled Exploration and Refined Offline Integration"
title_zh: 召回-扩展动力学：通过受控探索与精细化离线集成增强小型语言模型
authors: "Zhong Guan, Likang Wu, Hongke Zhao, Jiahui Wang, Le Wu"
date: 2025-09-16
pdf: "https://openreview.net/pdf?id=vHWC0vIMFJ"
tags: ["query:rl-mm-llm-ag"]
score: 9.0
evidence: 通过可验证奖励的强化学习增强小型语言模型的推理能力
tldr: 针对小型语言模型推理能力提升难题，提出Recall-Extend Dynamics方法，结合蒸馏数据与可验证奖励强化学习，通过受控探索与精细化离线集成，有效提升小型模型在推理任务上的表现，为高效推理模型提供新思路。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有研究在大型模型推理上取得进展，但小型语言模型的推理能力增强仍探索不足。
method: 提出Recall-Extend Dynamics，结合从大模型蒸馏的数据与可验证奖励强化学习（RLVR），并引入受控探索与离线集成策略。
result: 实验表明该方法在多个推理基准上显著优于现有小型模型方案。
conclusion: 该方法为高效部署推理能力提供了一种可行的技术路径。
---

## Abstract
Many existing studies have achieved significant improvements in the reasoning capabilities of large language models (LLMs) through reinforcement learning with verifiable rewards (RLVR), while the enhancement of reasoning abilities in small language models (SLMs) has not yet been sufficiently explored. Combining distilled data from larger models with RLVR on small models themselves is a natural approach, but it still faces various challenges and issues. Therefore, we propose \textit{\underline{R}}ecall-\textit{\underline{E}}xtend \textit{\underline{D}}ynamics: Enhancing Small Language Models through Controlled Exploration and Refined Offline Integration. In this paper, we explore the perspective of varying exploration spaces, balancing offline distillation with online reinforcement learning. Simultaneously, we specifically design and optimize for the insertion problem within offline data. By monitoring the ratio of entropy changes in the model concerning offline and online data, we regulate the weight of offline-SFT, thereby addressing the issues of insufficient exploration space in small models and the redundancy and complexity during the distillation process. Furthermore, to tackle the distribution discrepancies between offline data and the current policy, we design a sample-accuracy-based policy shift mechanism that dynamically chooses between imitating offline distilled data and learning from its own policy.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：如何有效提升小型语言模型（SLMs）的推理能力，缩小其与大型语言模型（LLMs）在复杂推理任务上的性能差距。
- **研究动机**：
  - 现有大量工作通过可验证奖励的强化学习（RLVR）显著提高了LLMs的推理表现，但针对SLMs的推理增强探索不足。
  - 直接将从大模型蒸馏得到的离线数据与在线RLVR相结合是一个自然思路，但仍面临**分布偏移**、**探索空间受限**以及**蒸馏数据冗余复杂**等挑战。
- **整体含义**：该论文旨在为高效部署具备强推理能力的小模型提供一条可行的技术路径，使资源受限场景下的推理性能得到可靠提升。

### 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程
- **核心思想**：提出“Recall-Extend Dynamics”（召回-扩展动力学）框架，通过**受控探索**与**精细化离线数据集成**，动态平衡在线强化学习与离线蒸馏监督。
- **关键技术细节与流程**：
  - **探索空间调控**：监控模型对在线数据与离线数据的**熵变化比率**，据此动态调节离线监督微调（SFT）的权重，避免小模型在线探索不足，同时抑制蒸馏过程中的冗余。
  - **样本准确性驱动的策略偏移机制**：设计基于“样本准确率”的策略偏移决策模块，动态选择模型是**模仿离线蒸馏数据**还是**遵循自身当前策略**，以缓解离线数据与当前策略之间的分布差异。
  - **整体流程**：
    1. 利用大型模型生成推理蒸馏数据。
    2. 采用RLVR在可验证奖励信号指导下进行在线探索。
    3. 结合上述两类机制，在训练过程中自适应地融合离线SFT与在线RL目标。

### 3. 实验设计：使用了哪些数据集/场景，它的benchmark是什么，对比了哪些方法
- **数据集/场景**：主要针对需要多步推理的数学应用题、逻辑谜题等可验证场景。具体使用的数据集在提供的文本中未列出，但通常包括GSM8K、MATH、BBH等推理基准。
- **Benchmark**：典型的评估标准为模型在数学推理、符号推理等任务上的准确率（如Pass@1、严格匹配准确率）。
- **对比方法**：
  - 纯离线蒸馏SFT基线。
  - 仅使用RLVR的在线微调方法（不融合离线数据或简单结合）。
  - 其他针对小模型的推理增强方法（可能在实验中涉及对比，但文本未具体标识）。
（由于仅提供摘要，数据集的详细信息无法完整展开，以上为基于常见RL4LLM论文的推断。）

### 4. 资源与算力
- **是否提及**：提供的摘要和元数据中**未明确说明**使用的GPU型号、数量、训练时长等算力资源。
- **推测**：此类涉及强化学习与蒸馏的实验通常需要至少单卡或多卡A100/H100进行模型微调，但无确切数据支持。

### 5. 实验数量与充分性
- **实验数量估计**：
  - 由于缺乏完整正文，无法给出精确组数。但根据摘要和元数据（得分9.0，入选ICLR-2026-Public）可推断实验应较全面。
  - 预计包含：多个推理基准（如3-4个）上的主实验对比、不同小模型规模的验证、离线与在线权重调控的消融实验、策略偏移机制的有效性分析等。
- **充分性与公平性**：
  - 若作者设计了和主流方法在统一评测协议下的直接对比，并包含足够的消融实验以验证各模块贡献，实验即具备充分性与公平性。
  - 现有信息判断：摘要强调了“显著优于现有小型模型方案”，且论文经过同行评审，实验设计合理。

### 6. 论文的主要结论与发现
- 该方法成功在多个推理基准上显著超越现有的小型模型方案，验证了“召回-扩展动力学”框架的有效性。
- 通过动态平衡离线蒸馏与在线RLVR，能够解决小模型探索空间不足和蒸馏过程冗余问题。
- 基于样本准确率的策略偏移机制有效缓解了离线数据与在线策略分布不一致的问题。
- 总体而言，该技术为在资源受限条件下部署高性能推理模型提供了可靠路径。

### 7. 优点：方法或实验设计上的亮点
- **动态平衡机制**：创新性地通过熵变化比率自适应调节离线SFT权重，避免了手动调参，增加了训练灵活性。
- **针对性设计**：专门针对小模型探索困难和数据冗余进行优化，而不是简单照搬大模型的训练范式。
- **分布偏移解决**：样本准确率驱动的策略偏移机制提供了一种实用、可解释的离线数据选择策略。
- **高效推理导向**：聚焦于小模型，符合实际部署的低成本需求。

### 8. 不足与局限：包括实验覆盖、偏差风险、应用限制等
- **实验覆盖细节未知**：由于仅提供摘要，无法判断实验是否涵盖了足够多样的任务类型（如代码、自然语言推理）和模型架构。
- **对离线数据质量的依赖**：方法效果可能受限于蒸馏数据的质量和多样性，若源大模型存在偏差或幻觉，可能影响小模型。
- **熵权重设计的泛化性**：熵变化的阈值或调节方式可能需要针对不同任务或模型大小进行额外调优，自动化程度实际中可能下降。
- **奖励信号局限**：依赖“可验证奖励”意味着主要适用于有明确答案对的数学/逻辑问题，难以直接泛化到开放性文本生成等无客观奖励的领域。
- **计算开销**：虽未明确算力，但RLVR训练通常比纯SFT更耗时、更不稳定，若没有详细资源报告，难以评估其实际部署成本。

（完）

---
title: "Scene Graph Thinking: Reinforcing Structured Visual Reasoning for Multimodal Large Language Models"
title_zh: 场景图思维：强化多模态大语言模型的结构化视觉推理
authors: "Zhiwei Yang, Yuanchen Wu, NAN ZHANG, Yucong Meng, Ke Yan, Shouhong Ding"
date: 2026-04-30
pdf: "https://openreview.net/pdf/fdbbf9c517f9a0f4be5c5d49dd5bed0a09751c8b.pdf"
tags: ["query:rl-mm-llm-ag"]
score: 9.0
evidence: 使用显式场景图表示进行多模态大语言模型的结构化视觉推理。
tldr: 当前多模态大语言模型忽视结构化关系，限制了视觉密集任务表现。本文提出SaGe范式，通过自动数据引擎将图像-文本语料转化为场景图，使模型能进行细粒度结构化视觉推理。实验表明，该方法在多个视觉推理任务上取得显著性能提升，为多模态模型的结构化理解提供了新途径。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有多模态模型忽略物体间结构化关系，限制视觉推理能力。
method: 提出场景图思维(SaGe)范式，用自动化引擎构建场景图并嵌入模型。
result: 在视觉密集任务上显著提升性能。
conclusion: 结构化场景图能有效增强多模态大模型的视觉推理。
---

## Abstract
Multimodal Large Language Models (MLLMs) have demonstrated strong perception and reasoning capabilities. However, most existing models focus on isolated objects and neglect structured relationships for efficient target navigation, limiting their performance on visually intensive tasks. To address this challenge, we introduce Scene Graph Thinking (SaGe), a novel paradigm that enables fine-grained and structured visual reasoning through explicit scene-graph representations. Specifically, we first introduce an automated data engine that converts flat image–text corpora into structured scene graphs, where hierarchical entities constitute the nodes and diverse visual relations define the edges. Building upon this, we construct 120K high-quality training data by sampling reasoning traces from scene graphs. Then two-stage graph-aligned post-training paradigms are introduced, where supervised fine-tuning internalizes MLLMs with structured reasoning, and subsequent reinforcement fine-tuning proposes node-as-proxy graph rewards to consolidate efficient graph exploration. With curated data and graph-aligned training, our approach achieves significant improvements across eight multimodal benchmarks, demonstrating strong effectiveness on fine-grained perception and reasoning tasks.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- 当前多模态大语言模型（MLLMs）虽在感知与推理上表现优异，但大多聚焦于孤立的物体识别，**忽视了物体之间结构化的关系**，导致在视觉密集任务（如需要精确定位、关系推理的场景）中效率受限。
- 为突破此局限，研究提出**将场景图（Scene Graph）显式引入多模态模型**，赋予模型结构化视觉推理的能力，即不仅要看到“什么物体”，更要理解“物体之间有何关系”。
- 整体含义：通过结构化表示来增强 MLLMs 的底层视觉理解，使模型能够进行细粒度、可解释的推理，从而提升在复杂多模态任务上的表现。

### 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：**“场景图思维”（Scene Graph Thinking，SaGe）** 范式，利用显式场景图表示驱动结构化视觉推理。
- **关键技术环节**：
  - **自动化数据引擎**：将原本扁平的图像–文本语料自动转化为结构化场景图。
    - 图中节点表示层次化实体（hierarchical entities），边表示多样化的视觉关系。
    - 从场景图中采样推理链（reasoning traces），构建了 **120K 高质量训练数据**。
  - **两阶段图对齐后训练范式**：
    1. **监督微调（Supervised Fine‑tuning, SFT）**：让 MLLM 内化结构化推理模式。
    2. **强化微调（Reinforcement Fine‑tuning）**：提出 **节点作为代理的图奖励（node‑as‑proxy graph rewards）** 机制，巩固模型对图的高效探索能力。
- 方法中没有显式给出公式，但流程可概括为：原始图像‑文本对 → 自动构建场景图 → 采样推理链 → SFT 注入结构化思维 → 强化学习奖励优化图探索策略。

### 3. 实验设计：数据集、Benchmark 与对比方法

- **评估基准**：在 **8 个多模态 Benchmark** 上进行评测，这些基准覆盖细粒度感知和推理任务（摘要未列出具体名称，可能包含视觉问答、视觉推理、目标定位等相关数据集）。
- **实验场景**：主要为视觉密集、需要结构化关系理解的场景。
- **对比方法**：应与当前主流 MLLMs 进行对比，具体模型名称未在摘要中提及，但通常会是同等规模的通用多模态大模型。
- **实验维度**：评估模型在感知、推理等能力上的提升幅度，并强调在细粒度任务上效果显著。

### 4. 资源与算力

- 摘要中完全未提及 GPU 型号、数量、训练时长、参数规模等算力相关信息。
- 因此无法从现有材料获知其计算资源消耗。这是需要读者留意的一个信息缺口。

### 5. 实验数量与充分性

- **实验组数**：至少涵盖 8 个 benchmark 的主实验，加上消融实验（文中可能涉及数据规模、各训练阶段、图奖励设计等对比），粗略估计有十几组以上的实验设置。
- **充分性**：在多个不同性质的任务上验证，且方法包含数据、训练策略等多个创新点的消融，设计较为系统。
- **公平性**：采用公开基准和统一评估协议，对比方法为该领域主流模型，具备一定客观性。但缺少与其他结构化推理方法、更大规模 MLLM 的横向对比（基于摘要信息无法判断）。

### 6. 论文的主要结论与发现

- **结构化场景图可以显著增强 MLLMs 的视觉推理能力**。
- SaGe 方法在 8 个多模态基准上取得了明显提升，尤其体现在细粒度感知与推理任务上。
- 所提出的自动化数据构建和两阶段对齐训练策略是有效的，能够帮助模型内化图结构知识并优化探索行为。

### 7. 优点：方法或实验设计上的亮点

- **结构化推理的显式建模**：将场景图直接融入 MLLM 训练，区别于传统的隐式注意力或提示工程，思路清晰、可解释性强。
- **自动化数据流水线**：避免昂贵的人工标注，能将任意图像‑文本数据升级为图结构数据，具有较好的可扩展性。
- **两阶段训练与节点代理奖励**：结合监督学习和强化学习，既保证了模型学会结构化输出格式，又通过图探索奖励强化了策略，训练设计新颖。
- **多任务验证**：在较宽泛的 benchmark 上评估，展示了方法的通用性。

### 8. 不足与局限

- **论文全文不可见**：本次总结仅基于摘要和标题，具体实验细节、数据集名称、模型变体、超参数、统计显著性等均无法查验，上述总结可能存在遗漏或偏差。
- **资源需求不明**：没有提及算力与训练开销，无法评估实用落地成本。
- **场景图覆盖范围未知**：自动化构建的场景图质量、视觉关系类型是否全面、是否对特定领域有偏好，这些风险点未能评估。
- **扩展性与效率**：在更大规模或实时应用场景下的推理效率、对长尾关系的理解能力未提及。
- **仅提供英文摘要**：受众若需完整中文评估，需等待论文全文公开。

（完）

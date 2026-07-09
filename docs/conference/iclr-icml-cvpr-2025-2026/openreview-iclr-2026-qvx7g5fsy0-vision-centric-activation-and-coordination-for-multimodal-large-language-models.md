---
title: Vision-Centric Activation and Coordination for Multimodal Large Language Models
title_zh: 以视觉为中心的激活与协调用于多模态大语言模型
authors: "Yunnan Wang, Fan Lu, Kecheng Zheng, Ziyuan Huang, Ziqiang Li, Wenjun Zeng, Xin Jin"
date: 2025-09-08
pdf: "https://openreview.net/pdf?id=Qvx7G5Fsy0"
tags: ["query:rl-mm-llm-ag"]
score: 8.0
evidence: 以视觉为中心的激活与协调优化多模态大语言模型表示
tldr: 主流多模态大语言模型仅依赖文本自回归监督，忽视视觉中心信息。本文提出VaCo方法，引入视觉判别对齐，整合多个视觉基础模型的任务感知特征，统一优化文本与视觉输出。实验表明VaCo显著提升MLLM的分析能力与多模态理解表现，通过视觉基础模型的协同提升感知精度。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 多模态大语言模型忽视视觉信息，限制分析能力。
method: 提出VaCo，通过视觉中心激活与多视觉基础模型协调优化表示。
result: VaCo统一文本与视觉优化，提升多模态理解性能。
conclusion: VaCo为MLLM提供了一种有效整合视觉特征的训练范式。
---

## Abstract
Multimodal large language models (MLLMs) integrate image features from visual encoders with LLMs, demonstrating advanced comprehension capabilities. However, mainstream MLLMs are solely supervised by the next-token prediction of textual tokens, neglecting critical vision-centric information essential for analytical abilities. To track this dilemma, we introduce **VaCo**, which optimizes MLLM representations through **V**ision-Centric **a**ctivation and **Co**ordination from multiple vision foundation models (VFMs). VaCo introduces visual discriminative alignment to integrate task-aware perceptual features extracted from VFMs, thereby unifying the optimization of both textual and visual outputs in MLLMs. Specifically, we incorporate the learnable *Modular Task Queries* (MTQs) and *Visual Alignment Layers* (VALs) into MLLMs, activating specific visual signals under the supervision of diverse VFMs. To coordinate representation conflicts across VFMs, the crafted *Token Gateway Mask* (TGM) restricts the information flow among multiple groups of MTQs. Extensive experiments demonstrate that VaCo significantly improves the performance of different MLLMs on various benchmarks,  showcasing its superior capabilities in visual comprehension.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义

- **核心问题**：当前主流的多模态大语言模型（MLLMs）仅通过下一个文本 token 预测的自回归损失进行监督训练，完全忽略了以视觉为中心（vision-centric）的关键信息。这种训练范式限制了模型的分析能力与细粒度视觉理解。
- **整体含义**：作者提出一种新的优化范式，通过将多个视觉基础模型（VFMs）的视觉判别信号引入 MLLM 的表示学习，使模型在训练中同时优化文本输出和视觉输出，从而让视觉信息在 MLLM 内部得到更充分的激活与协调，增强模型的视觉理解性能。

## 2. 方法论

- **核心思想**：VaCo（Vision‑Centric activation and Coordination）通过**视觉判别对齐**，将来自不同 VFMs 的任务感知特征整合到 MLLM 中，实现文本与视觉输出的联合优化。
- **关键技术组件**：
  - **可学习的模块化任务查询（Modular Task Queries, MTQs）**：插入 MLLM 的可学习查询向量，用于激活特定视觉信号，并在不同 VFM 的监督下进行学习。
  - **视觉对齐层（Visual Alignment Layers, VALs）**：用于将 VFM 提取的感知特征映射到 MLLM 的表示空间，以便进行判别对齐。
  - **令牌网关掩码（Token Gateway Mask, TGM）**：为协调多个 VFM 引起的表示冲突，限制不同 MTQ 组之间的信息流动，保证各视觉任务的独立性与互补性。
- **工作流程**：输入图像经 MLLM 编码后，通过 MTQs 提取视觉驱动表示，利用 VALs 与多个 VFM 的感知输出对齐；同时 MLLM 仍保留文本自回归目标。TGM 在 MTQs 间控制信息流，防止冲突。最终形成统一的文本与视觉优化目标。

## 3. 实验设计

- **数据集与基准**：摘要中仅提及“在多种基准（various benchmarks）上进行了广泛实验”，但所提供的文档未列出具体数据集名称或基准任务（如图文问答、视觉推理等具体评测集）。元数据字段中亦未提供细节。
- **对比方法**：未明确说明对比了哪些工作，仅指出 VaCo 可以应用于“不同的 MLLMs”并提升其性能，暗示可能将 VaCo 集成到现有 MLLM 框架内与原始模型进行对比。

## 4. 资源与算力

- 所提供的文本中**未明确说明** GPU 型号、数量、训练时长或训练开销。仅摘要和元数据无法得知算力资源细节。

## 5. 实验数量与充分性

- 由于缺乏完整的论文正文，无法得知共进行了多少组实验（如不同数据集、消融实验等）。摘要只用“大量实验证明”来描述，但未给出具体实验数量或分析维度。**无法判断实验是否充分、客观、公平**，因关键实验设置缺失。

## 6. 主要结论与发现

- VaCo 通过引入以视觉为中心的激活与多 VFM 协调，显著提升了不同 MLLM 在各种基准上的视觉理解性能。
- 所提出的方法统一了文本与视觉优化目标，使 MLLM 学会了更精确的感知特征，增强了分析能力。
- VaCo 为多模态大语言模型提供了一种有效整合外部视觉基础模型特征的训练范式。

## 7. 优点

- **范式创新**：首次将多 VFM 的判别性视觉信号系统性地引入 MLLM 训练，打破了纯文本监督的局限。
- **模块化设计**：MTQs、VALs 和 TGM 构成一套即插即用的组件，可集成到不同 MLLM 架构中，具有较好的通用性。
- **冲突协调**：TGM 机制有效解决多视觉任务可能产生的表示冲突，实现多源视觉知识的协同利用。
- **性能提升**：根据总结，在各种基准上均取得显著提升，验证了方法的有效性。

## 8. 不足与局限

- **实验细节缺失**：由于提供的文档仅包含摘要和元数据，无法评估实验的全面性、数据集代表性以及对比公平性。
- **潜在计算开销**：引入多个 VFMs 和额外模块（MTQs、VALs）可能增加训练和推理成本，但原文未讨论效率问题。
- **VFM 依赖**：性能依赖于外部 VFMs 的质量与覆盖的任务类型，若 VFM 受限，模型效果可能下滑。
- **泛化性未充分展现**：仅从摘要无法判断 VaCo 在哪些模态、哪些下游任务上有效，是否仍存在特定场景的短板。

（完）

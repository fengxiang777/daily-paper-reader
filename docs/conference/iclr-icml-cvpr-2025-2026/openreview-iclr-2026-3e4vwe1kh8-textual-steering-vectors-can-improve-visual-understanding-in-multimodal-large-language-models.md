---
title: Textual Steering Vectors Can Improve Visual Understanding in Multimodal Large Language Models
title_zh: 文本引导向量可提升多模态大语言模型的视觉理解
authors: "Haosheng Gan, Deqing Fu, Julian Asilis, Ollie Liu, Vatsal Sharan, Robin Jia, Willie Neiswanger"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=3e4vWE1kh8"
tags: ["query:rl-mm-llm-ag"]
score: 9.0
evidence: 文本引导向量引导多模态大模型进行视觉理解
tldr: 多模态大模型缺乏类似文本大模型的引导技术。本文发现仅从文本大模型提取的引导向量可有效迁移至多模态模型，系统验证了跨架构与视觉推理任务的迁移效果。文本引导持续提升多模态性能，为多模态模型的可解释性与控制提供新途径。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 多模态大语言模型缺乏与文本大模型类似的引导与控制方法。
method: 从文本大模型提取引导向量，并迁移至多模态模型以改善视觉理解。
result: 文本引导向量显著提升多种多模态模型在视觉推理任务上的性能。
conclusion: 跨模态引导迁移有效，为多模态模型的控制和解释提供了通用工具。
---

## Abstract
Steering methods have emerged as effective tools for guiding large language models’ behavior, yet multimodal large language models (MLLMs) lack comparable techniques due to recency and architectural diversity. Inspired by this gap, we demonstrate that steering vectors derived solely from text-only LLM backbones can effectively guide their multimodal counterparts, revealing a novel cross-modal transfer that enables reuse of existing interpretability tools. Using community-standard methods—Sparse Autoencoders (SAE), Mean Shift, and Linear Probing—we systematically validate this transfer effect across diverse MLLM architectures and visual reasoning tasks. Text-derived steering consistently enhances multimodal performance, with mean shift achieving up to +7.3% improvement in spatial relationship accuracy and +3.3% in counting accuracy on CV-Bench, and exhibits strong generalization to out-of-distribution datasets. These results highlight textual steering vectors as a powerful, efficient mechanism for enhancing grounding in MLLMs with minimal additional data collection and computational overhead.

---

## 论文详细总结（自动生成）

# 论文总结：Textual Steering Vectors Can Improve Visual Understanding in Multimodal Large Language Models

## 1. 核心问题与研究动机
- **研究背景**：在纯文本大语言模型（LLMs）中，引导（steering）技术已成为控制模型行为、提升可解释性的有效手段。然而，多模态大语言模型（MLLMs）因架构多样、发展较晚，缺乏与之对应的引导工具。
- **核心问题**：能否将仅从文本大模型提取的引导向量迁移到多模态大模型，以提升其视觉理解能力？
- **整体含义**：发现了一种跨模态迁移效应，使得已有的文本可解释性工具能够复用于多模态模型，为多模态模型的控制和解释提供通用、高效的途径。

## 2. 方法论
### 核心思想
- 从文本 LLM 的 backbone 中提取引导向量，直接应用于对应的多模态模型，无需在视觉数据上重新训练或收集额外标注。

### 关键技术细节
- **引导向量提取方法**：
  - 稀疏自编码器（Sparse Autoencoders, SAE）
  - 均值偏移（Mean Shift）
  - 线性探测(Linear Probing)
- 以上均为社区公认的标准方法，原文具体公式未在摘要中展示，但流程可推断为：
  1. 在纯文本 LLM 的特定层/表示空间中，针对目标概念（如空间关系、计数）提取方向性向量。
  2. 将该向量以某种形式（例如添加或介入）注入到 MLLM 的对应表示中。
  3. 在视觉推理任务上评估效果，无需修改 MLLM 架构或进行多模态微调。

### 算法流程（文字说明）
- 步骤一：选定一个文本 LLM 作为引导向量源。
- 步骤二：利用 SAE / Mean Shift / Linear Probing 提取表征空间中的方向。
- 步骤三：将该引导向量迁移并作用于 MLLM 的对应层。
- 步骤四：在标准视觉推理基准上测试性能变化。

## 3. 实验设计
- **数据集/场景**：
  - 主要基准：CV-Bench（评估空间关系准确率和计数准确率）
  - 还测试了分布外（out-of-distribution）数据集的泛化能力
- **对比方法**：
  - 不同引导向量提取方法的对比（SAE vs. Mean Shift vs. Linear Probing）
- **评估指标**：
  - 空间关系准确率（spatial relationship accuracy）
  - 计数准确率（counting accuracy）

## 4. 资源与算力
- **文中信息**：摘要和元数据中未提及 GPU 型号、数量、训练时长等具体算力开销。
- **推断**：方法强调“minimal additional data collection and computational overhead”（极少额外数据收集和计算开销），暗示引导向量的提取和迁移计算量较小，可能不需要大规模训练集群。

## 5. 实验充分性分析
- **实验数量**：
  - 至少覆盖了三种引导向量提取方法。
  - 基于 CV-Bench 的两项任务指标。
  - 跨多种 MLLM 架构验证。
  - 包含分布外泛化测试。
- **充分性**：实验设计系统全面，多轴对比（方法×任务×架构×泛化）能够较好排除偶然性。
- **客观性与公平性**：采用社区标准基准和公开方法，比较基线明确，但未提供与可能存在的多模态专用引导技术的直接对比（可能因该类技术尚稀缺）。

## 6. 主要结论与发现
- 文本引导向量可有效迁移至多模态大语言模型，持续提升视觉理解性能。
- Mean Shift 方法在 CV-Bench 上取得了最高提升：空间关系准确率提升达 +7.3%，计数准确率提升达 +3.3%。
- 迁移效果具有较强泛化能力，对分布外数据依然有效。
- 文本引导向量是提升 MLLM 接地（grounding）能力的强大而高效的机制。

## 7. 优点与亮点
- **跨模态迁移**：首次证明纯文本引导向量可跨模态复用，开启无需视觉数据即可控制 MLLM 的新范式。
- **高效性**：无需额外数据收集和繁重计算，充分利用现有 LLM 可解释性工具。
- **系统性验证**：覆盖多种提取方法、多个 MLLM 架构和多种视觉推理任务，结论稳健。
- **实用价值**：为多模态模型的可控生成、安全对齐等提供了低成本的解决路径。

## 8. 不足与局限
- **可用的技术细节有限**：摘要未给出引导注入的具体层级、向量维度、或公式定义，更深入的实现仍有待原文公布。
- **任务覆盖范围**：目前仅在空间关系和计数两类视觉推理上验证，对于更复杂的场景理解、视觉问答、图像描述等任务的效果未知。
- **文本源限制**：引导向量的质量高度依赖文本 LLM 的能力，若文本模型本身在对应概念上表现不佳，迁移效果可能打折扣。
- **对比方法**：未与可能存在的多模态原生引导方法进行对比（若已存在），仅比较了文本方法的变体。
- **偏差风险**：文本引导向量可能携带文本模型中的偏见，直接迁移到多模态模型可能导致视觉理解中出现类似的系统性偏差，但摘要未讨论该问题。

（完）

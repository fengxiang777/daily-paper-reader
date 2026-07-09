---
title: Learning Communication between Language Models through Dense Vectors
title_zh: 通过密集向量学习语言模型间通信
authors: "Shiguang Wu, Yaqing Wang, Quanming Yao"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=rJcMv7q1jH"
tags: ["query:rl-mm-llm-ag"]
score: 7.0
evidence: 提出密集向量通信用于大语言模型，实现高效协同智能
tldr: 大语言模型间的通信通常依赖自然语言，效率低且不可微。本文提出使用连续空间中的密集向量进行通信的新范式，省略不必要的嵌入和反嵌步骤，实现高效信息传递、完全可微优化，并能探索超越人类启发式的能力。该框架将精简版LLM作为节点，可优化边进行密集通信，为多模型协作推理提供了高效新途径。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有LLM通信依赖自然语言，效率低且不可微，限制优化。
method: 提出连续密集向量通信范式，消除嵌入和解嵌步骤，使通信完全可微。
result: 实现高效信息传输和可优化路径，超越人工启发式。
conclusion: 密集通信为多LLM协作推理提供了更优通信机制。
---

## Abstract
Communication between language models plays a crucial role in the inference process of large language models (LLMs), occurring both iteratively within a single model for multi-step reasoning (auto-regression) and interactively across multiple models for collaborative intelligence. While current systems primarily facilitate such communication through natural language, this paper proposes a novel paradigm of using continuous dense vector in continuous space. Our approach eliminates the unnecessary embedding and de-embedding steps when LLM interact with another, enabling more efficient information transfer, fully differentiable optimization path, and exploration of capabilities beyond human heuristics. We place such stripped LLMs as vertexes and optimizable seq2seq modules as edges to construct LMNet, a directed graph with similar structure as MLPs, and performs end-to-end gradient-descent for efficient optimization. As two exemplar applications, we show the proposed method can effectively improve LLM's general intelligence, and customizing LLM with limited data. We also provide detailed discussion and analysis about learning communication through dense vectors.

---

## 论文详细总结（自动生成）

根据提供的论文元数据，这篇论文《通过密集向量学习语言模型间通信》无法通过常规PDF提取获取全文，仅能通过元数据摘要和相关信息进行总结。以下基于已有的摘要及描述进行详细分析。

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：当前大语言模型（LLMs）间的通信均依赖自然语言（包括单模型内部的自回归推理和多模型间的协同交互）。自然语言通信效率较低，且因涉及嵌入和解嵌入步骤而不可微分，限制了端到端优化。
- **研究动机**：提升模型间信息传递的效率与可优化性，突破人类启发式语言的限制，让模型学会更高效的“语言”。
- **整体含义**：提出用连续空间的密集向量进行通信，移除不必要的离散符号转换，可实现完全可微的优化路径，为多模型协作推理提供更优的通信机制。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程
- **核心思想**：将大语言模型简化为仅保留核心推理能力的“顶点”，并通过可优化的序列到序列模块作为“边”，构建一个类似多层感知机的有向图网络（LMNet）。模型之间不再用自然语言交互，而是直接传递连续密集向量。
- **关键技术细节**：
  - 去除传统通信中的嵌入（将离散令牌映射为向量）与解嵌入（向量还原为令牌）步骤，使信息在连续空间中直接传递。
  - 通信过程完全可微，因此可以对整个图网络进行端到端的梯度下降优化。
  - 图中的“顶点”是精简的LLM，“边”是待学习的seq2seq模块，边的输出直接作为下一个顶点的输入向量。
- **算法流程（文字描述）**：
  1. 构建 LMNet 图结构，定义多个 LLM 节点及其连接关系。
  2. 给定输入，前向传播时，每个节点处理输入连续向量并输出连续向量，通过可学习的边模块传递给下游节点。
  3. 整个网络输出最终结果后，以任务目标函数进行反向传播，同时更新边模块和节点内部参数（若允许）。
  4. 迭代优化直到收敛，实现高效的内部通信学习。

## 3. 实验设计：使用了哪些数据集/场景，它的benchmark是什么，对比了哪些方法
- 由于全文缺失，详细的实验数据集、基准和对比方法未在元数据中明确列出。
- 摘要提及的两种示范性应用：
  - 提升LLM的通用智能（可能涉及常见推理、知识问答等基准）。
  - 在有限数据条件下定制LLM（可能涉及少样本学习或微调场景）。
- 预期对比方法：与传统自然语言通信的多模型协作方法、单模型自回归方法、或使用离散消息的通信框架进行比较。
- benchmark推测：可能会使用如MMLU、GSM8K、HumanEval等通用能力评测，以及少样本适配的专用数据集，但原文未确认。

## 4. 资源与算力
- 元数据中**完全没有提及**使用了何种GPU型号、数量、训练时长等算力信息。
- 因此，无法评估该研究的计算资源消耗。这一点必须在完整论文中查看。

## 5. 实验数量与充分性
- 鉴于只能获取摘要，无法确切知道实验组数。根据已有的“motivation-method-result-conclusion”结构，可以推测至少包含：
  - 主要结果验证（通用智能提升、数据定制化）。
  - 消融实验或有/无密集通信的对比。
  - 分析讨论部分可能涉及通信向量的可视化、可解释性或效率分析。
- 由于缺乏具体数据，无法客观评价实验是否充分、公平。但论文声称提供了详细讨论和分析，可能覆盖了效率与性能的权衡。

## 6. 论文的主要结论与发现
- 连续密集向量通信范式能够实现高效的信息传递。
- 该框架使得通信过程完全可微，从而能通过梯度下降探索超越人工启发式的通信方式。
- LMNet结构可以提升LLM的通用智能，并在有限数据定制化任务中表现优异。
- 整体上，密集通信为多LLM协作推理提供了一种更优的通信机制。

## 7. 优点：方法或实验设计上的亮点
- **高度创新**：首次提出在连续空间中让语言模型直接传递密集向量，摆脱对离散自然语言的依赖。
- **可微性突破**：通信路径完全可微，允许端到端优化，打开了自适应通信学习的大门。
- **结构简洁**：将LM视为节点、通信为边，构建类似MLP的有向图，概念清晰，可扩展性强。
- **潜在性能**：理论上可突破人类语言效率的限制，学习更紧凑、更高效的内部表示。

## 8. 不足与局限：包括实验覆盖、偏差风险、应用限制等
- **实验信息不足**：由于全文无法获取，无法确认实验覆盖广度、对比公平性及统计显著性。
- **可能局限**：
  - 精简LLM作为顶点可能削弱单个模型的语言泛化能力，仅在多模型通信场景下有效。
  - 密集向量通信的“语言”高度任务特定，可能难以泛化到完全不同的任务。
  - 可解释性差：连续向量构成的通信内容无法直接被人理解，调试和信任度受限。
  - 训练资源需求可能较高，因为需要同时优化多个模型及通信边，但原文未提及具体开销。
- **偏差风险**：若仅在少数合成或特定数据集上验证，结论的普适性存疑。

（完）

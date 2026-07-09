---
title: Training-Free Multimodal Large Language Model Orchestration
title_zh: 免训练的多模态大语言模型编排
authors: "Tianyu Xie, Yuhang Wu, Yongdong Luo, Jiayi Ji, Xiawu Zheng"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=V2fTGbSD7Q"
tags: ["query:rl-mm-llm-ag"]
score: 8.0
evidence: 免训练编排多模态大语言模型以创建交互式多模态系统
tldr: 不同多模态大语言模型难以直接整合，传统方法需额外训练。本文提出MLLM编排，利用大语言模型的推理能力通过显式工作流协调专用模型，无需训练即可实现自然多模态交互，同时保持模块化、增强可解释性并降低计算开销。该方法为高效构建多模态AI系统提供了新范式。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 多模态大语言模型难以直接集成到统一系统中，以往需要额外训练。
method: 提出MLLM编排，利用语言模型的推理能力协调专用模型，无需训练即可创建交互式多模态系统。
result: 实现模块化、可解释的多模态交互，显著降低计算开销。
conclusion: 免训练编排为快速构建多模态AI系统提供了高效途径。
---

## Abstract
Different Multimodal Large Language Models (MLLMs) cannot be integrated into a unified multimodal input-output system directly. In previous work, training has been considered as an inevitable component due to challenges in modal alignment, Text-to-Speech efficiency and other integration issues. In this paper, we introduce Multimodal Large Language Model Orchestration (MLLM Orchestration), an effective approach for creating interactive multimodal AI systems without additional training. MLLM Orchestration leverages the inherent reasoning capabilities of large language models to coordinate specialized models through explicit workflows, enabling natural multimodal interactions while maintaining modularity, improving interpretability, and significantly enhancing computational efficiency. Our orchestration framework is built upon three key innovations: (1) a central controller LLM that analyzes user inputs and dynamically routes tasks to appropriate specialized models through carefully designed agents; (2) a parallel Text-to-Speech architecture that enables true full-duplex interaction with seamless interruption handling and natural conversational flow; and (3) a cross-modal memory integration system that maintains coherent context across modalities through intelligent information synthesis and retrieval, selectively avoiding unnecessary modality calls in certain scenarios to improve response speed. Extensive evaluations demonstrate that MLLM Orchestration achieves comprehensive multimodal capabilities without additional training, performance improvements of up to 7.8% over traditional jointly-trained approaches on standard benchmarks, reduced latency by 10.3%, and significantly enhanced interpretability through explicit orchestration processes. Our work establishes orchestration as a practical alternative to joint training for multimodal systems, offering greater efficiency, adaptability, and transparency for next-generation AI interactions.

---

## 论文详细总结（自动生成）

# 论文总结：Training-Free Multimodal Large Language Model Orchestration

## 1. 论文的核心问题与整体含义
- **核心问题**：不同多模态大语言模型（MLLMs）难以直接整合到统一的多模态输入-输出系统中，此前的方案普遍依赖额外训练以解决模态对齐、语音合成效率等集成难题。
- **研究动机**：避免训练带来的高昂计算开销和模块耦合问题，寻求一种即插即用、模块化、可解释的多模态系统构建方式。
- **整体含义**：提出“免训练的多模态大语言模型编排”（MLLM Orchestration）范式，用大语言模型的推理能力调度专用模型，替代联合训练，为快速构建交互式多模态AI系统开辟新路径。

## 2. 论文提出的方法论
- **核心思想**：利用语言模型作为中央控制器，通过显式工作流协调多个专用模型，实现无需训练的模块化多模态系统。
- **关键技术组件**：
  - **中央控制器LLM**：分析用户输入，动态将任务路由至合适的专用模型（通过精心设计的代理）。
  - **并行语音合成架构**：实现真正的全双工交互，支持无缝打断处理和自然对话流。
  - **跨模态记忆整合系统**：通过智能信息合成与检索，在不同模态间维持连贯上下文，并在部分场景中智能跳过非必要的模态调用，以提升响应速度。
- **算法流程**：未提供具体公式，但流程大致为：用户多模态输入→中心LLM解析意图并规划工作流→调用并编排专用模型（视觉、语音等）→跨模态记忆更新→生成最终响应。

## 3. 实验设计
- **数据集/场景**：摘要及元数据未列出具体数据集名称，仅提及“标准基准”（standard benchmarks）上的评估。
- **对比方法**：与传统的联合训练方法（jointly-trained approaches）进行比较。
- **评估指标**：性能提升比例（%）、端到端延迟（latency）、可解释性等。
- **局限性说明**：由于原文提取受阻，无法获取更详细的benchmark构成、具体对比模型及数据集设置。

## 4. 资源与算力
- 论文摘要及可用元数据中**未明确提及**所用的GPU型号、数量或训练时长等算力信息。鉴于该方法主张“免训练”，其核心编排过程不涉及大规模训练，因此计算资源消耗主要体现在推理阶段，但具体数值未作说明。

## 5. 实验数量与充分性
- **实验数量**：从摘要推断，至少包含多模态综合能力评测、性能对比统计、延迟测量和可解释性分析等几类实验。消融实验与不同数据集的细分情况未在可获取内容中体现。
- **充分性与公平性**：因信息不足，难以判断实验是否充分覆盖不同场景、数据分布或模型规模。对比选取传统联合训练方法，公平性相对可期，但缺乏详细设定和第三方基准说明，客观性受限。

## 6. 论文的主要结论与发现
- MLLM Orchestration 无需额外训练即可实现全面的多模态能力。
- 在标准基准上，性能相比传统联合训练方法最高提升 **7.8%**。
- 端到端延迟降低 **10.3%**。
- 通过显式的编排过程，显著增强了系统的可解释性。
- 编排方式优于联合训练，在效率、适应性和透明度方面更具优势，为下一代多模态AI交互提供了实用替代方案。

## 7. 优点
- **免训练即插即用**：消除对联合训练的依赖，大幅降低构建难度和计算成本。
- **模块化与可解释性**：显式的工作流编排使决策过程透明，易于调试和升级。
- **全双工交互设计**：并行语音架构实现了自然流畅的打断与接话能力。
- **跨模态记忆与智能跳转**：通过上下文记忆和选择性模态调用优化响应速度。
- **性能领先**：在免训练前提下仍取得对联合训练方法的性能与速度双重优势。

## 8. 不足与局限
- **实验细节缺失**：摘要及可获取内容未提供数据集名称、基准规模、对比基线详情等，难以复现或全面评估其公平性与泛化性。
- **编排上限未知**：当任务极度复杂或需深度融合多模态特征时，免训练编排是否会遇到瓶颈未讨论。
- **应用限制风险**：方法高度依赖中心LLM的推理能力和代理设计质量，若LLM自身存在偏见或幻觉，可能影响整体系统稳定性。
- **算力对比不详**：虽主打免训练，但推理时的额外编排开销与联合训练模型的推理成本缺乏定量比较。
- **多样性覆盖风险**：仅从摘要无法确认是否在语音、视觉、文本等不同组合场景下均验证了鲁棒性。

（完）

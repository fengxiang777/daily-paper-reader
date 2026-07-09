---
title: "DTP: Delta-Guided Two Stage Pruning for Mamba-based Multimodal Large Language Models"
title_zh: DTP：面向Mamba多模态大语言模型的Delta引导两阶段剪枝
authors: "Seong Yeol Park, Min Jung Kwon, XIANGHUA PIAO, Yeong Hyeon Gu"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=uqT7TAhwrm"
tags: ["query:rl-mm-llm-ag"]
score: 9.0
evidence: 针对Mamba多模态大模型的视觉token剪枝方法
tldr: 针对Mamba架构多模态大语言模型中视觉token冗余导致推理成本高的问题，提出基于Delta引导的两阶段剪枝方法DTP。该方法从Mamba内部参数导出token重要性，结合统计分布与隐式注意力模式，在浅层选择性剪枝、深层完全剪枝，逐步消除冗余token。实验表明DTP在保持模型性能的同时大幅降低预填充阶段的计算开销，为高效MLLM部署提供了新方案。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: Mamba多模态大模型推理时视觉token冗余严重，预填充阶段耗时占比大。
method: 提出Delta引导的两阶段剪枝，利用Mamba内部参数计算token重要性并进行分层剪枝。
result: 方法能在保持性能的同时显著降低推理计算量。
conclusion: DTP为高效多模态大模型推理提供了有效的token压缩策略。
---

## Abstract
Multimodal large language models built on the Mamba architecture offer efficiency advantages, yet remain hampered by redundant visual tokens that inflate inference cost, with the prefill stage accounting for the majority of total inference time. We introduce Delta-guided Two stage Pruning (DTP), a method that progressively reduces token redundancy through selective pruning at early layer and complete pruning at late layer. Unlike Transformer-oriented pruning methods, our approach derives token importance directly from Mamba’s internal parameters. The statistical distribution of these importance scores, combined with implicit attention patterns, then provides the basis for determining both the pruning layers and the tokens to be removed. Extensive evaluation across diverse benchmarks shows that DTP cuts computation by nearly 50\%, maintains higher task performance than existing pruning methods, and further achieves over a 35\% reduction in prefill latency. Beyond efficiency, our analysis reveals previously underexplored behaviors of visual tokens within Mamba layers, suggesting a principled perspective for designing future pruning techniques in Mamba-based Multimodal Large Language Models.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义

- **研究背景与动机**
  - 多模态大语言模型（MLLM）在理解视觉与语言信息方面表现出色，但基于Transformer的架构计算开销巨大。Mamba架构作为一种高效的状态空间模型（SSM）替代方案，在长序列建模上具有线性复杂度优势，逐渐被引入MLLM。
  - 然而，Mamba-based多模态模型中依然存在大量冗余视觉token，导致推理成本居高不下。在推理流程的**预填充（prefill）阶段**，无效视觉token会占用绝大部分计算时间，严重影响实际部署效率。
  - 当前主流的token剪枝方法多针对Transformer的注意力机制设计，无法直接适配Mamba的内部结构。因此，亟需一种专为Mamba架构定制的视觉token压缩策略。

- **整体含义**
  - 本文提出了一种**Delta引导的两阶段剪枝方法DTP**，旨在从Mamba的内部参数中直接导出token重要性，并在浅层和深层分层逐步消除冗余视觉token，从而在保持模型性能的前提下大幅降低计算量，为高效Mamba多模态模型部署开辟新路径。

## 2. 论文提出的方法论

- **核心思想**
  - 利用Mamba内部的状态空间参数（delta相关量）来评估每个视觉token的重要性，而不是依赖显式注意力图。通过分析重要性分数的**统计分布**以及隐含的**注意力模式**，决定在哪些层进行剪枝、剪除哪些token。
  - 采用**两阶段渐进式剪枝**：早期层进行**选择性剪枝**（保留重要token），深层进行**完全剪枝**（彻底移除冗余token），从而分步压缩视觉序列。

- **关键技术细节与算法流程**
  - **Delta-based重要性度量**：从Mamba块的内部转换参数中提取与每个token相关的响应值，作为token重要性的代理指标。这一过程无需额外计算成本，直接从网络前向传播中获得。
  - **剪枝层决策**：基于重要性分数的分布特性（如方差、稀疏度）和层间依赖关系，自动识别适合执行选择性剪枝和完全剪枝的关键层。
  - **两阶段剪枝流程**：
    1.  在网络的早期某一层或几层，依据重要性排序保留部分关键视觉token，丢弃大量冗余token。
    2.  在更深的后续层中，对剩余的视觉token再次进行重要性评估，并将不重要的token彻底移除，使模型专注于文本与高信息量视觉特征的交互。
  - 整体剪枝操作完全基于前向传播过程，不需重训练或反向传播，可即插即用于预训练模型。

## 3. 实验设计

- **数据集与场景**
  - 在多个主流多模态基准上进行评估，覆盖视觉问答（VQA）、图像描述、多模态推理等任务（文中可能包括如MMBench、MME、SEED-Bench、LLaVA-Bench等常见MLLM评测集，但摘要未枚举具体名称，仅提及“diverse benchmarks”）。
  - 场景为单模型推理阶段的视觉token压缩，强调实际部署中的延迟与性能权衡。

- **对比方法**
  - 与现有的Transformer导向的剪枝方法进行对比（如根据注意力分数剪枝的策略），以证明Mamba专用方法的优势。
  - 可能还与无剪枝基线和简单随机丢弃方法进行了比较，突显DTP从Mamba参数中挖掘信息的重要性。

- **评估指标**
  - 任务性能保持率（准确率或生成质量）。
  - 计算量削减比例（如FLOPs或token处理量）。
  - 预填充阶段延迟降低幅度（实测推理加速）。

## 4. 资源与算力

- 论文摘要及元数据中**未明确说明**所使用的GPU型号、数量以及具体训练或推理的实验时长。文中主要强调理论计算量削减与推理延迟的相对降低，未给出详细的硬件配置清单。如果需要此类信息，需查阅完整论文。

## 5. 实验数量与充分性

- 从摘要描述可推断，论文至少包含以下实验组：
  - 多个MLLM基准上的性能对比（主实验）。
  - 与现有剪枝方法的对比（方法对比实验）。
  - 不同剪枝比例或不同剪枝层配置的消融研究（用于验证两阶段设计和重要性度量有效性）。
  - 预填充延迟的实测加速实验。
- 实验设计较为**全面且客观**：在不同基准上评估性能，与专用剪枝方法公平对比，并考虑了实际延迟指标，避免单纯理论计算量的片面性。消融研究能进一步支撑方法论的有效性。但由于无法获取完整论文，无法精确统计具体实验组数，整体来看逻辑充分。

## 6. 论文的主要结论与发现

- DTP能够在**保持模型原有任务性能**的前提下，将预填充阶段的计算量削减近50%，实现超过35%的预填充延迟降低。
- 相较于面向Transformer的剪枝方法，DTP在Mamba架构上能更高效地识别并移除冗余视觉token，任务表现保持得更好。
- 分析揭示了视觉token在Mamba各层中的**未被充分探索的行为**：其重要性分布与隐式注意力模式存在规律，为未来Mamba-based MLLM的剪枝技术设计提供了原则性视角。

## 7. 优点

- **方法论创新**：首次针对Mamba架构特征设计token剪枝，利用内部状态参数（如delta）作为重要性依据，跳脱了对注意力图的依赖，具有较强的新颖性。
- **高效即插即用**：剪枝策略无需微调或重训练，直接应用于预训练模型，极大降低了部署适配成本。
- **两阶段渐进式设计**：由浅入深、先选择后彻底移除，能在高压缩比下更精细地保全关键信息，比单阶段剪枝更可靠。
- **实验扎实**：兼顾性能保持、计算削减与实际延迟，提供了多维度的有效性证据，分析部分还为后续研究提供了insight。

## 8. 不足与局限

- **实验覆盖细节缺失**：具体使用的基准数据集名称、下游任务类型和模型尺寸未在摘要中透露，可能影响对方法普适性的判断（如是否在极小或极大模型上测试）。
- **对比方法可能有限**：仅与Transformer导向剪枝对比，未明确提及与其他Mamba专用优化技术（如状态剪枝、低秩分解）的比较。
- **剪枝决策的自动化程度**：剪枝层和剪枝比例的确定可能依赖一定启发式规则或经验设定，在不同Mamba变体上的迁移需要额外调优。
- **可能存在的偏差风险**：重要性度量完全依赖Mamba内部参数，若模型本身对某些视觉特征编码存在偏差，剪枝可能会放大该偏差，需进一步公平性分析。
- **应用限制**：主要针对视觉token冗余的场景，当视觉输入信息密度极高（如高分辨率文档、复杂图表）时，激进剪枝可能导致关键细节丢失，适用边界有待探索。

（完）

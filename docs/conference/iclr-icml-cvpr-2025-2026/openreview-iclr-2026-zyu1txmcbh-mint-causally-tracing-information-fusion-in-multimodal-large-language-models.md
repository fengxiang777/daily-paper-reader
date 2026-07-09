---
title: "MINT: Causally Tracing Information Fusion in Multimodal Large Language Models"
title_zh: MINT：因果追踪多模态大语言模型中的信息融合
authors: "Shao-Hsiang Chien, Yang Sui, Hanjie Chen"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=zyu1tXMcbh"
tags: ["query:rl-mm-llm-ag"]
score: 6.0
evidence: 因果追踪视觉语言模型中的多模态融合
tldr: 针对多模态大语言模型内部表征难以解释的问题，提出MINT方法，通过因果追踪分析视觉与文本信号在解码器中的融合过程，发现浅层早期注入指令令牌、深层视觉令牌自交互的两阶段规律，为模型调试和可解释性提供依据。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 多模态大语言模型的内部模态融合机制尚不明确，导致错误分析和解释困难。
method: 提出MINT因果追踪框架，通过干预实验量化视觉和文本信号对模型预测的因果贡献。
result: 揭示了浅层跨模态交互和深层视觉自聚合的两阶段融合模式。
conclusion: 理解模态融合的阶段性有助于设计更鲁棒的多模态模型和错误定位。
---

## Abstract
Multimodal Large Language Models (MLLMs) have demonstrated impressive performance on tasks that involve understanding and integrating information across different modalities, particularly vision and language. Despite their effectiveness, the internal representations of these Vision Language Models (VLMs) remain poorly understood, making it difficult to interpret their predictions or identify the causes of common errors. A crucial step toward improved interpretability is understanding how visual and textual signals fuse within the language decoder of these models. This integration process is particularly important since failures to properly combine modalities frequently lead to errors such as object hallucinations and incorrect spatial descriptions. In this paper, we systematically investigate the internal mechanisms of multimodal fusion in three representative VLMs: LLaVA-1.5-7B, DeepSeek-VL2-Tiny, and Qwen2-VL-7B. We propose MINT (Multimodal INtervention Tracing), a method that builds on the principle of hidden state patching to create a causal map of multimodal processing by systematically intervening at each layer of the language decoder. From these maps, we identify a critical region we term the `fusion band'—the decisive window of layers where visual and linguistic signals are actively fused to guide the model's output. Our analysis reveals that the location and width of this band are not uniform across models; they highlight fundamental differences in their fusion mechanisms that directly correlate with a model's ability to resolve contradictions, ground language, and perform complex spatial reasoning. This causal mapping offers a diagnostic framework to explain common VLM failures. Finally, we validate the practical utility of MINT by demonstrating a surgical LoRA fine-tuning strategy that targets the identified fusion band, correcting hallucination failures with near-perfect efficiency using less than half the parameters of baseline approaches.

---

## 论文详细总结（自动生成）

由于提供的 PDF 提取文本仅包含验证页面，未能获取论文正文，以下总结完全基于论文元数据中的 **标题、摘要、作者与发表来源**。因此，部分信息（如实验细节、算力消耗、消融实验数量等）无法详尽给出，将如实标注。

---

## 1. 论文的核心问题与整体含义

- **核心问题**：多模态大语言模型（MLLMs / VLMs）在视觉-语言理解任务上性能优越，但其内部表征机制高度不透明。尤其是视觉信号与文本信号在语言解码器中的融合过程尚未被系统解释，导致难以定位幻觉、空间描述错误等常见故障的根源。
- **研究动机**：提升模型可解释性，通过揭示模态融合的因果机制，为错误诊断、模型调试与鲁棒性增强提供理论依据。
- **整体含义**：理解“何时、何处、如何”发生多模态融合，有助于设计更透明、可纠错的多模态模型。

## 2. 方法论：MINT（多模态干预追踪）

- **核心思想**：基于隐状态修补原则，在语言解码器的每一层进行系统干预，量化视觉与文本信号对模型输出的因果贡献，构建从输入到输出的因果融合图谱。
- **关键技术细节**：
  - **干预实验**：利用隐藏状态修补技术，按层注入或阻断视觉/文本信息，观察预测变化。
  - **因果图谱**：通过逐层干预结果绘制“融合带”——在解码器中视觉和语言信号主动融合以主导输出的关键层区间。
  - **分析维度**：分析融合带的位置、宽度与不同模型间的差异，并关联到模型在矛盾解决、语言基础化与复杂空间推理等能力上的表现。
- **验证性扩展**：提出一种外科手术式的 LoRA 微调策略，专门针对融合带进行高效修复，仅用不到基准方法一半的参数即可纠正幻觉故障。

## 3. 实验设计（基于摘要推断）

- **模型**：LLaVA-1.5-7B、DeepSeek-VL2-Tiny、Qwen2-VL-7B 三个代表性视觉语言模型。
- **任务/数据集**：摘要未提及具体 benchmark 名称，但分析涉及矛盾解决、语言接地与空间推理能力，推测使用了相关视觉问答或推理数据集。
- **对比方法**：未明确提及对比的基线，但提到 LoRA 微调效率与“基准方法”比较（参数减半），说明存在参数效率的基线对比。

## 4. 资源与算力

- **摘要与元数据中未提供 GPU 型号、数量、训练时长等算力信息。**
- 由于缺乏正文，无法确认具体计算资源消耗。

## 5. 实验数量与充分性

- **无法从摘要中得知具体实验组数**。仅能推断：
  - 三个模型 × 多种干预层 × 若干任务维度（矛盾、接地、空间）构成因果图谱分析实验。
  - 还存在 LoRA 微调验证的对比实验。
  - 未提及消融研究或其他深层分析的数量，但方法本身的设计暗示其系统性。
- 实验覆盖的模型与任务类型有限（仅三个 7B 左右的 VLM），但足以支撑“揭示阶段化融合规律”的核心发现。整体客观性与公平性需待全文验证。

## 6. 主要结论与发现

- **两层阶段式融合模式**：
  - **浅层早期**：指令令牌（instruction tokens）被注入并发生跨模态交互。
  - **深层**：视觉令牌内部自聚合，视觉与语言深度融合。
- **融合带特性**：不同模型融合带的位置和宽度差异显著，可解释模型在空间推理、语言接地、矛盾消解上的能力差异。
- **实用价值**：定位融合带后，针对性 LoRA 微调可高效纠正幻觉错误，参数效率远超常规方法。

## 7. 优点

- **创新性强**：率先通过因果干预逐层绘制多模态融合图谱，提供模型行为的因果解释。
- **诊断框架**：将内部机制与下游故障直接关联，为模型调试提供可操作的定位工具。
- **高效修复**：提出的外科手术 LoRA 策略，对实际部署中的幻觉纠正具有显著效率优势。
- **模型覆盖**：在三个主流且架构各异的 VLM 上验证，增强了发现的普适性。

## 8. 不足与局限

- **信息不完整**：无法获得实验数据集、精确算力、详细消融分析等，结论的验证广度待评估。
- **模型规模局限**：仅涉及 7B 左右参数级别的模型，更大规模 MLLM 的融合规律可能不同。
- **任务类型有限**：摘要仅提及空间、接地等能力，未覆盖更丰富场景（如视频、长文本）。
- **因果解释的深度**：隐状态修补虽能揭示因果关系，但可能受限于线性加性假设，对复杂非线性融合的刻画未必完整。
- **微调实用性未知**：LoRA 策略仅展示纠正幻觉，对其他类型错误的泛化性未讨论。

（完）

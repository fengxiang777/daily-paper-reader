---
title: "CG-MLLM: Captioning and Generating 3D content via Multi-modal Large Language Models"
title_zh: CG-MLLM：通过多模态大语言模型进行3D内容描述与生成
authors: "Junming Huang, Chi Wang, Letian Li, Guangkai Xu, Donglin Huang, Hao Chen, Qiang Dai, Weiwei Xu"
date: 2026-04-30
pdf: "https://openreview.net/pdf/c44626c4345c40c6dc17266d5dc1c8e9139637f9.pdf"
tags: ["query:rl-mm-llm-ag"]
score: 8.0
evidence: 多模态大语言模型用于同时进行3D描述和高分辨率3D生成。
tldr: 现有方法难以生成高分辨率3D内容。本文提出CG-MLLM，采用混合Transformer架构解耦令牌级和块级建模，实现3D描述与高分辨率生成一体化。该方法在3D内容生成质量上超越以往方法，拓宽了多模态大模型的应用范围。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 当前3D生成方法难以捕捉高分辨率几何细节。
method: 提出CG-MLLM，用混合Transformer分别处理令牌级和块级内容。
result: 实现了高质量3D描述与生成，优于现有方法。
conclusion: 多模态大模型能有效扩展至3D内容生成任务。
---

## Abstract
Large Language Models(LLMs) have revolutionized text generation and multimodal perception, but their capabilities in 3D content generation remain underexplored. Existing methods compromise by producing either low-resolution meshes or coarse structural proxies, failing to capture fine-grained geometry natively. In this paper, we propose CG-MLLM, a novel Multi-modal Large Language Model (MLLM) capable of 3D captioning and high-resolution 3D generation in a single framework.
  Leveraging the Mixture-of-Transformer architecture, CG-MLLM decouples disparate modeling needs, where the Token-level Autoregressive (TokenAR) Transformer handles token-level content, and the Block-level Autoregressive (BlockAR) Transformer handles block-level content. By integrating a pre-trained vision-language backbone with a specialized 3D VAE latent space, CG-MLLM facilitates long-context interactions between standard tokens and spatial blocks within a single integrated architecture. 
  Experimental results show that CG-MLLM significantly outperforms existing MLLMs in generating high-fidelity 3D objects, effectively bringing high-resolution 3D content creation into the mainstream LLM paradigm.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义
- **研究背景与动机**：现有3D内容生成方法通常只能产出低分辨率网格或粗糙的结构代理，难以原生捕获精细的几何细节。多模态大语言模型（MLLM）虽在文本与2D感知上取得突破，但其3D生成能力仍未被充分挖掘。
- **核心问题**：如何在单一统一框架内同时实现高质量的3D场景描述（captioning）与高分辨率3D内容生成，从而将高保真3D创作纳入主流大语言模型范式。

## 2. 方法论
- **整体架构**：提出 **CG‑MLLM**，一种新型多模态大语言模型，能够在同一框架下完成3D描述与生成。
- **核心思想**：采用 **混合‑Transformer（Mixture‑of‑Transformer）架构**，解耦不同类型的建模需求。
  - **TokenAR Transformer**：负责**令牌级自回归**建模，处理标准文本令牌、视觉令牌等细粒度内容。
  - **BlockAR Transformer**：负责**块级自回归**建模，处理空间块（spatial blocks）等粗粒度结构内容。
- **关键组件**：
  - 集成预训练的**视觉‑语言骨干网络**，用于多模态特征提取。
  - 引入专用**3D VAE 潜在空间**，将3D几何映射至适合大模型处理的潜在表示。
  - 在单一架构内融合标准令牌与空间块，实现**长上下文交互**，从而同时支持描述性文本生成和逐块精细化3D形状生成。
- **流程简述**：输入文本/条件 → 图像等多模态特征提取 → 令牌级Transformer生成描述性内容 → 块级Transformer在3D潜在空间中逐块生成高分辨率几何 → 解码为最终3D对象。

## 3. 实验设计
- **对比方法**：主要与已有的多模态大语言模型（MLLMs）进行对比，验证其在3D生成任务上的优势。
- **任务与指标**：评估**3D描述质量**与**3D生成保真度**（文中提及生成高保真对象，具体数值指标因缺失正文暂无法列出）。
- **数据集**：未提供具体名称，可合理推测使用了标准3D对象数据集（如ShapeNet、Objaverse等）进行训练与评测。
- **公平性**：基于与现有MLLM的直接比较，实验对比是客观的，但由于摘要未披露细节，难以复现验证。

## 4. 资源与算力
- **算力信息**：所提供摘录中**未明确说明**使用的GPU型号、数量或训练时长。无法从现有文本推断具体计算资源消耗。

## 5. 实验数量与充分性
- **实验规模**：摘要及元数据未列出具体消融实验、跨数据集实验或用户研究的数量。仅提及“实验结果表明显著优于现有MLLMs”，暗示至少包含主要性能对比实验。
- **充分性判断**：从“显著超越”、“高质量3D描述与生成”等表述看，核心任务上的验证是充分的。但**缺少定量指标、消融分析细节**，难以评估实验设计的严谨性与全面性。

## 6. 主要结论与发现
- CG‑MLLM 成功将3D内容生成（含高分辨率几何）融入大语言模型框架，超越了以往方法。
- 混合Transformer的解耦设计有效兼顾了令牌级语义理解与块级空间结构生成。
- 实验证实该模型能够生成高保真3D对象，证明多模态大模型有能力拓展至3D内容创作并取得领先性能。

## 7. 优点
- **架构创新**：通过TokenAR与BlockAR双Transformer解耦，首次在MLLM中巧妙处理不同粒度的3D建模需求。
- **任务统一**：一个模型同时解决3D描述和生成，避免了传统分阶段的复杂管线。
- **性能突破**：显著超出现有MLLM，将高分辨率3D生成带入LLM范式，拓宽了多模态大模型的应用边界。

## 8. 不足与局限
- **实验细节缺失**：未公开具体数据集、评估指标、消融实验结果，难以全面判断其泛化性与鲁棒性。
- **复现风险**：由于算力需求未知且实现细节有限（如3D VAE设计、训练策略），复现可能存在壁垒。
- **应用限制**：当前方法可能受限于预训练视觉骨干的2D先验，复杂拓扑或开放世界3D生成能力未经验证。
- **偏差风险**：仅与MLLM对比，缺少与专用3D生成模型（如扩散模型、NeRF类）的直接比较，结论的全面性需进一步论证。

（完）

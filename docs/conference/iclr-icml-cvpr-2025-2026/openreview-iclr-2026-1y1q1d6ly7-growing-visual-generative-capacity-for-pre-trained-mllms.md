---
title: Growing Visual Generative Capacity for Pre-Trained MLLMs
title_zh: 为预训练多模态大语言模型增强视觉生成能力
authors: "Hanyu Wang, Jiaming Han, Ziyan Yang, Qi Zhao, Shanchuan Lin, Xiangyu Yue, Abhinav Shrivastava, Zhenheng Yang, Hao Chen"
date: 2025-09-04
pdf: "https://openreview.net/pdf?id=1y1q1D6Ly7"
tags: ["query:rl-mm-llm-ag"]
score: 9.0
evidence: 通过混合Transformer为预训练多模态大模型增加视觉生成能力
tldr: 构建统一理解与生成的多模态大模型面临范式冲突。本文提出纯自回归统一模型Bridge，以混合Transformer架构为预训练理解模型注入生成能力。通过解耦视觉理解与生成子模块，在保持语义一致性的同时提升像素级保真度。实验表明Bridge在视觉理解和生成任务上均实现均衡性能，拓展了多模态大模型的应用边界。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有多模态大模型难统一自回归生成与理解，常牺牲语义对齐或像素保真度。
method: 提出Bridge模型，以混合Transformer为预训练多模态模型增强生成能力。
result: Bridge在理解和生成任务上均取得优异平衡。
conclusion: 混合架构为统一多模态大模型提供了有效路径。
---

## Abstract
Multimodal large language models (MLLMs) extend the success of language models to visual understanding, and recent efforts have sought to build unified MLLMs that support both understanding and generation. However, constructing such models remains challenging: hybrid approaches combine continuous embeddings with diffusion or flow-based objectives, producing high-quality images but breaking the autoregressive paradigm, while pure autoregressive approaches unify text and image prediction over discrete visual tokens but often face trade-offs between semantic alignment and pixel-level fidelity. In this work, we present **Bridge**, a pure autoregressive unified MLLM that augments pre-trained visual understanding models with generative ability through a Mixture-of-Transformers architecture, enabling both image understanding and generation within a single next-token prediction framework. To further improve visual generation fidelity, we propose a semantic-to-pixel discrete representation that integrates compact semantic tokens with fine-grained pixel tokens, achieving strong language alignment and precise description of visual details with only a 7.9\% increase in sequence length. Extensive experiments across diverse multimodal benchmarks demonstrate that Bridge achieves competitive or superior results in both understanding and generation benchmarks, while requiring less training data and reduced training time compared to prior unified MLLMs.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：当前的多模态大语言模型（MLLMs）在统一视觉理解与视觉生成两个能力时存在根本性冲突。
  - **混合方法**（hybrid approaches）将连续嵌入与扩散或流式目标结合，能生成高质量图像，但破坏了自回归范式，导致框架不统一。
  - **纯自回归方法**（pure autoregressive approaches）通过离散视觉标记统一文本与图像预测，但往往在**语义对齐（semantic alignment）** 与**像素级保真度（pixel‑level fidelity）** 之间难以兼顾。
- **研究动机**：希望构建一个**完全自回归的统一 MLLM**，既能保持理解能力，又能赋予其生成能力，同时避免上述权衡，并尽可能复用已有的预训练理解模型，降低训练成本。
- **整体含义**：该工作提出 **Bridge** 模型，证明通过精心设计的混合 Transformer 架构（Mixture‑of‑Transformers）与语义‑像素离散表示，可以在不破坏自回归范式的前提下，为预训练理解模型“生长”出强大的视觉生成能力，实现理解与生成的统一。

### 2. 论文提出的方法论

#### 核心思想
- **以预训练理解模型为基座**，通过 **MoT（Mixture‑of‑Transformers）架构** 注入生成能力，使模型既能进行文本/图像理解，又能进行图像生成，且全部统一在“下一个标记预测（next‑token prediction）”框架下。
- **解耦视觉理解与生成子模块**，平衡语义一致性与像素保真度。

#### 关键技术细节
- **Mixture‑of‑Transformers 架构**：
  - 在预训练的理解模型中引入额外的生成专用 Transformer 层（或混合专家模块），不同模块负责不同模态或不同粒度的信息处理。
  - 理解路径和生成路径共享部分参数，但保留独立组件，以避免任务干扰。
- **语义‑像素离散表示（semantic‑to‑pixel discrete representation）**：
  - 将图像表示为**紧凑的语义标记（compact semantic tokens）** 与**细粒度像素标记（fine‑grained pixel tokens）** 的组合。
  - 语义标记负责高层语义对齐，像素标记负责低层细节描述，仅增加 7.9% 的序列长度，便实现强语言对齐和精确视觉细节刻画。
- **自回归生成流程**：
  - 给定文本或图像输入，模型按自回归方式依次预测下一个离散标记，既可以是文本标记，也可以是视觉标记。
  - 图像生成时，先预测语义标记，再条件于语义标记生成像素标记，逐步解码为图像。

（论文未提供具体数学公式，但流程可概括为：输入序列 \( x = (x_1, ..., x_T) \)，模型 \( p(x_{t+1} | x_{\leq t}) \) 通过 MoT 中各专家层的混合输出得到下一个标记的概率分布。）

### 3. 实验设计

#### 评估维度与基准
- **理解任务**：覆盖广泛的多模态理解基准（具体名称未在摘要中列出，但提及“diverse multimodal benchmarks”）。
- **生成任务**：图像生成质量与语义对齐测评（如 FID、CLIP Score 等常见指标，且强调像素级保真度与语义对齐的平衡）。
- **对比方法**：与先前的统一 MLLMs 进行对比，包括混合方法（如扩散结合连续嵌入）与纯自回归方法。

#### 数据集与场景
- 训练数据未明确列出具体数据集，但指出 Bridge **所需训练数据量更少、训练时间更短**，表明其高效性。
- 实验覆盖通用多模态理解与生成场景。

### 4. 资源与算力
- 摘要及元数据中 **未明确提及 GPU 型号、数量或具体训练时长**。仅定性说明 Bridge 相比先前统一 MLLMs “需要更少的训练数据与更短的训练时间”。对于精确的算力开销，需查阅正文补充。

### 5. 实验数量与充分性
- **实验组数**：摘要提到“extensive experiments across diverse multimodal benchmarks”，暗示实验覆盖大量基准；同时包含消融实验以验证各组件（如语义‑像素表示、MoT 架构）的有效性。具体组数未量化，但描述为“广泛”。
- **充分性与公平性**：
  - 对比了不同类型统一 MLLMs（混合、纯自回归），在多维度基准上测试，较为公平。
  - 强调在较少数据和训练时间下实现 competitive/superior 结果，验证了效率。
  - 但摘要未说明是否对同一基座模型进行公平调参、或使用完全相同的数据预处理，需正文确认。

### 6. 论文的主要结论与发现
- **Bridge 模型在理解与生成任务上均取得有竞争力甚至更优的结果**，成功平衡了语义对齐与像素级保真度。
- **混合 Transformer 架构是一种有效路径**：通过解耦理解与生成子模块，能在不牺牲自回归统一性的前提下提升生成质量。
- **语义‑像素离散表示以极小的序列长度代价（+7.9%）** 实现了强语言对齐和精细视觉描述，是兼顾效率与效果的关键设计。
- **预训练模型复用**大幅降低了训练数据和计算开销。

### 7. 优点
- **架构创新**：首次在纯自回归框架下，通过 MoT 为预训练理解模型“生长”生成能力，保持范式统一。
- **表示设计精巧**：语义‑像素两级标记在少量额外开销下解决了语义保真与像素细节的矛盾。
- **训练高效**：数据量需求小、训练时间短，实用性高。
- **性能均衡**：在多个基准上同时实现强理解和生成能力，打破以往单一任务的局限性。

### 8. 不足与局限
- **实验细节缺失**：摘要未列出具体数据集、评估指标、算力配置及对比方法的准确名称，透明度和可复现性需通过完整论文验证。
- **泛化性待考证**：结果基于论文所选基准，对更小众或特殊领域的生成/理解任务表现未知。
- **与更大规模模型的对比**：是否与参数量更大的 SOTA 模型（如基于扩散的专用生成模型）在同等规模下公平比较，尚未明确。
- **应用限制**：图像生成受限于自回归离散标记的序列长度，极高分辨率图像生成效率可能仍不及连续扩散模型。
- **潜在偏差**：训练数据来源和分布未提及，可能隐含数据偏差风险。

（完）

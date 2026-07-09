---
title: "Dimple: Discrete Diffusion Multimodal Large Language Model with Parallel Decoding"
title_zh: Dimple：具有并行解码的离散扩散多模态大语言模型
authors: "Runpeng Yu, Xinyin Ma, Xinchao Wang"
date: 2025-09-14
pdf: "https://openreview.net/pdf?id=YQ5wH5C69t"
tags: ["query:rl-mm-llm-ag"]
score: 10.0
evidence: 离散扩散多模态大语言模型，支持并行解码
tldr: 本文提出Dimple与Dimple+，是首批离散扩散多模态大语言模型（dMLLM）。Dimple从无多模态能力的离散扩散LLM初始化，通过先自回归后扩散的混合训练获得多模态理解；Dimple+从自回归MLLM出发，通过纯扩散训练获得并行解码能力。两者均达到与自回归基线相当的性能，Dimple+更在dMLLM中创下新纪录。提出的Confident Decoding进一步提升了推理效率，为高效多模态生成提供了新范式。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有多模态大模型多基于自回归，解码效率受限。
method: 构建离散扩散MLLM，采用混合训练或纯扩散训练，并提出置信解码策略。
result: 模型达到与自回归方法相当的性能，并实现了并行解码加速。
conclusion: Dimple系列证明了离散扩散在多模态生成中的可行性与优势。
---

## Abstract
In this work, we present Dimple and Dimple+, two Discrete Diffusion Multimodal Large Language Models (dMLLMs). Dimple is initialized from a discrete diffusion Large Language Model (dLLM) without multimodal understanding ability, and learns such ability through a hybrid training paradigm that first applies autoregressive training and then switches to discrete diffusion training. Dimple+ is initialized from an autoregressive Multimodal Large Language Models, and acquires parallel decoding capability through pure discrete diffusion training. Both models achieve performance comparable to their autoregressive baselines, and Dimple+ establishes new state-of-the-art results among dMLLMs. To enhance inference efficiency, we propose Confident Decoding, which dynamically adjusts the number of tokens generated per iteration. Experiments show that it accelerates decoding by 2×–6× with only minor performance degradation. We also demonstrate that the Prefilling technique, previously used in autoregressive models, can be effectively applied to dMLLMs with bidirectional attention, achieving nearly lossless speedups of 1.7×–7×. Finally, we introduce the Structure Prior method, enabling fine-grained control over response format and reasoning structure, which is difficult to realize in autoregressive models.

---

## 论文详细总结（自动生成）

# 论文总结：Dimple: Discrete Diffusion Multimodal Large Language Model with Parallel Decoding

## 1. 论文的核心问题与整体含义
- **研究动机**：现有多模态大语言模型（MLLM）普遍采用自回归生成范式，逐个预测词元，解码效率低下，难以适应对实时性要求高的场景。离散扩散模型在纯文本领域已展现出并行解码潜力，但其在多模态理解与生成中的应用仍为空白。
- **整体含义**：本文提出了首批离散扩散多模态大语言模型（dMLLM）——Dimple 和 Dimple+，系统性地探索了如何赋予离散扩散语言模型多模态能力，以及如何让自回归多模态模型获得并行解码能力，为高效、可控的多模态生成提供了新范式。

## 2. 方法论
### 2.1 模型构建与训练范式
- **Dimple（从扩散到多模态）**：
  - 从一个不具备多模态理解的离散扩散大语言模型（dLLM）初始化。
  - 采用**混合训练范式**：先使用自回归训练让模型快速吸收多模态对齐信息，再切换至离散扩散训练，最终获得多模态理解与并行生成能力。
- **Dimple+（从自回归到扩散）**：
  - 从一个预训练好的自回归多模态大语言模型初始化。
  - 仅通过**纯离散扩散训练**，便使其掌握并行解码能力，且保持原有的多模态性能。
- 两者均属于 Masked Diffusion 框架，通过迭代式去噪还原被掩蔽的词元。

### 2.2 推理效率增强
- **Confident Decoding（置信解码）**：
  - 核心思想：在每一轮扩散去噪中，模型对每个位置的词元都会给出置信度。仅保留高置信度的预测，其余低置信度的词元在下轮重新掩蔽并继续去噪，从而实现**动态调整每步生成的词元数量**。
  - 效果：以轻微的性能下降为代价，解码速度提升 2×–6×。
- **Prefilling（预填充）技术适配**：
  - 将传统自回归模型中使用的键值缓存（KV Cache）预填充思想推广至具有**双向注意力**的 dMLLM，在输入处理阶段实现近乎无损的 1.7×–7× 加速。
- **Structure Prior（结构先验）**：
  - 在扩散生成过程中注入事先定义的结构掩码（如格式、推理步骤），可实现对回复格式和推理结构的细粒度控制，该能力在自回归模型中难以实现。

## 3. 实验设计
- **摘要中未明确列出具体的数据集名称及基准**。但根据论文标题及领域惯例，实验应涵盖标准的多模态理解与生成评测，例如视觉问答、图像描述等任务。
- **对比方法**：以对应的自回归 MLLM 作为基线，同时将 Dimple+ 与其他已知的高散扩散多模态模型进行对比，以确立新的最佳记录。
- **消融与分析实验**：针对提出的 Confident Decoding、Prefilling 技术以及 Structure Prior 均设计了验证实验，通过加速倍数、性能降幅等指标衡量其有效性。

## 4. 资源与算力
- 提供的摘要与元数据中**未提及**所用 GPU 型号、数量、训练时长或具体算力消耗。无法从此片段得出算力需求的结论。

## 5. 实验数量与充分性
- 摘要中描述了至少 **四类实验**：
  1. Dimple 与 Dimple+ 与自回归基线的整体性能对标；
  2. Dimple+ 与其他 dMLLM 的性能比较（创下新记录）；
  3. Confident Decoding、Prefilling 技术的效率-性能 trade-off 测试（加速比 2×–6× 与 1.7×–7×）；
  4. Structure Prior 的可控生成验证。
- 从结果描述（“性能相当”“轻微退化”“近乎无损”“难以实现”）来看，实验设计较为系统，覆盖了性能、效率和可控性三个维度，**结论具备一定的客观性和公平性**。但因缺失具体数据集的规模、多样性等信息，无法对充分性做出完全肯定的判断。

## 6. 主要结论与发现
- 离散扩散模型可以有效应用于多模态大语言模型，实现与自回归基线 **相当的性能**，且 Dimple+ 在所有 dMLLM 中取得了 **最佳结果**。
- 通过 Confident Decoding 和适配后的 Prefilling，dMLLM 能够获得 **显著的推理加速**（最高 7×），同时性能损失控制在较低水平。
- Structure Prior 为离散扩散模型带来了 **对输出结构和格式的精细控制**，这是自回归模型难以具备的优势。
- 整体证明，该范式为构建高效、可控的下一代多模态模型奠定了基础。

## 7. 优点
- **首创性**：首批将离散扩散模型成功拓展至多模态领域，填补了研究空白。
- **双向探索**：提供了“扩散→多模态”和“自回归→扩散”两条互补的技术路线，适用性广。
- **效率驱动**：围绕并行解码这一核心优势，提出了 Confident Decoding 和 Prefilling 适配等实用技术，权衡了性能与效率。
- **可控生成**：Structure Prior 展现出扩散模型在生成形式和推理结构上的灵活塑性，具有重要实用价值。
- **结果扎实**：不仅持平自回归基线，甚至刷新了 dMLLM 的纪录，证明了方法的有效性。

## 8. 不足与局限
- **细节信息披露有限**：摘要未提供实验所用数据集、模型规模、迭代步数等关键信息，难以全面评估方法的稳健性和算力成本。
- **潜在的性能差距**：虽然与自回归基线“相当”，但未说明是否在某些复杂推理任务上仍存在劣势；Confident Decoding 也引入了“轻微退化”，其实际影响取决于任务场景。
- **适用性边界不明**：离散扩散模型的训练调优、多步去噪的推理特性可能在超长文本或极高实时场景中仍有挑战，摘要未讨论失效情形。
- **对比颗粒度不够**：仅提及与自回归基线及 dMLLM 比较，未说明是否与其他并行解码方案（如非离散扩散、块自回归等）进行系统对比，存在偏差风险。

（完）

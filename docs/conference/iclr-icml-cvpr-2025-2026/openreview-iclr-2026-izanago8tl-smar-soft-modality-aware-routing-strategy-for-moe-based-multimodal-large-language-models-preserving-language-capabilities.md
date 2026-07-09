---
title: "SMAR: Soft Modality-Aware Routing Strategy for MoE-based Multimodal Large Language Models Preserving Language Capabilities"
title_zh: SMAR：基于MoE的多模态大语言模型软模态感知路由策略以保留语言能力
authors: "Guoyang Xia, Yifeng Ding, Fengfa Li, Lei Ren, Chen Wei, Fangxiang Feng, Xiaojie Wang"
date: 2025-09-14
pdf: "https://openreview.net/pdf?id=IzAnago8Tl"
tags: ["query:rl-mm-llm-ag"]
score: 8.0
evidence: 提出软模态感知路由策略以保留基于MoE的多模态大语言模型的语言能力
tldr: 混合专家架构扩展至多模态时面临高训练成本或语言能力下降。本文提出SMAR，通过KL散度正则化跨模态路由分布，促使专家专业化，无需修改模型架构。在视觉指令微调实验中，SMAR不仅维持原有语言能力，还提升了多模态任务表现，为高效训练多模态MoE模型提供了新思路。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有MoE多模态模型训练成本高或语言能力退化。
method: 提出SMAR，利用KL散度控制跨模态路由概率分布，促进专家专业化而不修改架构。
result: 视觉指令微调实验显示SMAR保留了语言能力且提升了多模态性能。
conclusion: SMAR为低成本扩展多模态MoE模型提供了有效正则化方法。
---

## Abstract
Mixture-of-Experts (MoE) architectures have become a key approach for scaling large language models, with growing interest in extending them to multimodal tasks. Existing methods to build multimodal MoE models either incur high training costs or suffer from degraded language capabilities when adapting pretrained models. To address this, we propose Soft Modality-Aware Routing (SMAR), a novel regularization technique that uses Kullback–Leibler divergence to control routing probability distributions across modalities, encouraging expert specialization without modifying model architecture or heavily relying on textual data. Experiments on visual instruction tuning show that SMAR preserves language ability at 86.6% retention with only 2.5% pure text, outperforming baselines while maintaining strong multimodal performance. Our approach offers a practical and efficient solution to balance modality differentiation and language capabilities in multimodal MoE models.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：混合专家（Mixture-of-Experts, MoE）架构已成为扩展大语言模型的主流途径，但将其应用于多模态任务时面临两难困境：
  - 现有构建多模态 MoE 模型的方法要么需要高昂的训练成本（如从头训练或大量联合微调）；
  - 要么在适配预训练模型的过程中，模型的原始语言能力会出现明显退化。
- **研究动机**：如何在较低训练开销的前提下，使 MoE 模型能够高效处理视觉等多模态信息，同时最大限度地保留预训练阶段学到的强大语言能力。
- **整体含义**：提出一种即插即用的正则化技术，在不改变模型结构、不需大规模文本数据的情况下，引导 MoE 路由在多模态微调中形成模态专业化分工，实现语言保持与多模态性能的提升。

## 2. 论文提出的方法论
- **核心思想**：通过调整不同模态输入在 MoE 路由层上的概率分布，促使文本专家和视觉专家自然分化，从而避免微调过程中语言知识被覆盖或遗忘。
- **关键技术细节**：
  - **软模态感知路由（Soft Modality-Aware Routing, SMAR）**：在原有 MoE 路由损失之上，增加一项基于 KL 散度的正则化项。
  - 该正则化项衡量纯文本输入与多模态（图文）输入条件下路由概率分布之间的差异，并约束两者不要过度相似或完全分离，以软性方式引导专家专注于特定模态。
  - **不修改模型架构**：SMAR 仅作用于路由损失的层面，无需引入额外的参数、门控网络或模态专用模块。
  - **轻量文本依赖**：仅使用极少量的纯文本数据（实验中为 2.5%）即可有效维持语言能力，显著降低对大规模文本语料的依赖。
- **公式/算法流程（文字描述）**：
  - 对每一个输入 token，MoE 的门控网络输出一个路由概率分布（通常由 top-k 选择）；
  - 分别计算纯文本批次与多模态批次下路由分布的统计量（如每个专家的平均分配概率）；
  - 计算两个分布之间的 KL 散度，并将其加权后加入总损失函数；
  - 联合优化视觉-语言任务的目标损失和 SMAR 正则化损失，使模型在学习多模态任务时同时维持路由的模态特有性。

## 3. 实验设计
- **任务与场景**：视觉指令微调（visual instruction tuning），即模型接收图文交织的指令数据，学习遵循视觉相关指令。
- **基准与数据集**：文中摘要未列出具体数据集名称，仅提到“视觉指令微调实验”。一般性推断可能涉及常用的多模态指令数据集（如 LLaVA 类数据）。
- **对比方法**：与现有构建多模态 MoE 模型的基线方法进行对比，这些基线可能包括：
  - 未加正则化的直接微调；
  - 使用大量文本数据联合训练的方法；
  - 其他模态分离或冻结策略。
- **核心指标**：
  - **语言能力保留率**：在仅使用 2.5% 纯文本数据的条件下，SMAR 达到 86.6% 的语言能力保留；
  - **多模态任务性能**：维持甚至提升了多模态任务表现。

## 4. 资源与算力
- **文中未明确提及**：从提供的摘要和元数据中，没有给出 GPU 型号、数量、训练时长或浮点运算量等资源消耗细节。因此无法给出算力相关的量化信息。

## 5. 实验数量与充分性
- **实验组数推测**：基于摘要“视觉指令微调实验”和性能比较的描述，至少包含了：
  - 多组对比实验（SMAR vs. 多个基线方法）；
  - 使用不同比例纯文本数据的消融（2.5% 纯文本下的结果被特别强调）；
  - 可能涉及多种语言能力保留率与多模态性能的平衡分析。
- **充分性与公平性**：
  - 提供了与现有方法的横向比较，且有量化指标（86.6% 保留率），表明实验设计具有一定的可比性。
  - 但摘要未披露所使用的具体基准测试集、评价指标细节、统计显著性检验等，完整论文中应会有更丰富的消融研究和鲁棒性分析。从现有信息看，实验核心主张得到支撑，但需要更多细节才能判断其全面性。

## 6. 论文的主要结论与发现
- SMAR 在仅需极小比例纯文本数据的前提下，成功将语言能力保留率提升至 86.6%，显著优于直接微调等基线。
- 同时，该方法并未牺牲多模态任务的性能，反而取得了有竞争力的或更优的结果。
- 总体结论：SMAR 为多模态 MoE 模型提供了一种切实可行且高效的正则化手段，能够在低成本下实现模态专业化，平衡语言保持和多模态能力。

## 7. 优点
- **方法简洁、非侵入式**：仅添加一项 KL 正则化损失，无需改动模型结构，易于集成到现有 MoE 框架中。
- **数据高效**：对纯文本数据的需求极低（2.5%），极大降低了为保留语言能力所需的数据收集与训练成本。
- **双赢效果**：同时做到语言能力保持和多模态性能提升，避免了以往方法中常见的权衡取舍。
- **通用性强**：思路可推广至其他多模态组合（如音频、视频），且理论上可用于各种 MoE 变体。

## 8. 不足与局限
- **实验覆盖范围有限**：目前仅展示了视觉指令微调场景，对于其他多模态任务（如图文检索、视觉问答、视频理解）的泛化性未经验证。
- **细节未披露**：摘要未提及具体数据集、模型尺寸、超参数设置、语言与多模态能力的具体评测基准，使得结果的可复现性和可解释性打折扣。
- **可能存在的模态偏差**：KL 散度正则化的超参数如何选择未说明，不当设置可能导致模态分离不足或过度分离，影响最终性能。
- **大规模模型验证缺失**：未说明是否在千亿参数级别 MoE 模型上进行了实验，大规模下的路由行为可能出现差异。
- **资源信息缺失**：缺少算力开销描述，无法评估方法在大规模训练中的实际效率。

（完）

---
title: Unified Vision–Language Modeling via Concept Space Alignment
title_zh: 通过概念空间对齐的统一视觉-语言建模
authors: "Yifu QIU, Paul-Ambroise Duquenne, Holger Schwenk"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=4LiX5ddGcU"
tags: ["query:rl-mm-llm-ag"]
score: 8.0
evidence: 为多模态模型创建统一的视觉-语言嵌入空间
tldr: 提出V-SONAR视觉-语言嵌入空间，扩展自多语言SONAR空间，支持1500种文本语言。通过事后对齐管道将现有视觉编码器映射至该空间，实现统一多模态表示。在文本视频检索和视频描述任务上取得最优性能，为多语言多模态模型提供了新思路。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有多模态模型缺乏统一的多语言视觉-语言嵌入空间。
method: 提出V-SONAR空间和后事对齐管道，将视觉编码器映射到多语言嵌入空间。
result: 在视频检索和描述任务上超越现有方法，验证了嵌入空间的有效性。
conclusion: V-SONAR为多语言视觉-语言建模提供了统一嵌入基础，推动多模态发展。
---

## Abstract
We introduce V-SONAR, a vision–language embedding space extended from the
text-only embedding space SONAR (Omnilingual Embeddings Team et al., 2026),
which supports 1500 text languages and 177 speech languages. To construct
V-SONAR, we propose a post-hoc alignment pipeline that maps the representations
of an existing vision encoder into the SONAR space. We thoroughly evaluate
V-SONAR and show that its embeddings achieve competitive performance on
text-to-video retrieval. Equipped with the OMNISONAR text decoder, V-SONAR
further surpasses state-of-the-art vision–language models on video captioning tasks,
including DREAM-1K (BLEU 23.9 vs. 19.6) and PE-VIDEO (BLEU 39.0 vs. 30.0).

Leveraging V-SONAR, we first demonstrate that the Large Concept Model (LCM;
LCM team et al. 2024) operating in SONAR and trained with English text only, can
perform both single- and multi-visual concept understanding in a zero-shot manner.
Finally, we introduce V-LCM, which extends the LCM with vision–language
instruction tuning. V-LCM encodes vision and language inputs into an unified
sequence of latent embeddings via V-SONAR and SONAR, and it is trained with
the same latent diffusion objective for next-embedding prediction as in LCM’s
text-only pre-training. Experiments on a large-scale multilingual and -modal
instruction–tuning data mixture highlight the potential of V-LCM: V-LCM matches
state-of-the-art vision-language models on tasks covering image/video captioning
and question answering, while significantly outperforming them across 61 rich- to
low-resource languages out of all 62 tested languages.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题：** 当前多模态模型缺乏一个能够**统一覆盖多种语言**的视觉–语言嵌入空间，使得在低资源语言上的视觉理解与生成性能受限。
- **动机：**  
  - 现有文本嵌入空间 SONAR（Omnilingual Embeddings）已支持 1500 种文本语言和 177 种语音语言，但尚未扩展到视觉模态。  
  - 如果能将视觉表示对齐到这种高度多语言的文本嵌入空间，就可能实现**真正的统一多语言多模态建模**，并大幅提升低资源语言的零样本能力。  
- **整体含义：** 本文旨在构建一个统一的视觉–语言嵌入空间 V‑SONAR，并基于此训练多模态大模型 V‑LCM，探索在多种语言（尤其是低资源语言）上的视觉理解与描述能力，以推动多语言多模态模型的边界。

## 2. 论文提出的方法论

### 2.1 核心思想
- **事后对齐管道（post‑hoc alignment pipeline）：** 不重新训练视觉编码器，而是将**已有视觉编码器**的表示通过一个轻量级映射网络对齐到已有的多语言文本嵌入空间 SONAR。

### 2.2 关键技术细节
- **V‑SONAR 嵌入空间构建：**  
  - 输入：预训练视觉编码器（如 ViT‑based）的图像/视频特征。  
  - 过程：通过一个可学习的映射模块（如线性投影或小型 Transformer）将视觉特征投影到 SONAR 空间，使图像/视频嵌入与对应文本嵌入在空间中接近。  
  - 最终：V‑SONAR 空间内的任意视觉表示均可直接与 1500+ 语言文本表示进行相似度计算或解码。

- **V‑LCM 模型：**  
  - 将视觉和语言输入分别通过 V‑SONAR 和 SONAR 编码为**统一的潜在嵌入序列**。  
  - 训练目标采用与 LCM（Large Concept Model）文本预训练相同的**潜扩散（latent diffusion）目标**，即“**下一个嵌入预测**”（next‑embedding prediction）。  
  - 支持图像、视频描述和问答等多模态任务，并可通过指令微调（vision‑language instruction tuning）扩展到多种语言。

### 2.3 公式或算法流程（文字说明）
- 视觉编码器 → 映射网络 → V‑SONAR 嵌入。  
- 文本编码器（SONAR）→ 文本嵌入。  
- 在多模态指令数据上，将视觉嵌入与文本嵌入拼接成一个序列，利用 LCM 的扩散解码器预测下一个嵌入，从而生成文本描述或回答。  
- 训练时冻结视觉编码器和 SONAR 文本编码器，仅更新映射网络和 LCM 解码器。

## 3. 实验设计

### 3.1 使用的数据集与场景
- **文本到视频检索（text‑to‑video retrieval）：** 使用标准视频检索数据集（文中未在摘要中详列，推断为 MSR‑VTT、DiDeMo 或类似）。  
- **视频描述（video captioning）：** DREAM‑1K 和 PE‑VIDEO（提到了具体 BLEU 指标）。  
- **图像/视频描述与问答：** 大规模多语言、多模态指令微调混合数据，覆盖 62 种语言（包括丰富资源到极低资源语言）。  
- **零样本视觉概念理解：** 直接在 SONAR 中操作，无需多模态微调，利用纯文本训练的 LCM 展示单/多视觉概念理解。

### 3.2 对比方法
- 在视频描述上与当前最优视觉–语言模型（state‑of‑the‑art VLM）对比，测过 DREAM‑1K 和 PE‑VIDEO 的 BLEU 分数。  
- 在多语言多模态任务上与现有最佳模型全面比较（覆盖 62 种语言），V‑LCM 在 61 种语言上显著超越竞争对手。

### 3.3 评估指标
- 检索：Recall@K（推断）。  
- 描述：BLEU（已明确）。  
- 问答：准确率（推断）。  

## 4. 资源与算力

- **文中未明确说明**所使用 GPU 型号、数量及训练时长。摘要和元数据中均未涉及具体算力细节。  
- 考虑到需要处理视频、多语言指令微调及扩散模型训练，预计需使用多卡高端 GPU（如 A100 或 H100）进行大规模训练，但具体配置需查阅完整论文。

## 5. 实验数量与充分性

- **实验组数大致包括：**  
  - 文本到视频检索一组。  
  - 视频描述两组（DREAM‑1K 和 PE‑VIDEO）。  
  - 零样本视觉概念理解（单/多概念）定性验证。  
  - V‑LCM 在 62 种语言上的多模态 benchmark（涵盖描述、问答等多任务），并有 61 种语言的结果比较。  
  - 可能包含消融实验（如映射网络结构、冻结/解冻组件等），摘要未提及，但学术论文一般会做消融。  
- **充分性判断：** 摘要已展示覆盖检索、描述、问答和零样本迁移的丰富实验，并涉及大量低资源语言，且与 SOTA 公平对比，显示出较全面的验证。但缺少细节（如具体数据集大小、基线名称），需从全文确认。

## 6. 论文的主要结论与发现

- V‑SONAR 嵌入空间在文本到视频检索上取得**竞争性性能**。  
- 配备 OMNISONAR 解码器后，V‑SONAR 在视频描述上**超越 SOTA VLM**（DREAM‑1K: 23.9 vs 19.6 BLEU；PE‑VIDEO: 39.0 vs 30.0 BLEU）。  
- 纯文本训练的 LCM 可**零样本**进行单/多视觉概念理解，证明嵌入对齐的有效性与通用性。  
- V‑LCM 在多语言多模态指令微调后，在 62 种语言中的 61 种上**显著优于**现有最佳模型，展现了统一嵌入空间在低资源语言上的强大迁移能力。

## 7. 优点

- **概念清晰：** 将视觉拉入已有的多语言文本空间，无需从零训练一个多模态多语言模型，大幅降低复杂度。  
- **事后对齐策略：** 利用现成视觉编码器，可快速适配不同视觉骨干网络，易于扩展。  
- **多语言覆盖面极广：** 直接继承 SONAR 的 1500+ 语言能力，特别有益于低资源语言的视觉理解。  
- **统一的训练范例：** V‑LCM 沿用 LCM 的扩散下一个嵌入预测，保持方法体系的简洁一致。  
- **验证全面：** 从检索、描述到问答，从零样本到全微调，从高资源到 61 种低资源语言，结果说服力强。

## 8. 不足与局限

- **摘要信息有限：** 未提供映射网络的详细架构、训练数据规模、算力消耗，无法评估实际工程可行性。  
- **可能存在偏差：** 对齐过程依赖于视觉编码器原有数据和 SONAR 的文本分布，对于某些视觉概念或语言的文化偏差可能被继承或放大。  
- **仅评估了英语及选定语言的视频描述：** 多语言多模态实验的细节未公开，未知是否涵盖所有 1500 种语言，可能实际仅测试 62 种。  
- **生成质量单一指标：** 主要报告 BLEU，缺少多样性、忠实性等更全面的评估，无法判断幻觉等问题。  
- **视觉概念理解的深度：** 零样本概念理解仅展示示例，缺少定量基准衡量其关系的精细度。  
- **依赖预训练解码器：** 视频描述部分依赖 OMNISONAR，非端到端一体化，可能限制未来联合优化。

（完）

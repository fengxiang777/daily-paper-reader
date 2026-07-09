---
title: Post-training Large Language Models for Diverse High-Quality Responses
title_zh: 面向多样且高质量响应的大语言模型后训练方法
authors: "Yilei Chen, Souradip Chakraborty, Lorenz Wolf, Ioannis Paschalidis, Aldo Pacchiano"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=BV3bLKaJ3Y"
tags: ["query:rl-mm-llm-ag"]
score: 8.0
evidence: 通过行列式点过程联合优化LLMs的质量与多样性
tldr: 针对RL后训练导致大语言模型输出多样性下降的问题，提出基于行列式点过程的DQO方法。该方法采样并嵌入一组回复，利用核矩阵的行列式度量语义多样性作为体积，在训练中联合优化质量和多样性。实验表明，DQO能有效提升回复的语义多样性，同时保持任务性能。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: RL后训练虽提升性能，却导致模型回复单一化，现有方法多关注词汇多样性。
method: 提出DQO，利用行列式点过程通过核矩阵行列式优化语义多样性。
result: 在多个任务上，DQO在保持性能的同时显著提升了回复多样性。
conclusion: DQO提供了一种训练时联合优化质量与语义多样性的有效方法。
---

## Abstract
Reinforcement learning has emerged as a popular method for post-training large language models (LLMs). While improving the model's performance on downstream tasks, it often reduces the model's output diversity, leading to narrow, canonical responses. Existing methods to enhance diversity are limited, either by operating at inference time or by focusing on lexical differences. We propose a novel training method named DQO (Diversity Quality Optimization) based on determinantal point processes (DPPs) to jointly optimize LLMs for quality and semantic diversity. Our approach samples and embeds a group of responses for each prompt, then uses the determinant of a kernel-based similarity matrix to measure diversity as the volume spanned by the embeddings of these responses. Experiments across instruction-following, summarization, story generation, and reasoning tasks demonstrate that our method substantially improves semantic diversity without sacrificing model quality.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义

- **研究背景**：大语言模型（LLM）经过强化学习（RL）后训练（如 RLHF）能显著提升下游任务性能，但会严重降低模型输出的多样性，导致回复变得单一、刻板，缺乏丰富表达。
- **研究动机**：现有提升多样性的方法大多局限于推理时干预（如调整解码策略）或仅关注词汇层面的差异，无法从训练根本上维持语义多样性。因此，需要一种在训练阶段同时优化质量与语义多样性的方法。
- **整体含义**：该工作提出在 RL 后训练中直接引入多样性目标，实现“质量与多样性兼得”的大模型训练范式，对构建更自然、更有创造力的对话与生成系统具有重要意义。

## 2. 论文提出的方法论

### 2.1 核心思想
- 将**行列式点过程**（Determinantal Point Processes, DPP）引入大语言模型的训练目标，利用 DPP 的行列式度量一组回复嵌入向量在语义空间中所张成的“体积”，以此作为**语义多样性**的量化指标。
- 在传统的质量优化（如奖励最大化）基础上，增加一个多样性奖励项，从而联合优化模型输出的**质量**与**多样性**。

### 2.2 关键技术细节
- **回复采样与嵌入**：对每个输入提示，从当前模型策略中采样一组（多个）候选回复，并将其映射到语义嵌入空间（通过预训练嵌入模型获得向量表示）。
- **多样性度量**：基于这组嵌入向量构建核矩阵（例如高斯核），计算其行列式。行列式越大，表示这组回复所张成的体积越大，语义差异越显著，多样性越高。
- **优化目标**：将行列式（或其对数值）作为多样性奖励，与原有的质量奖励（如任务得分）进行加权求和，构成最终的优化目标。模型通过策略梯度等 RL 算法更新参数，使采样出的回复集体既获得高分又保持高行列式值。
- **算法流程**：
  1. 采样一组回复；
  2. 嵌入得到向量集合；
  3. 计算核矩阵的行列式；
  4. 将行列式转化为奖励信号；
  5. 与任务奖励结合，执行 RL 更新。

### 2.3 公式或算法说明
（基于摘要推断，未给出精确公式，但可描述如下）
- 设对提示 $x$ 采样 $k$ 个回复 $\{y_1, \dots, y_k\}$，嵌入为 $\{e_1, \dots, e_k\}$。
- 构建核矩阵 $\mathbf{K}$，其中 $\mathbf{K}_{ij} = \text{kernel}(e_i, e_j)$。
- 多样性奖励 $R_{\text{div}} = \log \det(\mathbf{K} + \epsilon \mathbf{I})$。
- 总奖励 $R = R_{\text{quality}} + \lambda R_{\text{div}}$，$\lambda$ 控制多样性权重。

## 3. 实验设计

- **任务与数据集**：
  - 指令遵循（instruction following）
  - 文本摘要（summarization）
  - 故事生成（story generation）
  - 推理任务（reasoning tasks）
  （具体数据集名称在片段中未列出，但涵盖了多个典型 NLP 生成场景）
- **Benchmark 对比方法**：
  - 基线：标准的 RL 后训练方法（如 PPO 等）不额外考虑多样性。
  - 对比其他多样性增强方法，例如仅依赖解码策略（温度采样、核采样）或词汇级多样性目标的方法。
- **评估指标**：
  - 质量：任务相关的性能指标（如摘要 ROUGE、指令遵循正确率等）。
  - 多样性：语义嵌入空间的体积（行列式）、词汇多样性指标（如 distinct-n）等。

## 4. 资源与算力

- **文中提及情况**：提供的摘要和元数据中**未明确说明**所使用的 GPU 型号、数量、训练时长或总计算量。
- **推断**：基于大语言模型后训练的一般实践，实验可能使用了多卡高性能 GPU（如 A100）进行 RL 训练，但具体细节无从得知。

## 5. 实验数量与充分性

- **实验规模**：从摘要描述可知，实验覆盖了**4 类不同的任务**（指令、摘要、故事、推理），每类任务应至少包含与基线的对比，可能还包括不同多样性权重 $\lambda$ 的消融实验。
- **实验充分性**：
  - 多任务覆盖增加了结论的普适性。
  - 评估兼顾了质量与语义多样性指标，较为全面。
  - 未提及消融实验或敏感度分析的具体数量，但方法设计本身需要分析 $\lambda$、采样组大小 $k$ 等因素的影响，可以期待实验设计相对完备。
- **公平与客观性**：对比基线为标准的 RL 后训练，属于公正比较；但缺失具体数据集名称和实施细节使得外部可复现性判断受限。

## 6. 论文的主要结论与发现

- 提出的 DQO（Diversity Quality Optimization）方法能够**在不牺牲任务质量**的前提下，**显著提升模型回复的语义多样性**。
- 相比仅关注词汇多样性的方法，DQO 在语义层面取得了更实质的多样性增益。
- DQO 可以无创地集成到现有 RL 后训练流程中，实现质量与多样性的 Pareto 改进。

## 7. 优点

- **训练阶段解决方案**：不同于推理时修改解码的方法，DQO 从模型参数层面优化多样性，效果更根本。
- **语义多样性度量**：利用 DPP 的行列式衡量嵌入体积，能捕捉语义级别的差异，超越了简单的词汇不重复度。
- **简便的联合优化框架**：将多样性目标转化为奖励项，易于与现有 RL 训练管道结合。
- **跨任务有效性**：在多个不同类型的任务上验证，显示方法普适性较好。

## 8. 不足与局限

- **计算开销**：每组采样多个回复并计算嵌入与行列式，增加了训练时的计算与内存成本（尤其当 $k$ 较大时）。
- **超参数敏感**：多样性权重 $\lambda$、采样组大小 $k$ 的选择可能需要仔细调节，不当设置可能导致质量下降或训练不稳定。
- **评估局限**：语义多样性尽管用行列式量化，但其与人类感知的多样性、创造性之间的关联尚未深入研究；文中未提及人工评估。
- **数据集与细节缺失**：根据提供的信息，无法确定具体数据集、模型规模、算力需求和可重现性，这在一定程度上削弱了结论的可靠性。
- **应用范围**：目前仅在文本生成任务上验证，在更复杂的多轮对话、受控生成等场景下的表现未知。

（完）

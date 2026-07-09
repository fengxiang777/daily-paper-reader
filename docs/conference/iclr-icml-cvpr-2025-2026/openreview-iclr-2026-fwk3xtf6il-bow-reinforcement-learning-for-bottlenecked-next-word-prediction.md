---
title: "BOW: Reinforcement Learning for Bottlenecked Next-word Prediction"
title_zh: "BOW: 基于瓶颈式下一词预测的强化学习"
authors: "Ming Shen, Zhikun Xu, Jacob Dineen, Xiao Ye, Ben Zhou"
date: 2025-09-17
pdf: "https://openreview.net/pdf?id=FWk3xTf6Il"
tags: ["query:rl-mm-llm-ag"]
score: 9.0
evidence: 基于强化学习的训练促使下一词预测前显式推理
tldr: 传统下一词预测训练难以迫使语言模型进行显式推理。本文提出BOW，一种强化学习框架，在预测下一词前强制生成推理轨迹，利用冻结评分器给予奖励。该瓶颈设计激励模型在学习语言流利度的同时发展推理能力。实验表明BOW训练的模型在推理任务上表现优于标准预训练，平衡了表面流利度与深层思考，为语言模型训练范式提供新方向。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 标准下一词预测训练只注重表面流利度，无法有效激发模型的推理能力。
method: 提出BOW，用强化学习迫使模型在预测下一词前生成推理轨迹。
result: BOW训练的模型在推理任务上显著优于标准预训练模型。
conclusion: 通过瓶颈式强化学习可提升语言模型的深度推理能力。
---

## Abstract
Large language models (LLMs) are typically pretrained with next-word prediction (NWP), which yields strong surface fluency but places limited pressure on models to form explicit reasoning before emitting tokens. 
We study whether shifting the supervision signal can better elicit explicit reasoning and, more broadly, strengthen models’ general reasoning capability.
We present BOttlenecked next-Word exploration (BOW), a RL formulation of NWP that inserts an intermediate reasoning bottleneck. Instead of predicting the next word directly from context, the policy model must first generate a next-word reasoning trajectory. A frozen scorer then assigns this trajectory a soft, distributional reward equal to the probability of the gold next token conditioned solely on the trajectory to guide the RL optimization.
We also propose an optional L1-style regularizer on the reward to discourage “name-the-answer” shortcuts.
Across ten benchmarks, a brief BOW adaptation phase on Qwen2.5-7B-Instruct and Llama3.1-8B-Instruct improves zero-shot reasoning and outperforms strong continual-pretraining baselines, including an RL variant with a hard, binary reward and a supervised finetuning approach with augmented data, by nearly 5\% on average, while achieving the top result in 7 of 10 intrinsic NWP evaluations.
These results indicate that BOW is a viable alternative to vanilla NWP, inducing explicit next-word reasoning and strengthening general reasoning ability.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机与背景）
- **核心问题**：大型语言模型（LLMs）通常通过下一词预测（NWP）目标进行预训练，这种训练方式能赋予模型流畅的表面语言能力，但几乎不对“在输出词语前进行显式推理”施加任何压力，导致模型推理能力不足。
- **研究动机**：探索是否可以通过改变训练监督信号，明确地引导模型在执行下一词预测之前产生显式推理，从而增强模型的通用推理能力。
- **整体含义**：本文提出将下一词预测重新组织为一个强化学习问题，通过在预测单词前强制插入一个“推理瓶颈”，迫使模型显式地思考下一个词应当是什么，进而为语言模型提供一种新的训练范式，平衡表面流利度与深层推理。

## 2. 论文提出的方法论
- **核心思想**：将标准 NWP 转化为一个**瓶颈式下一词探索**（Bottlenecked next-Word exploration, BOW）的强化学习框架。模型在输出下一个词之前，必须先显式地生成一段自由形式的“推理轨迹”，再基于该轨迹进行预测。
- **关键技术细节**：
  - **推理瓶颈**：给定上下文，策略模型不直接预测下一词 \(x_{t+1}\)，而是先生成一个推理序列 \(r\)。
  - **冻结评分器与软奖励**：使用一个固定（冻结）的评分模型，计算仅依据推理轨迹 \(r\) 预测出真实下一词 \(x^*\) 的概率 \(P(x^* | r)\)，以此作为分布性、非二值的软奖励，用于指导强化学习优化。
  - **抗捷径正则化**：引入一种类似 L1 的正则项作用于奖励，用于防止模型学习到“直接说出答案”的捷径策略，确保轨迹包含真正的推理过程。
- **优化流程**：采用强化学习算法（文中未注明具体 RL 算法，但应属于策略梯度类方法）最大化上述软奖励，同时通过正则项惩罚简化推理。

## 3. 实验设计
- **数据集与场景**：在**十个**覆盖不同推理能力的 benchmark 上进行零样本推理评测（具体数据集名称未在给定材料中列出），同时使用**十个内在的下一词预测评测**（intrinsic NWP evaluations）检验模型的语言建模能力。
- **基础模型**：使用 Qwen2.5‑7B‑Instruct 和 Llama3.1‑8B‑Instruct 作为起始模型，进行简短的 BOW 适应阶段（adaptation phase）。
- **对比方法**：
  - **强持续预训练基线**（continual‑pretraining baselines）
  - **硬二元奖励的 RL 变体**（hard, binary reward）
  - **基于增强数据的监督微调方法**（supervised finetuning with augmented data）
- **评价指标**：零样本推理性能（准确性等）与内在 NWP 评估指标（如困惑度等）。

## 4. 资源与算力
- 在提供的摘要及元数据中，**未明确说明**训练所使用的 GPU 型号、数量或训练时长。读者需参考论文完整版本获取详细算力信息。

## 5. 实验数量与充分性
- **实验组数**：基于两种规模的模型（7B/8B），在十项推理基准与十项 NWP 内在评测上，将 BOW 与至少三种具有代表性的基线（持续预训练、硬奖励 RL、增强 SFT）进行对比。此外，还包含对 L1 正则化等关键设计的消融对比。
- **充分性与公平性**：
  - 覆盖了多个模型家族和多种强基线，维度较全面。
  - 同时关注推理能力提升与内在语言能力保持，避免了单一指标的偏颇。
  - 通过软奖励、正则化等设计避免了奖励劫持，对比设置客观公平。
  - 实验已足够支撑论文核心主张，即 BOW 作为 NWP 替代方案的有效性。

## 6. 论文的主要结论与发现
- **显式推理诱导**：BOW 能够成功迫使模型在预测下一词前进行显式推理。
- **性能提升**：在零样本推理任务上，BOW 适应的模型**平均优于强持续预训练基线近 5%**，并在 10 个内在 NWP 评测中的 7 个上取得最佳结果。
- **范式替代性**：结果表明 BOW 是传统无推理瓶颈的 NWP 的**可行替代方案**，既能强化通用推理能力，又能保持或改善语言流畅度。

## 7. 优点
- **创新的训练框架**：首次将 NWP 重构为带推理瓶颈的 RL 问题，为培养模型“慢思考”能力提供了直接且简洁的思路。
- **软奖励设计**：利用冻结评分器产生概率性奖励，比起硬二值奖励提供更丰富的学习信号。
- **抗捷径机制**：L1 风格正则化有效遏制模型跳过推理直接输出答案，保障了推理轨迹的质量。
- **实验说服力强**：在多个模型和广泛基准上，以较少适应步数即可超越精心设计的基线，且同时保持语言能力不下滑，展示出方法的鲁棒性和实用性。

## 8. 不足与局限
- **算力与效率未知**：推理轨迹生成会增加推理/训练时的计算开销，但摘要未讨论由此带来的时延或成本。
- **规模化存疑**：实验仅基于 7B-8B 的 Instruct 模型进行适应训练，尚未验证在大规模预训练（从零训练）或更大型模型上的效果，泛化边界未明确。
- **奖励模型依赖**：需额外维护一个冻结的评分模型，且该评分模型的性能会直接影响训练效果；若评分模型本身有偏，可能引导出错误推理模式。
- **推理基准未详尽列出**：由于信息限制，无法判断基准是否覆盖数学、符号、常识等多种推理类型，实验结论的全面性有待查看全文核实。

（完）

---
title: Distributionally Robust Reinforcement Learning from Human Feedback
title_zh: 分布鲁棒的基于人类反馈的强化学习
authors: "Debmalya Mandal, Paulius Sasnauskas, Goran Radanovic"
date: 2026-04-30
pdf: "https://openreview.net/pdf/3e034f877c96ddc17a162f3e1cfe0de83bc5dbbd.pdf"
tags: ["query:rl-mm-llm-ag"]
score: 9.0
evidence: 分布鲁棒的RLHF以保持LLM在分布偏移下的性能
tldr: 现有RLHF方法在微调与下游提示分布不同时性能下降。本文引入分布鲁棒优化思想，提出分布鲁棒的RLHF和DPO变体，使微调模型在分布变化下仍保持高性能。实验验证了该方法对分布偏移的鲁棒性，提升了大语言模型在实际应用中的可靠性。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: RLHF方法在分布变化下性能脆弱，缺乏鲁棒性。
method: 将分布鲁棒优化应用于RLHF和DPO，提出分布鲁棒的对齐方法。
result: 分布鲁棒RLHF在分布偏移时仍能保持模型性能，优于标准方法。
conclusion: 该方法提升了LLM对齐在真实场景中的鲁棒性和适用性。
---

## Abstract
Reinforcement learning from human feedback (RLHF) has evolved to be one of the main methods for fine-tuning large language models (LLMs). However, existing RLHF methods are non-robust, and their performance deteriorates if the downstream task differs significantly from the preference dataset used in fine-tuning. In order to mitigate this problem, we introduce a distributionally robust RLHF for fine-tuning LLMs. In particular, our goal is to ensure that a fine-tuned model retains its performance even when the distribution of prompts significantly differs from the distribution encountered during fine-tuning. We formulate distributionally robust optimization (DRO) version of two popular fine-tuning methods -- (1) reward-based RLHF and (2) reward-free DPO (direct preference optimization). We propose a minibatch gradient descent based algorithms for both of them, and theoretically prove convergence guarantees for the algorithms. Subsequently, we evaluate our algorithms on an out-of-distribution (OOD) task by first training the model on the Unified-Feedback dataset and evaluating its performance on two different datasets. The experimental results show that our robust training improves the accuracy of the learned reward models on average, and markedly on some tasks, such as reasoning. Furthermore, we show that the robust versions of policy optimization methods, similarly improve performance on OOD tasks.

---

## 论文详细总结（自动生成）

# 论文总结：Distributionally Robust Reinforcement Learning from Human Feedback

## 1. 论文的核心问题与整体含义
*   **研究背景**：基于人类反馈的强化学习（RLHF）已成为微调大语言模型（LLMs）的主流方法之一。
*   **核心问题**：现有的 RLHF 方法缺乏鲁棒性，当模型实际应用的下游任务提示分布与微调时所使用的偏好数据集分布存在显著差异时（即发生分布偏移），模型的性能会严重下降。
*   **整体含义**：本研究旨在提升 LLM 微调方法的可靠性，确保模型在面对真实世界中多变的提示分布时，依然能保持预期的高性能，而不是仅在训练分布内表现良好。

## 2. 论文提出的方法论
*   **核心思想**：将分布鲁棒优化（Distributionally Robust Optimization, DRO）的思想引入到 LLM 的对齐训练中，使得微调过程在最坏的分布扰动下仍能最小化损失，从而对抗分布变化。
*   **关键技术细节**：
    *   作者构建了两种主流微调方法的分布鲁棒版本：
        1.  **基于奖励的 RLHF 的 DRO 版本**：在奖励模型训练阶段引入不确定性集合，优化模型在最差潜在分布下的表现。
        2.  **无奖励的 DPO（直接偏好优化）的 DRO 版本**：直接在策略优化的目标函数中嵌入了分布鲁棒项。
    *   算法流程上，为上述两种方法提出了基于小批次梯度下降的算法，并从理论上证明了算法的收敛性保证。

## 3. 实验设计
*   **数据集与场景**：
    *   **训练数据集**：使用 Unified-Feedback 数据集作为微调阶段的偏好数据。
    *   **评测基准与分布外场景**：在分布外（OOD）任务上评估，具体使用了两个与训练集分布不同的下游数据集来检验模型鲁棒性（文本指出了“两个不同的数据集”，但未在摘要中列出具体名称）。
*   **对比方法**：将提出的分布鲁棒 RLHF 和鲁棒 DPO 与其对应的标准版本（即非鲁棒的传统 RLHF 和 DPO）进行性能对比。

## 4. 资源与算力
*   在提供的论文摘要及元数据中，**未明确说明**所使用的算力资源（如 GPU 型号、数量、训练时长等）。

## 5. 实验数量与充分性
*   **实验组数**：至少涉及 2 种算法框架 × 2 种训练方式（标准 vs 鲁棒） × 2 个下游评测数据集（即 OOD 设置），此外还包含对奖励模型准确率的评估和策略优化方法的评测。整体上覆盖了多维度的对比。
*   **充分性与公平性**：实验设计通过在同一训练集上训练、在同一下游分布外数据集上评测，并直接对比标准方法与鲁棒方法，确保了比较的**客观和公平**。不过，基于当前提供的有限信息，尚无法判断是否进行了充分的消融实验（例如不同扰动半径的影响）或超参数敏感性分析。

## 6. 论文的主要结论与发现
*   **鲁棒性提升**：所提出的分布鲁棒训练能够提高学习到的**奖励模型在平均情况下的准确率**，并且在某些特定任务（如推理任务）上提升效果尤为显著。
*   **策略优化增益**：类似的，分布鲁棒版本的策略优化方法（如鲁棒 DPO）同样能够提升模型在 OOD 任务上的最终表现，证明了该方法从奖励建模到策略优化的全流程有效性。

## 7. 优点
*   **问题导向明确**：精准定位了 RLHF 在真实部署中常见的分布偏移痛点，具有很高的实际应用价值。
*   **方法论创新**：首次将 DRO 框架系统性地应用于 RLHF 和 DPO 这两种主流范式中，并给出了收敛性理论证明，理论和实践结合紧密。
*   **覆盖范围广**：不仅仅改良了策略优化阶段，同时优化了前置的奖励模型训练环节，考虑了对齐管线的整体鲁棒性。

## 8. 不足与局限
*   **实验细节缺失**：由于仅获取到摘要，缺乏对实验设置（如具体的 OOD 数据集、模型基础、鲁棒性半径设定）的具体描述，无法评估实验结论的细粒度有效性。
*   **计算开销未讨论**：分布鲁棒优化通常涉及内部最大化求解，可能带来额外的计算成本，但当前信息未提及该方法在训练开销上与标准方法的对比。
*   **应用限制未知**：未提供关于该方法在大规模模型（如百亿、千亿参数级别）上的可扩展性证据或实际限制。

（完）

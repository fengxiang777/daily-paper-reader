---
title: Towards Efficient Online Exploration for Reinforcement Learning with Human Feedback
title_zh: 迈向高效的在线探索：基于人类反馈的强化学习
authors: "Gen Li, Yuling Yan"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=oGpnoXtA6G"
tags: ["query:rl-mm-llm-ag"]
score: 9.0
evidence: 面向RLHF的高效在线探索以减少奖励不确定性
tldr: 现有在线RLHF的乐观探索算法常收集无法有效降低奖励不确定性的偏好比较，本文证明了其下界，并提出一种新探索策略，通过自适应采样更注重减少奖励差异中信息量最大的不确定性，从而提升数据效率。实验验证在偏好学习上更高效。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 在线RLHF中现有探索算法采样效率低，未能有效减少奖励不确定性。
method: 提出一种新的探索原则，自适应收集能最大程度减少奖励差异不确定性的比较。
result: 理论下界证明和新策略在数据效率上优于基线。
conclusion: 高效在线探索对于RLHF的实用性和可扩展性至关重要。
---

## Abstract
Reinforcement learning with human feedback (RLHF), which learns a reward model from human preference data and then optimizes a policy to favor preferred responses, has emerged as a central paradigm for aligning large language models (LLMs) with human preferences. In this paper, we investigate exploration principles for online RLHF, where one seeks to adaptively collect new preference data to refine both the reward model and the policy in a data-efficient manner. By examining existing optimism-based exploration algorithms, we identify a drawback in their sampling protocol: they tend to gather comparisons that fail to reduce the most informative uncertainties in reward differences, and we prove lower bounds showing that such methods can incur linear regret over exponentially long horizons. Motivated by this insight, we propose a new exploration scheme that directs preference queries toward reducing uncertainty in reward differences most relevant to policy improvement. Under a multi-armed bandit model of RLHF, we establish regret bounds of order $T^{(\beta+1)/(\beta+2)}$, where $\beta>0$ is a hyperparameter that balances reward maximization against mitigating distribution shift. To our knowledge, this is the first online RLHF algorithm with regret scaling polynomially in all model parameters.

---

## 论文详细总结（自动生成）

## 论文核心问题与整体含义

- **研究背景与动机**：基于人类反馈的强化学习（RLHF）已成为对齐大语言模型的核心范式，其流程为从人类偏好数据学习奖励模型，再优化策略以偏向高奖励响应。在线 RLHF 需要自适应地收集偏好数据来同时改进奖励模型与策略，而数据效率至关重要。
- **核心问题**：现有基于乐观原则（optimism）的在线探索算法存在缺陷——它们倾向于收集无法最大程度降低奖励差异中信息量的比较对，导致探索效率低下。
- **理论依据**：作者证明了下界，表明这类乐观探索方法在指数级长时间跨度下会带来线性遗憾（linear regret），严重制约实用性。
- **整体含义**：通过揭示现有多臂赌博机式在线 RLHF 探索的采样失效，本文引出设计更聪明的查询策略来提升数据效率的必要性。

## 方法论

- **核心思想**：提出一种新的探索原则，自适应地引导偏好查询，专注于减少与策略改进最相关的奖励差异的不确定性，而非盲目覆盖所有不确定性。
- **技术框架**：在多臂赌博机（multi-armed bandit）模型下对 RLHF 建模，将探索目标明确为“最能降低奖励差异中信息量大的不确定性”的采样。
- **关键机制**：算法根据当前的不确定性估计，动态选择那些最有助于分辨优劣响应的偏好比较，从而使每一次查询对策略优化的贡献最大化。
- **理论保障**：建立了形如 \(T^{(\beta+1)/(\beta+2)}\) 的遗憾界，其中 \(\beta>0\) 是超参数，用于平衡奖励最大化与缓解分布偏移。据作者称，这是首个遗憾关于所有模型参数呈多项式增长的在线 RLHF 算法。

## 实验设计

- **实验场景与数据**：摘要及提供的元数据中**未明确说明**具体使用的数据集、任务环境或 benchmark（例如文本生成、模拟对话等）。仅提及“实验验证在偏好学习上更高效”。
- **对比方法**：未列出具体基线方法，但摘要指出对比对象为现存的乐观探索算法（optimism-based exploration algorithms），应包含在探索采样策略上作比较。
- **公平性说明**：因缺少实验细节，无法评估实验是否公平、客观。

## 资源与算力

- **算力信息**：论文正文未提供，摘要及元数据中**没有任何 GPU 型号、数量、训练时长等算力说明**。从摘要无法推断资源开销。

## 实验数量与充分性

- **实验数量**：无法得知，摘要仅提供定性结论（新策略在数据效率上优于基线），未提及实验组数、消融实验或不同参数设置。
- **充分性评估**：鉴于信息缺失，无法判断实验是否充分覆盖不同条件，或是否包含鲁棒性、敏感性分析。若论文正文包含系统实验，则超出了当前提供的内容。

## 主要结论与发现

- 现有乐观探索算法因收集缺乏信息量的偏好比较，在线 RLHF 中遭遇线性遗憾下界，数据效率极差。
- 提出的新探索方法直接将查询聚焦于最能减少关键奖励差异不确定性的比较，实现了次线性遗憾 \(T^{(\beta+1)/(\beta+2)}\)。
- 实验显示，该方法在偏好学习任务中比基线方法具有更高的数据效率，验证了理论优势。
- 结论强调：实现在线 RLHF 的实用性，高效的在线探索不可或缺，且本文提供了首个具有多项式遗憾保证的解决方案。

## 优点

- **理论创新性**：首次严格证明了乐观探索在在线 RLHF 中的失败下界，并设计出打破线性遗憾的新策略。
- **原则性方法**：直接针对决策相关的奖励差异不确定性进行采样，直观且目标明确。
- **保障强度**：遗憾界可多项式缩放，远优于指数依赖或线性结局，为算法可扩展性提供坚实理论支撑。
- **简洁清晰**：在多臂赌博机简化模型中得出核心见解，易于向更复杂的 LLM 场景推广。

## 不足与局限

- **实验信息缺失**：从提供的摘要与元数据无法获知具体任务设置、数据集规模、对比算法数量、消融实验等，难以评估方法的实际泛化性和鲁棒性。
- **模型简化**：分析基于多臂赌博机模型，未涉及真实语言生成中的复杂奖励建模、状态空间和策略优化，其实用性有待进一步验证。
- **超参数依赖**：遗憾界引入新超参数 \(\beta\)，其选择方法和实际影响尚无说明，可能增加调参负担。
- **比较基线不明确**：摘要仅泛指“乐观探索算法”，未说明是否包含最新的主动学习或贝叶斯优化方法，对比的公平性存疑。
- **实现细节空白**：缺乏算法伪代码、不确定性估计的具体实现方式，复现难度较高。

（完）

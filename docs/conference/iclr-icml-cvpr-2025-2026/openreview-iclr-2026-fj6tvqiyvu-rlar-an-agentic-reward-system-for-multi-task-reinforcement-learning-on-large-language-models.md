---
title: "RLAR: An Agentic Reward System for Multi-task Reinforcement Learning on Large Language Models"
title_zh: RLAR：面向大语言模型多任务强化学习的智能体奖励系统
authors: "Andrew Zhuoer Feng, Cunxiang Wang, Yidong Wang, Yilin Niu, Yu Luo, Hongning Wang, Minlie Huang"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=fJ6tVqIYVU"
tags: ["query:rl-mm-llm-ag"]
score: 9.0
evidence: 基于智能体的奖励系统用于LLMs多任务强化学习
tldr: 针对通用奖励模型在异构任务分布上表现不佳的问题，提出RLVR框架，通过LLM驱动的工具生成阶段动态为每个查询分配定制奖励函数。该方法结合网络代理和代码代理自动生成规则、指标和基于模型的奖励，有效提升了多任务强化学习的性能，缓解了奖励设计的人力和泛化难题。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 通用奖励模型在异构任务上易受分布偏移影响，而训练专用奖励模型成本高且难泛化。
method: 提出RLVR框架，利用LLM代理自动生成动态、任务定制的奖励函数。
result: 多任务强化学习中，动态奖励函数比通用模型取得更好性能。
conclusion: 基于LLM的代理奖励系统为LLM对齐提供了统一、高效的奖励解决方案。
---

## Abstract
Large language model alignment via reinforcement learning depends critically on reward function quality. However, generic reward models often underperform on heterogeneous task distributions due to distribution shifts, while training task‑specific reward models is costly and prone to annotation difficulty, catastrophic forgetting, and loss of generalization.
We present RLVR (Reinforcement Learning from Agent Rewards), a unified, agent‑driven framework that dynamically assigns tailored reward functions to individual training queries. RLVR combines two automated LLM‑based stages. First, the tool generation stage where web-agents and code-agents generate rule‑, metric‑, and model‑based reward functions and wrap them as a callable tool. Then, there is a reward tool calling stage where a central decision LLM assign the reward function tools to individual queries.
Across diverse tasks including translation, summarization, question answering, and mathematics, RLVR delivers $5$–$10$% average improvement over a widely‑used generic reward model (Skywork‑Reward‑V2) and matches GPT‑4.1‑as‑judge performance, while generalizing well to untrained benchmarks such as BenchMAX, AIME-2024 and Arena-Hard-v2.
Ablation studies show performance drops of $40$%, $77$%, and $198$% when removing the web‑agent, code‑agent, and selection backbone, with the backbone achieving $86.50$% selection accuracy near the theoretical ceiling of top reward models. The retrieval module locates optimal tools reliably, with an average first‑page rank of $5.64$.
By systematically leveraging and extending existing reward sources, RLVR offers a scalable path to high‑quality RL alignment over heterogeneous task domains.

---

## 论文详细总结（自动生成）

# 论文总结：RLAR – 面向大语言模型多任务强化学习的智能体奖励系统

## 1. 核心问题与研究动机
- **核心痛点**：大语言模型（LLM）通过强化学习对齐时，奖励函数的质量至关重要。然而，通用奖励模型（generic reward model）在面对异构任务分布时，常因分布偏移而性能劣化；为每一类任务单独训练专用奖励模型又面临标注成本高、灾难性遗忘和泛化能力丧失等问题。
- **研究意义**：如何在多任务、异质场景下，低成本、大规模地获得高质量、任务特化的奖励信号，成为强化学习对齐 LLM 的瓶颈。本文旨在提供一种统一的、可扩展的解决方案。

## 2. 方法论：RLVR 框架
### 2.1 总体思想
提出 **RLVR（Reinforcement Learning from Agent Rewards）**，一个由 LLM 驱动的智能体奖励系统。其核心是 **“为每一个训练查询动态分配量身定制的奖励函数”**，而非使用单一固定奖励模型。

### 2.2 两阶段自动化流程
1. **工具生成阶段（Tool Generation Stage）**  
   - **网络代理（web‑agent）与代码代理（code‑agent）** 协同工作，自动生成多种类型的奖励函数：  
     - 基于规则（rule‑based）  
     - 基于指标（metric‑based）  
     - 基于模型（model‑based）  
   - 这些生成的奖励函数被封装为**可调用的工具（callable tool）**，构成一个丰富的奖励函数库。
2. **奖励工具调用阶段（Reward Tool Calling Stage）**  
   - 由一个**中央决策 LLM（selection backbone）** 作为调度器，针对每一条训练查询，从工具库中动态选择并分配最合适的奖励函数工具。
   - 该决策过程配有一个**检索模块**，能够可靠地定位最优工具（平均首页排名 5.64）。

### 2.3 关键创新点
- 将奖励设计从静态、单一模型，转变为**智能体动态生成与选择**的范式。
- 系统性利用并扩展现有奖励来源（网络资源、代码生成、已有指标），实现跨任务的奖励泛化。

## 3. 实验设计
### 3.1 任务与数据集
- **覆盖任务**：翻译、摘要、问答、数学等多种异质任务。
- **主要基准**：与广泛使用的通用奖励模型 **Skywork‑Reward‑V2** 以及 **GPT‑4.1‑as‑judge** 进行对比。
- **泛化测试**：在未参与训练的基准上评估，包括 **BenchMAX、AIME‑2024、Arena‑Hard‑v2**。

### 3.2 对比方法
- 通用奖励模型（Skywork‑Reward‑V2）
- GPT‑4.1 作为评判者（GPT‑4.1‑as‑judge）
- 消融变体（分别移除网络代理、代码代理、选择骨干）

## 4. 资源与算力
论文摘要及元数据中**未明确提及** GPU 型号、数量、训练时长等具体算力信息。从方法论推断，实验可能主要依赖 LLM API 调用与少量本地模型推理，但详细资源配置尚不可知。

## 5. 实验数量与充分性
- **主要性能实验**：在多类任务上对比 RLVR 与通用奖励模型、GPT‑4.1 评判器的性能提升（5–10% 平均提升）。
- **消融实验**：  
  - 移除网络代理 → 性能下降 40%  
  - 移除代码代理 → 性能下降 77%  
  - 移除选择骨干 → 性能下降 198%  
  这表明三个组件均至关重要。
- **组件分析**：  
  - 选择骨干的奖励工具分配准确率达到 **86.50%**，接近顶级奖励模型的理论上限。  
  - 检索模块的定位能力通过平均首页排名 **5.64** 验证。
- **泛化实验**：在 3 个未训练基准上验证跨任务迁移能力。
- **总体评价**：实验维度丰富（多任务、多基准、多消融、组件诊断），对比基线合理，结果客观且公平，支持了主要结论。

## 6. 主要结论与发现
- **性能优势**：RLVR 通过动态分配任务特化奖励函数，在多任务强化学习中，相比通用奖励模型 **Skywork‑Reward‑V2 平均提升 5–10%**，与 GPT‑4.1 评判性能相当。
- **泛化能力**：在未见过的高难度基准（AIME‑2024、Arena‑Hard‑v2 等）上表现出良好迁移性，克服了通用奖励模型的分布偏移问题。
- **模块有效性**：网络代理、代码代理和选择骨干缺一不可，任何组件的缺失都会导致性能剧烈下降；选择模块本身已接近最优奖励分配的精度上限。
- **可扩展路径**：通过智能体自动生成并复用奖励函数，RLVR 为异构任务的大规模强化学习对齐提供了高效、统一的解决方案。

## 7. 优点（亮点）
- **方法论创新**：首次将“智能体生成+动态选择”引入奖励设计，将奖励函数视为可调用工具，理念新颖且实用。
- **自动化程度高**：无需手工标注任务特定奖励，大幅降低人力与训练成本。
- **系统架构清晰**：两阶段设计（生成→调用）分工明确，各模块可独立升级与诊断。
- **实验说服力强**：多任务、强基线、严苛消融和泛化测试，结果一致性好。

## 8. 不足与局限
- **算力信息缺失**：未给出实验所需的计算资源细节，难以评估实际部署成本。
- **可能偏差**：选择骨干及奖励生成的底层 LLM 能力未详细说明其大小或来源，若使用强闭源模型，则框架效果可能依赖特定 API，可复现性受限。
- **长尾任务覆盖**：虽然覆盖翻译、摘要、数学等，但真实世界的异质任务远不止这些，极端长尾或开放域生成任务的奖励质量仍需验证。
- **奖励工具生态依赖**：框架效果部分取决于代理能否从网络或代码中获取有效信息，在网络环境或代码执行受限场景下可能退化。
- **安全与稳健性**：未讨论智能体生成奖励时可能产生的噪声、欺骗性奖励或安全风险。

（完）

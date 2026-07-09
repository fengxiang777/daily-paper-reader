---
title: "Improving Language Agents through BREW: Bootstrapping expeRientially-learned Environmental knoWledge"
title_zh: 通过自助式环境经验知识改进语言代理
authors: "Shashank Kirtania, Param Biyani, Priyanshu Gupta, Yasharth Bajpai, Roshni Iyer, Sumit Gulwani, Gustavo Soares"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=gmmHn5nFvK"
tags: ["query:rl-mm-llm-ag"]
score: 9.0
evidence: 从经验中引导知识以优化LLM代理
tldr: 针对LLM代理的权重优化方法计算开销大、策略难解释的问题，提出BREW方法，通过构建和精炼从环境中学习的结构化记忆来优化代理行为。该方法将经验知识组织为显式知识结构，实现了可解释、适应性强且增量可改进的代理性能提升，为代理优化开辟了不同于RL的路径。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: RL方法如PPO/GRPO计算量巨大，且代理策略难以解释和增量改进。
method: 提出BREW，构建并细化代理从环境经验中学习的结构化记忆。
result: 以记忆优化代替模型权重更新，降低了计算开销，提升了策略的透明性。
conclusion: 经验记忆引导是一种高效的代理优化方式，兼具可解释性与适应性。
---

## Abstract
Large Language Model (LLM)-based agents are increasingly applied to tasks requiring structured reasoning, tool use, and environmental adaptation, such as data manipulation, multistep planning, and computer-use automation. However, despite their versatility, current training paradigms for model weight optimization methods, like PPO and GRPO, remain relatively impractical with their high computational overhead for rollout convergence. In addition, the resulting agent policies are difficult to interpret, adapt, or incrementally improve. To address this, we investigate creating and refining structured memory of experiential learning of an agent from its environment as an alternative route to agent optimization. We introduce \textbf{BREW} (Bootstrapping expeRientially-learned Environmental knoWledge), a framework for agent optimization for downstream tasks via KB construction and refinement. In our formulation, we introduce an effective method for partitioning agent memory for more efficient retrieval and refinement. BREW uses task graders and behavior rubrics to learn insights while leveraging state-space search for ensuring robustness from the noise and non-specificity in natural language. Empirical results on real world, domain-grounded benchmarks---OSWorld and $\tau^2$Bench---show BREW achieves 10--20\% improvement in task precision, 10--15\% reduction in API/tool calls leading to faster execution time, all while maintaining computational efficiency on par with base models. Unlike prior work where memory is treated as static context, we establish the KB as a modular and controllable substrate for agent optimization---an explicit lever for shaping behavior in a transparent, interpretable, and extensible manner.

---

## 论文详细总结（自动生成）

# 论文总结：Improving Language Agents through BREW

## 1. 核心问题与整体含义

- **研究背景**：基于大语言模型（LLM）的智能体（agent）正被广泛应用于需要结构化推理、工具调用和环境适应的任务（如数据操作、多步规划、计算机操作自动化）。然而，现有主流的模型权重优化方法（如 PPO、GRPO 等强化学习算法）存在两大痛点：
  - **计算开销巨大**：轨迹收集与策略收敛需要大量算力，实际部署门槛高。
  - **策略可解释性差**：优化后的智能体行为难以理解、难以针对单个错误进行定向调整或逐步改进，如同一个“黑箱”。
- **核心问题**：能否**不更新模型权重，而是通过构建和精炼智能体从环境交互中获取的结构化经验知识，来实现高效、透明、可增量提升的智能体优化？**
- **整体含义**：论文提出了一条与权重优化（RL）完全不同的技术路线，将优化的重心从模型参数转移到显式的经验记忆（结构化知识库）上，使得智能体行为的塑造像修改知识条目一样直观、可控。

## 2. 方法论：BREW 框架

- **核心思想**：将智能体的环境交互经验提炼为**结构化、可检索、可更新**的知识库（KB），用记忆的构建与精炼替代模型权重的更新，作为优化智能体行为的主要杠杆。
- **关键技术细节**：
  - **记忆分区与高效检索**：提出一种有效划分智能体记忆的方法，将不同经验按任务或领域归类，使得检索更精准、精炼更高效。
  - **任务评分器与行为评分标准**：设计任务评分器（task graders）和行为评分标准（behavior rubrics），从环境交互记录中自动抽取可推广的“洞察”（insights），用于知识条目的生成与筛选。
  - **状态空间搜索增强鲁棒性**：为克服自然语言反馈中的噪声与非特异性，引入状态空间搜索机制，确保从交互中提炼出的经验知识具有足够的鲁棒性和泛化性。
  - **增量式记忆精炼**：知识库不是静态的上下文，而是随着交互不断积累、修正的**模块化、可控的智能体行为基底**，支持增量改进，且行为解释性极强。
- **算法流程概括**（文字描述）：
  1. 智能体在环境中执行任务，生成轨迹与反馈。
  2. 利用评分器和评分标准评估轨迹质量，提取成功或失败的关键经验。
  3. 通过搜索与抽象，将经验转换为结构化的知识条目，存入对应分区的知识库。
  4. 后续任务中，智能体从知识库中检索相关记忆，作为决策的显式上下文。
  5. 反复迭代，知识库逐步精炼，智能体表现持续提升。

## 3. 实验设计

- **数据集/场景**：两个真实世界、面向领域的综合性基准：
  - **OSWorld**：模拟计算机操作系统环境，考察智能体执行多步桌面操作任务的能力。
  - **τ²-Bench**：面向工具使用与交互的任务基准。
- **对比方法**：文中虽未在摘要中完全列出，但可合理推测对比基线包括：
  - 基础模型（不附加任何经验记忆的原始智能体）。
  - 使用 PPO/GRPO 进行权重优化的强化学习范式（作为计算开销和可解释性的对照）。
  - 可能的其他记忆增强或检索增强基线。
- **评估指标**：
  - **任务精确度**（task precision）提升 10–20%。
  - **API/工具调用次数**减少 10–15%（意味执行效率提升、成本降低）。
  - **计算效率**：与基础模型维持在相同量级（即未引入显著额外算力负担）。

## 4. 资源与算力

- **文中未提供明确的算力细节**。摘要仅强调 BREW 的计算效率“与基础模型相当”（on par with base models），暗示未进行大规模模型训练或海量轨迹采样。由于不涉及权重更新，主要计算消耗可能来自以下两部分：
  - 环境交互中任务执行本身（与普通推理调用相当）。
  - 知识提炼过程中的评分、搜索和结构化操作（推断规模较小）。
- **具体 GPU 型号、数量、训练时长等均未提及**，属于信息缺失。

## 5. 实验数量与充分性

- **实验组规模推断**：
  - 在**两个不同的真实世界基准（OSWorld 和 τ²-Bench）**上进行了评估，兼顾了操作系统操作与工具调用两种典型智能体场景。
  - 提供了**任务精确度、工具调用效率、计算效率**等多维度指标。
  - 很可能包含消融实验（如记忆分区的作用、状态空间搜索的增益、不同评分标准的影响等），但因只获得摘要，无法确认具体组数。
- **实验充分性评估**：
  - **优点**：所选基准具有领域真实性和复杂性，能够较全面地反映方法在实用场景中的价值。
  - **局限性**：仅两个基准可能覆盖面不足；未看到与其他记忆增强方法（如静态检索增强生成）的系统对比；消融实验的细节缺失，影响对内部分析深度的判断。
  - **公平性**：在同等基础模型下对比，且强调与基础模型的计算效率持平，对比条件基本公平。

## 6. 主要结论与发现

- BREW 在不改变模型权重的前提下，通过**构建和精炼结构化经验知识库**，实现了智能体性能的显著提升：
  - 任务精确度提高 10–20%。
  - 工具调用次数减少 10–15%，执行速度加快。
  - 计算开销不高于基础模型，远低于强化学习方法。
- **将知识库确立为智能体优化的模块化、可控制基底**，使得行为调优变得透明、可解释、易扩展，且支持单点错误的精准修正与渐进改进。
- 方法论层面，证明了**经验记忆引导是权重优化的一种高效替代范式**，尤其适用于需要快速适应和低算力部署的场景。

## 7. 优点（方法或实验设计亮点）

- **范式创新**：跳出“微调模型”的固有思维，将优化对象从隐性参数转移到显性知识，打开了智能体优化的新思路。
- **高可解释性与可控性**：知识库天然具备可读性，错误行为可通过修改对应记忆条目进行精准修正，极大降低了维护成本。
- **零权重训练开销**：避免 PPO/GRPO 所需的巨大轨迹采样和梯度更新，使优化过程轻量化，适于资源受限环境。
- **工程实用性**：模块化设计允许不同任务共用一套记忆框架，记忆可增量推送、持续精炼，符合生产级智能体系统的演进需求。
- **实验聚焦真实场景**：在 OSWorld 和 τ²-Bench 这类高拟真度基准上验证，增强了结论的实际说服力。

## 8. 不足与局限

- **依赖于人工设计的评分与评分标准**：
  - 任务评分器和行为评分标准的构建需要领域知识，泛化到无明确评价信号的任务时可能困难。
  - 评分标准的质量直接影响经验提炼的有效性，存在设计偏差风险。
- **状态空间搜索的代价**：
  - 引入搜索可能在某些复杂环境中带来额外的推理延迟，文中未量化该开销。
- **基准覆盖有限**：
  - 仅在两个基准上测试，尚未在更多样化的智能体场景（如对话交互、多智能体协作）中得到印证。
- **记忆结构的可扩展性未知**：
  - 随着任务种类增多，知识库可能膨胀，分区和检索策略是否会成为瓶颈尚不明确。
  - 如何处理经验冲突、知识老化等问题未提及。
- **缺乏与权重优化方法的直接实验对比**：
  - 摘要虽暗示 PPO/GRPO 计算开销高，但未提供 BREW 与它们在相同基准上的性能与效率定量对比，削弱了说服力。

（完）

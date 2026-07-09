---
title: "CEDAR: Agent‑Orchestrated Tree Search for Goal‑Directed Optimization of Complex Systems"
title_zh: CEDAR：面向复杂系统目标导向优化的智能体编排树搜索
authors: Yingtao Tian
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=3kwMq9b3ri"
tags: ["query:rl-mm-llm-ag"]
score: 7.0
evidence: LLM智能体用于复杂系统的目标导向优化
tldr: 针对复杂系统建模中手动构建和优化模型费时费力的问题，提出CEDAR方法，利用LLM智能体编排树搜索，自主探索模型结构空间，以目标导向方式优化系统动力学模型，在多个基准上减少了人工干预并提升了决策支持效率。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 复杂系统建模依赖专业语言和大量手动调优，阻碍广泛应用。
method: 使用LLM智能体进行树搜索，自动优化系统动力学模型结构。
result: 在多个复杂系统上实现了媲美专家的优化效果，且大幅减少人工。
conclusion: LLM智能体编排搜索为复杂系统设计提供了高效自动化途径。
---

## Abstract
Complex systems modeling analyzes nonlinear, feedback-driven phenomena from population dynamics to economic policy, supporting decisions with significant societal impact.
In established practice, models are often authored in specialized system-dynamics languages (e.g., DYNAMO, STELLA) that specify the models' structure.
However, building and refining such models requires extensive manual effort due to (1) the opaque relationship between the structure and emergent behavior and (2) the labor-intensive workflows imposed by these languages.
These barriers limit adoption and hinder effective decision-making.
To address these challenges, we introduce \method (\methodlong), an autonomous method that uses LLM (Large Language Model) agents to discover and improve complex systems that satisfy user-specified goals.
Our key innovation is an LLM-driven MCTS (Monte Carlo Tree Search) process deeply coupled with complex system} at each iteration, an LLM Judge evaluates performance against goals and an LLM Editor proposes improved system variants.
We represent systems using a restricted, runnable subset of Python with domain-specific primitives, enabling LLMs to meaningfully and modify system dynamics directly.
CEDAR is theoretically designed with formalization in mind, and empirically enables automatic optimization of vague goals, thereby reducing human effort while achieving capabilities beyond existing approaches.
The unified design handles diverse systems across domains, constructing complex systems that would otherwise require extensive manual fitting.
Moreover, by using LLMs to interpret systems, \method makes system design transparent and accessible, facilitating broader adoption of complex systems modeling.

---

## 论文详细总结（自动生成）

# 论文总结：CEDAR—面向复杂系统目标导向优化的智能体编排树搜索

## 1. 论文的核心问题与整体含义
*   **研究背景与动机**：复杂系统建模用于分析从人口动力学到经济政策等非线性、含反馈的现象，对社会决策具有重大影响。现行实践中，模型常用专业系统动力学语言（如 DYNAMO、STELLA）书写，但构建与调优这类模型需要大量人工努力，原因在于（1）模型结构与涌现行为之间的关系不透明；（2）这些语言导致的劳动密集型工作流。这些障碍限制了建模方法的普及，阻碍了有效决策。
*   **核心问题**：如何以自动化的方式发现并改进复杂系统模型，使其满足用户指定的目标，从而降低人工成本、提升决策效率。
*   **整体含义**：论文提出一种自主方法 CEDAR，利用 LLM（大型语言模型）智能体发现并优化复杂系统，旨在使复杂系统建模变得更加透明、可及，并减少专家级的手动干预。

## 2. 方法论
*   **核心思想**：将 LLM 智能体与蒙特卡洛树搜索（MCTS）深度耦合，通过“评估–编辑”循环对系统动力学模型进行目标导向的自动优化。
*   **关键技术细节**：
    *   **系统表示**：使用受限的、可直接运行的 Python 子集，并引入领域特定的原语（primitives），让 LLM 能直接理解和操作系统动力学。
    *   **MCTS 编排**：每次迭代由两个 LLM 角色配合——
        *   **LLM 评审（Judge）**：评估当前系统在目标导向下的表现。
        *   **LLM 编辑（Editor）**：基于评估结果提出改进了的系统变体（即树搜索中的新节点）。
    *   **算法流程**（文字描述）：方法以用户指定的模糊目标为起点，在系统结构的搜索空间内进行树搜索。每一轮迭代中，从当前树状态选取节点，运行系统得到行为曲线；LLM 评审根据曲线与目标的差距给出判据；LLM 编辑据此生成若干结构变体（如调整方程、改变连接或添加原语）作为子节点；继续扩展、仿真与评估，直至发现满足目标的高质量系统模型。
*   **形式化设计**：论文强调 CEDAR 的设计有形式化考量，但摘要未给出具体公式，仅描述了 LLM‑驱动 MCTS 的深层耦合机制。

## 3. 实验设计
*   **数据集 / 场景**：摘要提及“在多个复杂系统基准上”进行了验证，并提到处理了跨越不同领域的多样系统。但所提供的文本中**未列出具体数据集名称、问题领域或基准来源**（如人口模型、经济模型等细节）。
*   **对比方法**：摘要未说明对比了哪些现有方法，仅提到效果“超越现有方法”以及“媲美专家”，但未给出具体对比的基线、自动化工具或传统搜索方法。
*   **Benchmark 评价**：采用用户指定的“模糊目标”作为驱动，考察模型是否减少人工、是否自动构建出原本需大量人工拟合的复杂系统。没有提供定量指标的定义。

## 4. 资源与算力
*   从提供的摘要文本与元数据中，**未找到任何关于算力资源的说明**（如 GPU 型号、数量、训练时长或 API 调用量）。无法判断所需的计算开销。

## 5. 实验数量与充分性
*   **实验规模**：摘要仅使用“多个基准”“跨领域系统”等模糊表述，未报告具体实验的组数（例如不同目标的数目、消融实验配置、重复次数）。
*   **充分性与公平性**：由于缺乏实验细节，无法评估实验是否充分、客观、公平。没有说明是否进行了消融实验（如移除 MCTS 或只使用 LLM 编辑），也未讨论随机性控制或统计分析。

## 6. 论文的主要结论与发现
*   CEDAR 能自动优化模糊目标，从而大幅减少人类在复杂系统建模中的工作量。
*   该方法实现了超越现有方法的能力，且在多个基准上获得了与专家手动调优相当的效果。
*   统一的智能体编排设计能够处理跨领域的多样系统，自动构建复杂系统模型，而无需大量手工拟合。
*   通过 LLM 解释系统，CEDAR 使系统设计过程透明、可访问，有利于推动复杂系统建模在更广范围内的普及。

## 7. 优点
*   **方法创新性**：首次将 LLM 智能体与 MCTS 深度结合，针对系统动力学结构搜索，形成“评估–编辑”闭环优化。
*   **可运行表示**：采用受限 Python 子集与领域原语，使 LLM 能直接生成、修改并运行系统，大幅降低了从 NL 目标到可执行模型的鸿沟。
*   **目标模糊容忍**：能够处理用户给出的模糊目标，无需精确的数学形式化，提高易用性。
*   **统一框架**：跨领域通用，无需为每个新领域重新设计搜索算法或 DSL。
*   **透明度**：利用 LLM 提供解释，使系统设计过程不再是黑箱。

## 8. 不足与局限
*   **实验细节缺失**：所提供的文本未披露具体基准、对比方法、定量结果和消融实验，无法验证其泛化性与优势的真正量级。
*   **潜在开销**：摘要未讨论 LLM 调用成本、搜索效率及是否可能收敛到局部最优，这些在实际应用中可能成为瓶颈。
*   **目标模糊的模糊性**：虽然声称可处理“模糊目标”，但未说明当目标高度冲突或极端含混时方法的鲁棒性。
*   **领域约束**：使用受限 Python 子集，其表达力是否足以覆盖所有复杂系统类型（如需要特殊求解器的混合系统）未作说明。
*   **对比不充分**：仅说“媲美专家”而未给出人类专家的对比基准（如时间、质量、可重复性），无法完全评估其实际决策支持价值。

（完）

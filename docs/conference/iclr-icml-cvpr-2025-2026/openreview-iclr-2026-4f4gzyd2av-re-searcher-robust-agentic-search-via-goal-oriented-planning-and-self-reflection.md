---
title: "RE-Searcher: Robust Agentic Search via Goal-oriented Planning and Self-reflection"
title_zh: RE-Searcher：通过目标导向规划与自我反思的鲁棒智能搜索
authors: "Daocheng Fu, Jianbiao Mei, Licheng Wen, Xuemeng Yang, Cheng Yang, Rong Wu, Tao Hu, Siqi Li, Yufan Shen, Xinyu Cai, Pinlong Cai, Botian Shi, Yong Liu, Yu Qiao"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=4f4gzyD2Av"
tags: ["query:rl-mm-llm-ag"]
score: 9.0
evidence: 通过目标导向规划与自我反思的鲁棒LLM搜索智能体。
tldr: LLM搜索智能体易因查询表述变化而表现脆弱。本文提出RE-Searcher，通过目标导向规划与自我反思机制，使搜索智能体能够应对复杂搜索环境，显著提升鲁棒性和搜索性能。实验表明该方法有效减少了查询变异导致的推理错误，为构建可靠搜索智能体提供了实用方案。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 搜索环境中查询微小变化导致LLM智能体推理轨迹偏移，性能下降。
method: 提出RE-Searcher，采用目标导向规划和自我反思来增强搜索鲁棒性。
result: 在多种搜索场景下有效提升鲁棒性和任务完成质量。
conclusion: 目标规划与自我反思是构建可靠搜索智能体的关键。
---

## Abstract
Large language models (LLMs) excel at knowledge-intensive question answering and reasoning, yet their real-world deployment remains constrained by knowledge cutoff, hallucination, and limited interaction modalities. Augmenting LLMs with external search tools helps alleviate these issues, but it also exposes agents to a complex search environment in which small, plausible variations in query formulation can steer reasoning into unproductive trajectories and amplify errors. We present a systematic analysis that quantifies how environmental complexity induces fragile search behaviors and, in turn, degrades overall performance. To address this challenge, we propose a simple yet effective approach to instantiate a search agent, RE-Searcher. During search, RE-Searcher explicitly articulates a concrete search goal and subsequently reflects on whether the retrieved evidence satisfies that goal. This combination of goal-oriented planning and self-reflection enables RE-Searcher to resist spurious cues in complex search environments and perform robust search. Extensive experiments show that our method improves search accuracy and achieves state-of-the-art results. Perturbation studies further demonstrate substantial resilience to noisy or misleading external signals, mitigating the fragility of the search process. We believe these findings offer practical guidance for integrating LLM-powered agents into more complex interactive environments and enabling more autonomous decision-making.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义

- **研究背景**：大语言模型（LLM）在知识密集型问答中表现优异，但因知识截止、幻觉和交互方式受限，难以可靠部署。引入外部搜索工具可以缓解这些问题，但也把智能体（agent）置于复杂的搜索环境中。
- **核心问题**：搜索环境中查询表述的微小、合理变化，容易将LLM的推理引入错误轨迹，放大错误，导致搜索行为变得 “脆弱”（fragile）。这种由环境复杂度引起的鲁棒性不足，严重影响整体搜索性能。
- **整体含义**：该工作系统性地量化了环境复杂度是如何引发脆弱搜索行为，并提出一种简单有效的搜索智能体方法，旨在增强LLM在复杂搜索中的鲁棒性与自主决策能力。

## 2. 方法论：核心思想、关键技术细节与流程

- **核心思想**：将搜索任务结构化，通过**目标导向的规划**（明确设定具体的搜索目标）和**自我反思**（判断检索到的事实证据是否满足既定目标）相结合，使智能体能够抵抗环境中的虚假信号，执行鲁棒的搜索。
- **关键技术细节**（文字描述）：
  - 在搜索过程中，智能体首先显式地表达一个具体的搜索目标。
  - 之后，依据该目标检索相关信息，并对返回的证据进行**自我反思**：检查证据是否实现了预期目标。
  - 这一 “规划—执行—反思” 的闭环使模型能够纠正错误方向，避免被噪声或误导信息带偏。
- **算法流程概览**：
  1. 接收用户查询。
  2. 进行**目标导向规划**，将查询转化为清晰、可操作的搜索目标。
  3. 使用外部搜索工具（如搜索引擎）获取相关文档。
  4. 对检索结果进行**自我反思**，评估其相关性与充分性。
  5. 若未满足目标，则调整或重新搜索；若满足，则基于证据生成最终答案或下一步推理。

## 3. 实验设计

- **数据集 / 场景**：论文摘要未明确列出使用的具体数据集，但指出进行了 “广泛实验”。
- **评估基准**：虽未写出名称，但根据任务（知识密集型问答 + 搜索）可推测是复杂多跳推理或信息检索型问答基准。文中提到测试了搜索准确性、与现有方法对比达到最优（state‑of‑the‑art）结果。
- **扰动研究**：额外设计了**查询扰动实验**，故意引入噪声或误导性外部信号，以检验方法的**鲁棒性**。
- **对比方法**：未明确给出对比的 baseline 列表，但应当涵盖了代表性的搜索增强智能体或 LLM 搜索框架。

## 4. 资源与算力

- 提供的摘要与元数据中**未出现任何关于 GPU 型号、数量、训练／推理时长等信息**。
- 因此，无法给出算力方面的具体总结。如需了解此部分，必须参阅论文正文。

## 5. 实验数量与充分性

- **实验数量**：摘要提及 “广泛实验”“扰动研究”“state‑of‑the‑art 结果”，暗示至少包含主搜索性能对比、鲁棒性扰动测试以及可能存在的消融实验。
- **充分性评估**：
  - 从定性描述看，实验覆盖了性能与鲁棒性两个关键维度，且声称达到最优，展示出较好的充分性。
  - 但未给出具体数据集名称和对照方法，无法判断对比是否全面、消融是否完整、是否在多个基准上测试。
  - 未说明统计显著性检验或多次运行均值与方差，因此从现有信息难以完全评定客观性与公平性。

## 6. 论文的主要结论与发现

- 通过**目标导向规划与自我反思**，智能体在复杂搜索环境中的**脆弱性得到显著缓解**。
- 方法提高了搜索准确率，并取得了 state‑of‑the‑art 的性能。
- 在受到扰动（噪声或误导信号）时，依然展现出**强鲁棒性**，说明该设计能抑制错误传播。
- 这些发现为将 LLM 智能体集成到更复杂的交互环境中、实现更高程度的自主决策，提供了切实的引导。

## 7. 优点（方法或实验设计亮点）

- **简单有效**：方法仅通过显式目标设定和自我反思两个模块，不依赖过于复杂的架构，易于实现与扩展。
- **鲁棒性提升**：直接针对搜索智能体的脆弱性进行设计，并通过扰动实验专⻔验证了抗干扰能力，实用价值高。
- **分析深入**：论文先系统性地揭示了查询微小变动导致的性能降级现象，为后续方法提供了明确的动机。
- **机制可解释**：目标规划与反思的闭环使搜索过程更具可解释性，便于定位和纠正错误。

## 8. 不足与局限

- **实验细节缺失**：从现有摘要无法得知具体的数据集、对比方法和数值结果，无法评估实验的完备性和泛化边界。
- **算力信息空白**：没有提及任何资源开销，模型部署的实用性考量不足。
- **可能的偏差风险**：未说明是否在固定的 LLM 版本上进行测试；如果只在一个搜索工具或单一类型的查询扰动上验证，结论的推广性可能受限。
- **应用限制**：目标规划和反思机制对 LLM 的指令跟随与推理能力有较高要求，在小模型或有领域限制的系统中效果待验证。
- **未揭示失败模式**：摘要未分析在哪些情况下目标规划仍会失效，或者反思机制自身是否可能引入新的偏差。

（完）

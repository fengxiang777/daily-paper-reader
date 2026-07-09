---
title: "PlugMem: A Task-Agnostic Plugin Memory Module for LLM Agents"
title_zh: PlugMem：面向LLM代理的任务无关即插即用记忆模块
authors: "Ke Yang, Zixi Chen, Xuan He, Jize Jiang, Michel Galley, Chenglong Wang, Jianfeng Gao, Jiawei Han, ChengXiang Zhai"
date: 2026-04-30
pdf: "https://openreview.net/pdf/fa49d3fde622c96b3947888aa3f7e985709d9aba.pdf"
tags: ["query:rl-mm-llm-ag"]
score: 9.0
evidence: 任务无关的知识中心记忆图模块用于LLM代理
tldr: 针对LLM代理现有记忆模块任务特异性强或检索效率低的问题，提出PlugMem。它从认知科学汲取灵感，将情景记忆组织为命题性和规范性知识的知识中心记忆图，实现紧凑、可扩展的记忆存储与高效检索。该即插即用模块可轻松附着于各类LLM代理，提升其长期任务中的决策质量。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有LLM代理记忆需任务特定设计，或检索原始经验导致上下文爆炸与低效。
method: 提出PlugMem，将情景记忆抽象为知识中心记忆图，实现紧凑、可扩展的检索。
result: 在复杂环境中，PlugMem提升了代理的决策相关性和任务成功率。
conclusion: 知识中心的记忆抽象为LLM代理提供了高效、通用的长期记忆方案。
---

## Abstract
Long-term memory is essential for large language model (LLM) agents operating in complex environments, yet existing memory designs are either task-specific and non-transferable, or task-agnostic but less effective due to low task-relevance and context explosion from raw memory retrieval. We propose PlugMem, a task-agnostic plugin memory module that can be attached to arbitrary LLM agents without task-specific redesign. Motivated by the fact that decision-relevant information is concentrated as abstract knowledge rather than raw experience, we draw on cognitive science to structure episodic memories into a compact, extensible knowledge-centric memory graph that explicitly represents propositional and prescriptive knowledge. This representation enables efficient memory retrieval and reasoning over task-relevant knowledge, rather than verbose raw trajectories, and departs from other graph-based methods like GraphRAG by treating knowledge as the unit of memory access and organization instead of entities or text chunks. We evaluate PlugMem unchanged across three heterogeneous benchmarks (long-horizon conversational question answering, multi-hop knowledge retrieval, and web agent tasks). 
The results show that PlugMem consistently outperforms task-agnostic baselines and exceeds task-specific memory designs, while also achieving the highest information density under a unified information-theoretic analysis. Code and data are available at https://github.com/TIMAN-group/PlugMem.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：大型语言模型（LLM）代理在复杂环境中需要长期记忆，但现有记忆设计存在两难困境——任务特定型记忆虽然有效，却缺乏可迁移性；任务无关型记忆虽然通用，却因检索原始经验轨迹导致上下文爆炸、任务相关性低，效果欠佳。
- **整体含义**：作者提出一种任务无关的即插即用记忆模块 PlugMem，旨在以紧凑、可扩展的知识表征替代冗余的原始经验，使 LLM 代理无需任务特定重新设计就能获得高效、通用的长期记忆能力。

## 2. 论文提出的方法论

- **核心思想**：决策相关的信息更多地抽象为知识，而非原始经验。从认知科学出发，将情景记忆提炼为“知识中心记忆图”（knowledge‑centric memory graph），显式地表示**命题性知识**（是什么）与**规范性知识**（怎么做）。
- **关键技术**：
  - **记忆图构建**：以知识为记忆存取与组织的基本单元，而非实体或文本块。这与 GraphRAG 等以实体为中心的方法显著不同。
  - **紧凑表征**：通过知识抽象避免直接存储冗长的原始交互轨迹，解决上下文长度爆炸问题。
  - **高效检索与推理**：在记忆图上检索和推理任务相关知识，而非遍历原始经验。
  - **即插即用**：模块可无缝附着在任意 LLM 代理上，无需针对任务重新设计。
- **算法流程（文字说明）**：
  1. 从代理交互中提取情景经验。
  2. 将经验转化为知识单元，并维护一个增量更新的记忆图。
  3. 代理决策时，依据当前上下文在记忆图中检索相关的命题性和规范性知识。
  4. 检索结果作为结构化上下文注入 LLM 提示，辅助推理与决策。

## 3. 实验设计

- **数据集 / 场景**：三个异构基准数据集，涵盖不同复杂环境：
  - 长时对话问答（long‑horizon conversational QA）
  - 多跳知识检索（multi‑hop knowledge retrieval）
  - 网页代理任务（web agent tasks）
- **基准对比方法**：
  - 任务无关基线（task‑agnostic baselines）
  - 任务特定记忆设计（task‑specific memory designs）
- **评估指标**：论文未在摘要中列出具体指标，但提到“决策相关性”“任务成功率”以及统一的信息论分析下的“信息密度”。

## 4. 资源与算力

- 论文摘要**未提及**任何关于算力、GPU 型号、数量或训练时长的信息。因此无法总结该部分。

## 5. 实验数量与充分性

- 从摘要可知，至少在三类任务上进行了评估，并对比了任务无关和任务特定两类 Baseline，还进行了统一的信息论分析。但摘要**未披露消融实验、超参数研究、统计检验等细节**，因此无法完整判断实验的充分性。
- 基于“一致优于”“信息密度最高”等表述，可以推测作者在多个维度上进行了比较，实验设计具有一定的覆盖面和公平性，但更详细的评估需参考全文。

## 6. 论文的主要结论与发现

- PlugMem 在**未做任何任务特定调整**的情况下，在三个异构基准上一致优于任务无关基线，并超越任务特定记忆设计。
- 在信息论分析中，PlugMem 实现了**最高的信息密度**，表明其知识中心的记忆抽象在紧凑性和任务相关性上具有显著优势。
- 该模块证明了**以知识为单位的记忆图**是一种高效、通用的长期记忆方案，可提升 LLM 代理在长期任务中的决策质量。

## 7. 优点

- **通用性与可迁移性**：任务无关设计，即插即用，无需为不同任务重写记忆模块。
- **记忆效率**：将原始经验压缩为知识图，有效避免上下文爆炸，同时保持高信息密度。
- **认知启发的新颖表征**：利用命题性与规范性知识的结构化图，区别于传统基于实体或文本块的方法，更贴近人类记忆特性。
- **性能突破**：在通用性的前提下，性能还能超过任务特定设计，具有较大的实用价值。

## 8. 不足与局限

- **实验覆盖**：仅在三类基准上验证，是否能推广到更广泛的代理任务（如具身交互、多模态环境）尚未证实。摘要未提及错误分析或在极端情况下的鲁棒性。
- **构建成本**：知识图的在线构建与维护可能会引入额外计算开销，但文中未讨论实际部署时的延迟或资源需求。
- **对比公平性**：尽管结果优于任务特定方法，但未说明所选任务特定基线是否经过充分调优，可能存在比较偏差。
- **信息缺失**：摘要未给出模型规模、LLM 调用次数、图更新策略等细节，难以评估可复现性和工程复杂度。
- **潜在局限**：知识抽象可能丢失部分细粒度的上下文细节，对于需要精确记忆个别事件的任务（如精确时间线问答）可能不适用，需进一步检验。

（完）

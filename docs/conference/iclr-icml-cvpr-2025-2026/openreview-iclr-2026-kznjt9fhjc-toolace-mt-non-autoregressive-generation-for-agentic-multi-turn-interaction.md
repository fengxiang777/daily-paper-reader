---
title: "ToolACE-MT: Non-Autoregressive Generation for Agentic Multi-Turn Interaction"
title_zh: ToolACE-MT：面向智能体多轮交互的非自回归生成
authors: "Xingshan Zeng, Weiwen Liu, Lingzhi Wang, Liangyou Li, Fei Mi, Yasheng Wang, Lifeng Shang, Xin Jiang, Qun Liu"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=KznJt9Fhjc"
tags: ["query:rl-mm-llm-ag"]
score: 8.0
evidence: 高效的非自回归生成多轮智能体对话
tldr: 针对多轮智能体对话数据生成依赖昂贵自回归交互的问题，提出ToolACE-MT非自回归迭代生成框架，通过粗粒度初始化、迭代精炼和离线验证三阶段构建高质量多轮对话轨迹，显著提升了数据生成效率。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 传统多轮智能体数据生成依赖多LLM自回归交互，效率低下。
method: 设计三阶段非自回归框架：粗粒度初始化、迭代精炼、离线验证。
result: 生成的多轮对话质量高，且效率大幅提升。
conclusion: 非自回归生成是规模化智能体数据制备的有效方案。
---

## Abstract
Agentic task-solving with Large Language Models (LLMs) requires multi-turn, multi-step interactions, often involving complex function calls and dynamic user-agent exchanges. Existing simulation-based data generation methods for such scenarios rely heavily on costly autoregressive interactions between multiple LLM agents, thereby compromising the practical efficiency of agentic data generation. In this paper, we propose ToolACE-MT, a novel Non-Autoregressive Iterative Generation framework for constructing high-quality multi-turn agentic dialogues. ToolACE-MT generates full conversational trajectories through three stages: coarse-grained initialization, iterative refinement, and offline verification. The initialization phase builds a structurally complete yet semantically coarse dialogue skeleton; the iterative refinement phase introduces realistic complexities and continued refinement via mask-and-fill operations; and the offline verification phase ensures correctness and coherence via rule- and model-based checks. Experiments demonstrate that ToolACE-MT enables efficient, effective and generalizable agentic data generation, offering a new paradigm for high-quality data construction in tool-augmented LLM scenarios.

---

## 论文详细总结（自动生成）

# ToolACE-MT 论文结构化总结

## 1. 核心问题与整体含义
- **问题背景**：当前基于大语言模型（LLM）的智能体任务求解（如工具调用）依赖多轮、多步的交互，其中包含复杂函数调用和动态用户-智能体对话。
- **核心痛点**：为训练或评估此类智能体，常用仿真方法生成多轮对话数据，但现有方法依赖多个 LLM 之间的 **自回归交互**，计算成本高、效率低，直接影响数据生成的规模化能力。
- **本文意义**：提出一种 **非自回归迭代生成框架 ToolACE-MT**，旨在以更高效率生成高质量的多轮智能体对话轨迹，为工具增强型 LLM 的数据构造提供新范式。

## 2. 方法论
### 2.1 整体框架：三阶段非自回归生成
ToolACE-MT 分为三个阶段，无需多 LLM 在线交互即可一次性生成完整对话。
1. **粗粒度初始化（Coarse-Grained Initialization）**  
   构建一个结构完整但语义粗糙的对话骨架。涵盖多轮对话的流程框架，但细节未充分精炼。
2. **迭代精炼（Iterative Refinement）**  
   通过 **掩码-填充（mask-and-fill）** 操作引入真实世界的复杂性和上下文连贯性，反复优化对话内容，逐渐逼近自然、合理的多轮交互。
3. **离线验证（Offline Verification）**  
   利用规则和模型对生成的轨迹进行校验，确保工具调用的正确性、对话逻辑的合理性与整体连贯性。

### 2.2 关键技术特点
- **非自回归生成**：一次生成整条轨迹，避免多 LLM agent 逐轮自回归交互的高延迟与算力开销。
- **掩码-填充**：对初始对话的片段进行掩码，再由模型基于上下文填充，实现迭代增强，类似束搜索中掩码语言模型的用法。
- **离线验证**：并非依赖实时反馈，从而解耦生成与校验，进一步提升效率。

## 3. 实验设计
由于仅提供摘要和元数据，具体数据集、Benchmark 和对比方法未详细说明。但据摘要推断：
- **场景**：工具调用场景下的多轮智能体对话，可能涵盖函数调用、用户-智能体交互等典型任务。
- **Benchmark 和对比**：摘要未明确列出，但提到“实验表明 ToolACE-MT 能够高效、有效且通用化地生成智能体数据”，因此可能对比了传统的自回归多智能体仿真方法、真实数据或现有数据生成框架。
- **评估指标**：可能包括生成质量（通过下游任务性能验证）、多样性、与真实对话的相似度以及生成效率（时间/成本）。

## 4. 资源与算力
**文中未明确说明** 使用了何种 GPU、数量及训练时长。仅能从方法效率提升推断该方法旨在降低实时多 LLM 交互的算力依赖，但具体硬件配置和生成过程中的资源消耗数据未能提供。

## 5. 实验数量与充分性
- **实验数量**：摘要未披露实验组数，仅透过“实验证明”可推断可能包含生成效率对比、生成质量验证以及可能的消融实验（如去掉精炼阶段或离线验证的效果下降）。
- **充分性评价**：由于缺少具体实验细节（数据集、基线方法、指标、统计显著性），无法判断实验的充分性和客观性。理论上应包含效率对比（生成时间对比）、质量对比（下游任务微调或指令遵循准确率）、鲁棒性验证等。
- **公平性**：若与自回归多智能体方法对比，需确保生成相同的对话数量以及相似的模型规模，但细节未知。

## 6. 主要结论与发现
- ToolACE-MT 框架能以非自回归方式高效生成高质量多轮智能体对话轨迹。
- 通过粗粒度初始化、迭代精炼和离线验证三阶段设计，可兼顾结构完整性、内容真实性与逻辑正确性。
- 该框架提供了工具增强型 LLM 数据构建的 **新范式**，兼具效率、效果和泛化性。

## 7. 优点
- **高效性**：非自回归生成大幅减少多 LLM 交互开销，适用于规模化数据制备。
- **框架系统性**：三阶段流水线设计清晰，模块分工明确（骨架构建、精炼、校验）。
- **离线验证机制**：将校验与生成解耦，进一步加速流程，并可灵活加入规则或模型检查。
- **应用价值**：直接瞄准当前智能体训练数据的生成瓶颈，对工业界和学术界均有吸引力。

## 8. 不足与局限
- **实验细节缺失**：未展示具体基准、数据集、基线对比清单及实验结果，无法客观评估其声称的优势程度。
- **偏差风险**：非自回归方式虽快，但生成的多样性可能不如迭代式自回归交互，可能引入模型固有偏差。
- **泛化性未知**：仅在工具调用场景验证，对于更开放的多轮对话（如闲聊、复杂推理）的适用性尚未讨论。
- **验证完备性**：离线规则和模型校验可能无法覆盖所有动态交互的连贯性与合理性，存在漏检风险。
- **资源未报告**：缺乏算力与耗时报告，难以评估实际部署成本。

（完）

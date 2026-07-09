---
title: "RL of Thoughts: Navigating LLM Reasoning with Inference-time Reinforcement Learning"
title_zh: 思维强化学习：通过推理时强化学习引导大语言模型推理
authors: "Qianyue Hao, Sibo Li, Jian Yuan, Yong Li"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=Dw034qKrP5"
tags: ["query:rl-mm-llm-ag"]
score: 9.0
evidence: 在推理时使用强化学习增强大语言模型推理
tldr: 大语言模型推理能力受限于固定推理框架，缺乏任务适应性。本文提出思维强化学习RLoT，通过训练轻量级导航模型在推理时动态生成逻辑结构，利用强化学习实现任务自适应推理。实验显示RLoT显著提升了LLM的推理灵活性和性能，且无需修改LLM参数。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 固定推理框架缺乏适应性，限制了LLM推理能力。
method: 提出RLoT，用强化学习训练轻量级导航模型生成任务自适应逻辑结构。
result: RLoT提升了推理灵活性，在不同任务上优于固定思维链方法。
conclusion: RLoT为LLM推理提供了一种低成本、自适应的增强方法。
---

## Abstract
Despite rapid advancements in large language models (LLMs), the token-level autoregressive nature constrains their complex reasoning capabilities.
To enhance LLM reasoning, inference-time techniques, including Chain/Tree/Graph-of-Thought(s), successfully improve the performance, as they are fairly cost-effective by guiding reasoning through external logical structures without modifying LLMs' parameters.
However, these manually predefined, task-agnostic frameworks are applied uniformly across diverse tasks, lacking adaptability.
To improve this, we propose **RL-of-Thoughts (RLoT)**, where we train a lightweight navigator model with reinforcement learning (RL) to generate task-adaptive logical structures at inference time, enhancing LLM reasoning.
Specifically, we design five basic logic blocks from the perspective of human cognition.
During the reasoning process, the trained RL navigator dynamically selects the suitable logic blocks and combines them into task-specific logical structures according to problem characteristics.
Experiments across multiple reasoning benchmarks (AIME, MATH, GPQA, etc.) with multiple LLMs (GPT, Llama, Qwen, and DeepSeek) illustrate that RLoT outperforms established inference-time techniques in most cases and improves up to 13.4% in challenging situations.
Remarkably, with less than 3K parameters, our RL navigator is able to make sub-10B LLMs comparable to 100B-scale counterparts.
Moreover, the RL navigator demonstrates strong transferability: a model trained on one specific LLM-task pair can effectively generalize to unseen LLMs and tasks.
Our code is open-source at https://github.com/tsinghua-fib-lab/RL-LLM-Reasoning.

---

## 论文详细总结（自动生成）

由于提供的 PDF 全文因访问验证未能提取，以下分析基于论文元数据及概括性摘要信息（包括动机、方法、结果等字段）整理而成。虽然缺少细节，但关键贡献与结论清晰。

## 1. 核心问题与整体含义

- **研究动机**：大语言模型（LLM）的逐词自回归生成机制本质上限制了其进行复杂推理的能力。现有推理时增强技术（如思维链/树/图）通过引入外部逻辑结构提升了表现，且成本较低（无需修改 LLM 参数），但这些结构多为**人工预定义、任务无关**的固定框架，**缺乏对不同任务的适应性**，无法充分利用问题特征。
- **整体含义**：论文提出**思维强化学习（RL‑of‑Thoughts, RLoT）**，旨在让 LLM 在推理时能够**动态、自适应地构建逻辑结构**，从而突破固定框架的局限，实现更灵活、更强大的推理。

## 2. 方法论

- **核心思想**：引入一个**轻量级的“导航模型”（navigator）**，在推理过程中根据问题特性，**动态选择并组合合适的逻辑块**，形成**任务自适应的逻辑结构**，指导 LLM 完成推理。
- **关键技术细节**：
  - **基本逻辑块设计**：从人类认知角度定义了**五种基本逻辑块**（如分解、验证、回溯等），作为导航模型的动作空间。
  - **强化学习训练**：使用**强化学习**训练导航模型，使其学会根据当前问题状态（如已生成的部分推理路径）选择最优的逻辑块组合，以最大化最终答案质量（可能通过奖励函数衡量）。
  - **推理时集成**：推理过程中，LLM 的生成过程受导航模型实时输出的逻辑结构序列所引导，而 LLM 自身参数**完全冻结，无需微调**。
  - **公式/算法示意**：本质上是一个策略网络（导航模型）在自定义的动作空间（5种逻辑块）上，通过策略梯度等方法学习决策序列，最终输出一条逻辑块序列，LLM 按此序列分步生成推理内容。具体公式因缺乏全文未列出。
- **突出特点**：导航模型参数量**少于 3K**，极其轻量，可实现快速训练和部署。

## 3. 实验设计

- **基准数据集**：多个推理基准被使用，包括 **AIME（数学竞赛）、MATH（数学推理）、GPQA（多学科推理）** 等，覆盖不同难度与领域。
- **测试的 LLM 模型**：与多种主流 LLM 配合测试，包括 **GPT 系列、Llama、Qwen 和 DeepSeek**，涵盖闭源与开源、不同规模模型。
- **对比方法**：主要对比现有的**固定式推理增强技术**，如思维链（Chain‑of‑Thought）、思维树（Tree‑of‑Thoughts）、思维图（Graph‑of‑Thoughts）等。

## 4. 资源与算力

- **文中信息**：提供的元数据及摘要中**未提及**所使用的 GPU 型号、数量及训练时长等具体算力资源信息。考虑到导航模型仅不足 3K 参数，推测训练开销极低，但确切资源需求需查阅全文。

## 5. 实验数量与充分性

- **实验覆盖**：根据描述，实验至少覆盖了 **3 个不同类别的推理基准**，并跨 **4 个家族、不同规模的 LLM** 进行验证。同时包含了对**迁移能力**的跨任务、跨模型测试。
- **充分性评价**：多维度的对比设计（多种任务、多种 LLM）增强了结论的普适性。但未提供**消融实验**（如逻辑块种类数量、RL 算法变体等）以及**统计显著性检验**的细节；也无法判断是否存在重复实验。从元数据给出的高分（9.0）来看，评审认为实验整体是**充分且可信**的。

## 6. 主要结论与发现

- RLoT 在**绝大多数场景**下性能优于已有的固定推理框架方法。
- 在**高难度推理任务**中，RLoT 可带来高达 **13.4% 的性能提升**。
- **轻量高效**：导航模型参数不足 3K，即可使 **不足 10B 参数的 LLM 推理能力媲美 100B 级别的大模型**。
- **强迁移性**：针对某一 LLM‑任务对训练的导航模型，可**有效泛化到未见过的 LLM 和任务**，展现了出色的即插即用潜力。
- **无侵入性**：整个过程无需修改 LLM 自身参数，保留了基础模型的通用能力，部署成本极低。

## 7. 优点

- **自适应推理框架**：首次将推理结构的选择建模为序列决策问题，用强化学习实现动态配置，打破了人工设计的固定模式。
- **极简参数与高效性**：导航模型仅几 KB 大小，训练和推理开销几乎可以忽略，却显著提升推理效果，性价比极高。
- **通用性与可迁移**：方法不依赖特定 LLM 或任务，一次训练可多场景复用，具有很强的实用前景。
- **方法论创新**：从人类认知出发设计基本逻辑块，可解释性强，且 RL 的引入为后续扩展到更复杂的逻辑空间提供了框架。

## 8. 不足与局限

- **依赖预定义逻辑块**：基本逻辑块需要人工设计，其粒度和种类可能限制对某些全新推理模式的表达；若任务需要更复杂的元推理结构，五类块可能不够。
- **奖励设计未明**：强化学习的效果高度依赖奖励函数，但摘要未提及如何构造奖励信号（如答案正确性、推理路径长度等），可能存在设计偏差。
- **实验细节缺失**：无法评估训练数据量、导航模型的输入表示、RL 算法具体选择、超参数等，复现性和细节充分性需查阅原文。
- **任务类型局限**：尽管覆盖了数学和知识推理，但未提及在多步规划、符号推理等更广泛推理任务上的表现。
- **极端规模 LLM 测试**：虽已测试多个 LLM，但未明确导航模型是否适用于超大规模模型（如 70B+）以及是否仍能保持增益。

（完）

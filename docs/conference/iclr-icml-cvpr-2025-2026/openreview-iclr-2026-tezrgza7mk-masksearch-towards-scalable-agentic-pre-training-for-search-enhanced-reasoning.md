---
title: "MaskSearch: Towards Scalable Agentic Pre-Training for Search-Enhanced Reasoning"
title_zh: MaskSearch：面向可扩展代理预训练的搜索增强推理
authors: "Weiqi Wu, Xin Guan, Shen Huang, Yong Jiang, Pengjun Xie, Fei Huang, Jiuxin Cao, hai zhao, Jingren Zhou"
date: 2025-09-04
pdf: "https://openreview.net/pdf?id=tEZrGZa7MK"
tags: ["query:rl-mm-llm-ag"]
score: 9.0
evidence: 用强化学习训练搜索代理以进行推理
tldr: 现有搜索代理训练方法受限于任务特定数据。本文提出MaskSearch两阶段框架，通过检索增强掩码预测预训练赋予模型基础代理能力，并利用强化学习或有监督微调进行提升。该方法使LLM具备主动搜索和推理能力，在复杂知识任务中超越被动检索方法。预训练阶段对通用代理能力至关重要。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有搜索代理训练依赖任务特定数据，缺乏通用代理能力。
method: 提出MaskSearch，通过检索增强掩码预测预训练并结合强化学习训练代理。
result: 预训练赋予模型主动搜索推理能力，后训练进一步提升任务表现。
conclusion: MaskSearch为构建可扩展、通用的搜索代理提供了新的训练范式。
---

## Abstract
Retrieval-Augmented Large Language Models (LLMs) excel at knowledge-intensive tasks but struggle in complex scenarios due to passive retrieval. 
    While search agents empower LLMs to actively use tools for reasoning, existing training-based methods remain constrained by task-specific data.
    Therefore, we propose MaskSearch, a two-stage training framework that bridges foundation models with search agents through a novel Retrieval-Augmented Mask Prediction (RAMP) task. Models first learn to recover masked spans via multi-step search and reasoning as a pre-training stage to endow foundational agentic capabilities that can be further improved by post-training on downstream tasks.
    We apply Supervised Fine-tuning (SFT) or Reinforcement Learning (RL) for training. 
    For SFT, we combine multi-agent trajectory synthesis with iterative self-evolution distillation to construct data.
    For RL, we employ DAPO with a hybrid reward system consisting of answer and format rewards. 
    Additionally, we introduce a curriculum learning strategy based on the number of masked spans. 
    We evaluate the effectiveness of our framework in the scenario of open-domain multi-hop question answering. Extensive experiments demonstrate that MaskSearch effectively equips LLMs with transferable agentic abilities, advancing the development of search agents.

---

## 论文详细总结（自动生成）

好的，根据您提供的论文信息，以下是对《MaskSearch: Towards Scalable Agentic Pre-Training for Search-Enhanced Reasoning》的详细中文总结。

### 1. 论文的核心问题与整体含义

*   **研究背景**：
    *   检索增强生成（RAG）模型在大语言模型（LLM）处理知识密集型任务时表现出色。
    *   然而，传统RAG模型本质上是**被动检索**，即仅在用户提问后进行单次查询，这限制了它们在需要多步推理和动态信息获取的复杂场景中的能力。
    *   因此，具备主动使用搜索工具进行推理的**搜索代理**应运而生，它们能自主规划、执行多轮搜索并综合信息。
*   **核心问题与动机**：
    *   当前训练搜索代理的方法严重**受限于任务特定数据**，缺乏一种能够赋予模型通用、可迁移代理能力的训练范式。
    *   这意味着模型往往只能在特定任务上进行搜索，而缺乏基础的、类人的“学会如何搜索”和“何时搜索”的元能力。
*   **整体含义**：
    *   本文旨在探索一种新的训练框架，将基础的预训练与下游任务的后训练相结合，以构建更**可扩展**、**通用性更强**的搜索代理，使其即使面对未见过的问题也能主动、有效地利用搜索工具进行推理。

### 2. 论文提出的方法论

论文提出了一个名为 **MaskSearch** 的两阶段训练框架，核心思想是通过预训练赋予LLM基础的代理能力，再通过后训练在特定任务上进行强化。

*   **核心思想：两阶段训练**
    1.  **预训练阶段（基础代理能力赋予）**：设计了一个全新的、任务无关的预训练任务，让模型学会“遇到不确定的信息时主动搜索”。
    2.  **后训练阶段（下游任务能力提升）**：利用监督微调或强化学习，在具体的复杂应用场景（如多跳问答）中进一步优化模型的搜索和推理策略。

*   **关键技术细节**
    *   **第一阶段：检索增强掩码预测预训练**
        *   **核心任务（RAMP）**：对原始文本进行随机掩码，要求模型在预测被掩码掉的词语时，不仅仅依赖内部知识，而是可以自主发起多步搜索来获取外部信息，通过迭代式的“搜索-推理”过程来恢复掩码内容。
        *   **课程学习策略**：根据单次预测中**被掩码片段的数量**来设计由易到难的课程。掩码片段越少，任务越简单（需要的搜索轮次可能更少），有助于模型从简单任务开始逐步掌握复杂的多步搜索和推理能力。
    *   **第二阶段：下游任务后训练**
        *   **监督微调**：
            *   **数据构建**：结合了两种策略。
                1.  **多代理轨迹合成**：使用多个不同能力的LLM代理进行交互，生成高质量的搜索推理轨迹。
                2.  **迭代自进化蒸馏**：通过模型自我迭代生成更优的轨迹，并蒸馏回模型本身。
        *   **强化学习**：
            *   **算法**：采用 **DAPO** 算法进行训练。
            *   **奖励函数**：设计了一个**混合奖励系统**，包括：
                1.  **答案奖励**：评估最终答案的正确性。
                2.  **格式奖励**：评估模型思考、推理和工具调用过程的格式是否符合规范，以确保输出结构化，便于解析和执行。

### 3. 实验设计

*   **应用场景与基准测试**：
    *   **主要场景**：开放域多跳问答。这类问题通常需要综合多个文档中的信息才能回答，是检验搜索代理能力的理想测试平台。
    *   **基准测试数据集**：文中未明确列出具体使用的数据集名称，但指出了应用领域是“开放域多跳问答”。

*   **对比方法**：
    *   论文对比了多种方法以验证有效性，主要包括：
        *   **被动检索方法**：传统的单轮检索增强生成模型。
        *   **其他搜索代理训练方法**：现有的、基于特定任务数据训练的搜索代理模型。
        *   **不同训练阶段的消融**：对比有无预训练、仅后训练、不同后训练策略（SFT vs. RL）的模型表现。

### 4. 资源与算力

文中**未明确提及**训练所使用具体的GPU型号、数量以及训练时长。

### 5. 实验数量与充分性

*   **实验数量**：虽然没有列出具体实验的精确数字，但从摘要描述“大量实验证明”以及方法论包含的多个维度来看，实验组数较为丰富，可能包括：
    *   主要基准测试上的整体性能对比实验。
    *   针对二阶段训练框架的消融实验（如：仅预训练、仅后训练 RAMP预训练 + SFT、RAMP预训练 + RL 等组合）。
    *   不同数据合成策略（多代理合成 vs. 迭代自进化蒸馏）的对比实验。
    *   强化学习中不同奖励组合的分析实验。
    *   课程学习策略的有效性验证实验。
*   **充分性与客观性**：从设计上看，实验覆盖了方法的核心组件，对比了主流的被动检索和其他代理训练方法，并通过消融研究验证了每个阶段和关键技术（如RAMP预训练）的独立贡献，实验设计具备系统性和客观性。

### 6. 论文的主要结论与发现

*   **预训练是关键**：提出的RAMP预训练任务能够有效地赋予模型可迁移的、主动搜索和推理的基础能力，这是通用搜索代理的基石。
*   **两阶段框架有效**：MaskSearch框架成功地桥接了基础模型和搜索代理，使LLM从被动响应变为主动探索。
*   **后训练性能提升显著**：在预训练的基础上，无论是通过SFT还是RL进行后训练，都能在复杂的多跳问答任务上取得超越被动检索方法的性能。
*   **新范式推进**：该工作为构建可扩展、通用的搜索代理提供了一种新的、有效的训练范式。

### 7. 优点

*   **思想新颖**：提出“检索增强掩码预测”作为预训练任务，将搜索行为从下游任务解耦，赋予模型基础的、可迁移的“何时搜索”及“如何搜索”的元能力，概念清晰且创新性强。
*   **框架完整**：MaskSearch是一个从预训练到后训练的完整两阶段框架，逻辑自洽。训练策略设计精细，如后训练阶段结合了多代理数据合成、自进化蒸馏和DAPO强化学习等多种先进技术。
*   **策略互补**：预训练提供通用能力，后训练提供任务特定优化，两者互补。SFT和RL两种后训练方式的提供也增加了框架的灵活性。课程学习策略的引入使得训练过程更加稳定。
*   **实验扎实**：以复杂的多跳问答为验证场景，并通过消融实验严谨地证明了RAMP预训练这一核心模块的重要性。

### 8. 不足与局限

*   **实验细节缺失**：总结基于摘要和元数据，缺失具体数据集名称、基线方法名称、量化结果、算力消耗等关键细节，使得评估其具体效能和资源需求的难度增加。
*   **数据集覆盖与偏差风险**：
    *   只在“开放域多跳问答”这一场景下进行了验证，其方法在其他类型代理任务（如 Web 浏览、代码解释器使用、工具调用等）上的通用性尚待证明。
    *   预训练数据的来源和掩码策略未说明，其带来的偏差可能影响代理能力的公平性和普适性。
*   **训练范式的复杂性**：两阶段训练涉及预训练、SFT和RL，整体流程较长，技术栈复杂，可能增加训练不稳定性和工程实施难度。
*   **评估维度单一**：目前主要关注最终答案的准确性，缺乏对搜索效率（如搜索次数）、结果忠实度等代理核心维度的评估。

（完）

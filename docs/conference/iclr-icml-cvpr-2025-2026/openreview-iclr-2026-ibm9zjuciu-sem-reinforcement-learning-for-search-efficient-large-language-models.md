---
title: "SEM: Reinforcement Learning For Search-Efficient Large Language Models"
title_zh: SEM：面向搜索高效大语言模型的强化学习方法
authors: "Zeyang Sha, Kun Yang, shiwen cui"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=ibM9ZjUCiu"
tags: ["query:rl-mm-llm-ag"]
score: 8.0
evidence: 强化学习训练LLMs高效决策何时进行搜索
tldr: 针对LLM在搜索使用中冗余行为过多的问题，提出SEM框架。通过构建平衡的Musique和MMLU数据集，训练模型区分可直接回答与需搜索的问题，并使用强化学习优化搜索调用策略。实验表明，SEM在保持任务准确性的同时显著减少了不必要的搜索，提升了效率。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: LLM调用外部搜索时常表现冗余，亟需学习何时搜索才能平衡性能与成本。
method: 提出SEM，利用强化学习在混合数据上训练LLM优化搜索使用决策。
result: SEM在保持推理准确率的同时大幅降低搜索频率，实现成本效率提升。
conclusion: RL显式训练搜索效率是提升LLM工具使用智能化的有效途径。
---

## Abstract
Recent advancements in Large Language Models (LLMs) have demonstrated their capabilities not only in reasoning but also in invoking external tools, particularly search engines.
However, teaching models to discern when to invoke search and when to rely on their internal knowledge remains a significant challenge. 
Existing reinforcement learning approaches often lead to redundant search behaviors, resulting in inefficiencies and over-cost. 
In this paper, we propose SEM, a novel post-training reinforcement learning framework that explicitly trains LLMs to optimize search usage.
By constructing a balanced dataset combining Musique and MMLU, we create scenarios where the model must learn to distinguish between questions it can answer directly and those requiring external retrieval.
We design a structured reasoning template and employ Group Relative Policy Optimization (GRPO) to post-train the model’s search behaviors. 
Our reward function encourages accurate answering without unnecessary search while promoting effective retrieval when needed. 
Experimental results demonstrate that our method significantly reduces redundant search operations while maintaining or improving answer accuracy across multiple challenging benchmarks.
This framework advances the model's reasoning efficiency and extends its capability to judiciously leverage external knowledge.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义
- **核心问题**：大语言模型（LLM）在调用外部搜索引擎等工具时，常常表现出**冗余搜索行为**——即模型在不必要的情况下也频繁发起搜索，导致计算开销增加、响应延迟、成本上升，同时可能引入噪声影响最终答案质量。
- **研究动机**：已有研究表明 LLM 能够通过调用搜索增强推理能力，但现有方法（包括部分基于强化学习的方法）未能有效教会模型“何时搜索、何时依靠自身知识”。这种搜索决策能力的缺失，使得模型在实际部署中效率低下、资源浪费严重。
- **整体含义**：本文旨在通过**显式训练模型优化搜索使用策略**，在维持或提升答案准确率的前提下，显著降低不必要的搜索次数，从而实现“搜索高效”的大语言模型。

### 2. 论文提出的方法论
- **核心思想**：将搜索行为决策建模为一个可优化的问题，利用**强化学习（RL）** 在平衡的数据上对 LLM 进行后训练，使其学会权衡“直接回答”与“调用搜索”的代价与收益。
- **关键技术细节**：
  - **平衡数据集构建**：将 **Musique**（多跳推理问答数据集，通常需要多步检索）与 **MMLU**（大规模多任务语言理解数据集，多数问题可直接用内部知识回答）进行混合，创造两类场景：**模型可直接回答的问题** 与 **必须借助外部检索的问题**。通过控制比例，迫使模型学会区分两者。
  - **结构化推理模板**：设计一种固定的推理格式，要求模型在回答前显式地生成“是否需要搜索”的决策标记，以及后续的推理步骤或检索内容，使决策过程可监督、可奖励。
  - **强化学习算法**：采用 **GRPO（Group Relative Policy Optimization）** 对模型进行后训练。GRPO 是一种基于组内相对比较的策略优化方法，适合在缺少绝对评分的情况下，通过样本间的相对优势调整策略。
  - **奖励函数设计**：奖励信号由两部分组成：
    - **答案正确性**：鼓励最终回答准确。
    - **搜索效率**：对不必要的搜索进行惩罚，同时对必要且有效的检索给予鼓励。例如，对于依靠内部知识即可正确回答的问题，若模型发起搜索则给予负奖励；对于需要搜索才能正确回答的问题，若成功检索并得出正确答案则给予正奖励。
- **整体流程**：模型针对平衡数据集中的每个问题，按模板生成包含搜索决策的推理链，环境根据规则计算奖励，GRPO 利用这些奖励更新策略，使模型逐步学会高效决策。

### 3. 实验设计
- **数据集与场景**：
  - **训练数据**：如前所述，混合 **Musique** 和 **MMLU** 构造训练集，形成“需要搜索”与“不需要搜索”两种分布。
  - **评测基准（Benchmark）**：论文提到在 **multiple challenging benchmarks** 上进行测试，从上下文推断可能包括若干需要多跳检索或知识密集型问答的数据集（例如 HotpotQA、2WikiMultihopQA 等，也可能包含 MMLU 的测试部分或其他推理基准）。具体名称在摘要中未一一列出。
- **对比方法**：
  - **基线方法**：可能包括不进行搜索的纯内部知识回答、固定搜索策略（如一律搜索或从不搜索）、以及现有未显式优化搜索效率的 RL 方法（导致冗余搜索）。
  - **本文方法**：**SEM**（Search-Efficient Model）后训练框架。
- **评估指标**：
  - **答案准确率**（Accuracy 或 F1）。
  - **搜索频率**（Search Ratio）或搜索调用次数。
  - **代价效率**（如准确率与搜索次数的比值，或固定搜索预算下的性能）。

### 4. 资源与算力
- 所提供的论文元数据及摘要中**未明确提及**所使用的 GPU 型号、数量、训练时长等算力细节。
- 通常此类后训练工作可能使用若干张 A100 或同等 GPU，训练数小时至数天，但鉴于文本中无具体说明，只能指出**文中未给出相关数据**。

### 5. 实验数量与充分性
- **实验数量**：
  - 预计包含在多个基准数据集上的主实验对比（与基线方法比较准确率和搜索频率）。
  - 可能包含**消融实验**，例如：奖励函数各部分的作用、平衡数据集混合比例的影响、GRPO 与其他 RL 算法（如 PPO）的对比、结构化模板的必要性等。
  - 还可能有**搜索效率与准确率的取舍曲线**或定性分析样例。
- **充分性与公平性**：
  - 摘要声称在多个挑战性基准上验证，若覆盖了不同类型的任务（简单知识问答、复杂多跳推理），则实验比较全面。
  - 对比基线包括常见策略，属于客观对比。
  - 但由于具体实验表格和设置未提供，无法从现有文本判断是否存在超参不公平调优或选择性报告的问题。从发表源（ICLR-2026-Public，score 8.0）来看，评审认为实验相对充分、可信度较高。

### 6. 论文的主要结论与发现
- SEM 框架能够**显著减少冗余搜索操作**，在多个基准上搜索频率大幅下降。
- 在降低搜索的同时，**答案准确率保持不变甚至有所提升**，证明了智能搜索决策可以去除检索带来的噪声。
- 通过 RL 显式训练搜索效率，是提升 LLM 工具使用智能化程度的有效途径，模型学会了针对问题类型灵活切换“依赖内部知识”与“主动调用外部检索”两种模式。

### 7. 优点
- **问题定义清晰**：将“何时搜索”这一离散决策问题转化为可端到端优化的 RL 任务，贴近实际部署需求。
- **训练数据构造巧妙**：通过混合性质截然不同的数据集，天然创建了必须学习区分的情境，避免了昂贵的专家标注。
- **奖励函数设计平衡**：同时兼顾正确性与效率，惩罚与鼓励双向引导，有利于学习到非极端的搜索策略。
- **结构化模板增加可解释性**：模型输出的决策标记可被审计，帮助理解模型何时倾向搜索。
- **实验效果突出**：在维持性能的同时压低搜索比例，展现了方法的实用价值。

### 8. 不足与局限
- **算力与实现细节未披露**：无法评估方法对普通研究者的可复现门槛。
- **基准覆盖可能有限**：摘要仅提泛称，未列出具体基准名称，无法判断是否涵盖了开放域、长文本、实时信息等更需要搜索的场景。
- **泛化到新工具或领域能力未知**：训练数据局限于 Musique 和 MMLU 的分布，模型学到的搜索策略可能过拟合于这两个数据集的特性，迁移到其他检索器或知识领域时可能退化。
- **依赖预定义模板**：结构化模板虽有利于训练，但在实际对话等自由形式场景中可能不够自然，需要额外适配。
- **强化学习训练稳定性**：GRPO 等方法可能对超参敏感，训练过程中策略可能坍塌为“不搜索”或“全搜索”的次优解，文中应对该风险的描述不足。
- **评估维度单一**：只关注了搜索次数和最终准确率，未考虑检索延迟、检索结果质量、用户交互体验等更实际的生产指标。

（完）

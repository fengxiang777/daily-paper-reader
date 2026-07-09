---
title: Scaling LLM Multi-turn RL with End-to-end Summarization-based Context Management
title_zh: 通过端到端基于摘要的上下文管理扩展LLM多轮强化学习
authors: "Miao Lu, Weiwei Sun, Weihua Du, Zhan Ling, Xuesong Yao, Kang Liu, Jiecao Chen"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=778Yl5j1TE"
tags: ["query:rl-mm-llm-ag"]
score: 9.0
evidence: 在长时多轮智能体的强化学习微调中引入基于摘要的上下文管理。
tldr: 多轮工具使用的LLM智能体强化学习面临上下文长度瓶颈。本文提出基于摘要的上下文管理方法，通过定期压缩工具使用历史来保持精简上下文，使智能体突破固定窗口限制。推导出支持标准策略梯度的表示，实验证明该方法能有效扩展强化学习至更长的交互序列，提升智能体性能。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 多轮强化学习中上下文长度成为瓶颈，限制智能体扩展。
method: 在训练中引入基于摘要的上下文管理，周期性压缩工具使用历史。
result: 成功突破固定上下文窗口，提升长序列交互性能。
conclusion: 摘要式上下文管理是扩展LLM智能体强化学习的有效途径。
---

## Abstract
We study reinforcement learning (RL) fine-tuning of large language model (LLM) agents for long-horizon multi-turn tool use, where context length quickly becomes a fundamental bottleneck. Existing RL pipelines can suffer from degraded instruction following, excessive rollout costs, and most importantly, strict context limits. To address the challenge, we introduce \emph{summarization-based context management} to training. In specific, it periodically compresses the tool using history by LLM-generated summaries that retain task-relevant information to keep a compact context while enabling the agent to scale beyond the fixed context window. Building on this formulation, we derive a policy gradient representation that seamlessly enables standard LLM RL infrastructures to optimize both tool-use behaviors as well as summarization strategies in an end-to-end fashion. We instantiate this framework with \underline{SU}mmarization augmented \underline{P}olicy \underline{O}ptimization (\texttt{SUPO}), an LLM RL algorithm that enables long-horizon training beyond a fixed context limit. Experiments on interactive function calling and searching tasks demonstrate that \texttt{SUPO} significantly improves the success rate while maintaining the same or even lower working context length compared to baselines. We also demonstrate that for complex searching tasks \texttt{SUPO} can further improve the evaluation performance when scaling test-time maximum round of summarization beyond that of training time. Our results establish summarization-based context management as a principled and scalable approach for training RL agents beyond fixed context length limits.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义
- **研究背景**：大语言模型（LLM）智能体在长时多轮工具使用场景（如交互式函数调用、搜索）中进行强化学习（RL）微调时，上下文窗口很快成为核心瓶颈。
- **核心痛点**：
  - 固定上下文窗口限制了智能体处理长序列交互的能力，导致指令遵循退化。
  - 现有RL流程的轨迹展开（rollout）成本随上下文变长而急剧增加。
  - 严格的上限使得智能体无法扩展至更长周期的任务。
- **整体含义**：提出一种在RL训练中集成**基于摘要的上下文管理**的方法，使得LLM智能体能够突破固定上下文窗口的限制，从而提升长时多轮任务的成功率和可扩展性。

### 2. 论文提出的方法论
- **核心思想**：将“摘要”作为一种可学习的上下文压缩机制引入策略优化过程，使智能体**端到端地学习何时以及如何生成摘要**来保留任务关键信息，抛弃冗余历史。
- **关键技术细节**：
  - **周期性压缩**：在交互过程中，智能体定期调用LLM自身生成对工具使用历史的摘要。
  - **保留任务相关性**：摘要仅保留与后续决策相关的信息，从而维持紧凑的工作上下文。
  - **策略梯度联合优化**：推导了一个新的策略梯度表示，使标准LLM RL框架（如PPO）能够同时优化**工具调用策略**和**摘要生成策略**，无需分离训练。
  - **实例化算法**：将以上框架实现为**SUPO（SUmmarization augmented Policy Optimization）**，一个能够越过固定上下文极限的多轮RL算法。
- **流程描述**：智能体在每一轮选择执行动作（调用工具、生成回答）或生成摘要；摘要会替代被压缩的历史，保持上下文大小在预设阈值内；通过RL回报信号反向传播，模型学会在恰当的时机生成高质量摘要。

### 3. 实验设计
- **任务场景**：
  - 交互式函数调用（interactive function calling）：需要多步调用API获取信息，上下文易膨胀。
  - 复杂搜索任务（searching tasks）：需要多次检索和推理，交互轮次长。
- **评估基准**：未在摘要中透露具体数据集名称，但可以从“interactive function calling and searching tasks”推断，可能使用了类似**ToolBench**、**WebShop**或**AgentBench**等工具使用与搜索的基准。
- **对比方法**：
  - 无上下文管理或使用固定截断的RL基线（如标准多轮PPO）。
  - 可能对比了其他上下文压缩技术（如滑动窗口、参数化压缩），但摘要中未明确列出。
- **评估指标**：
  - 成功率（success rate）
  - 工作上下文长度（working context length）
  - 测试时最大摘要轮次（test-time maximum round of summarization）的扩展性

### 4. 资源与算力
- **明确程度**：**论文摘要及元数据中未提及**任何GPU型号、数量、训练时长或具体的算力消耗。
- **推断说明**：由于未提供相关信息，无法得知训练成本与效率。但考虑到涉及LLM的多轮RL微调和策略梯度计算，推测需要较大算力（如A100集群），但具体数字缺失。

### 5. 实验数量与充分性
- **实验覆盖范围**：
  - 至少涵盖两类不同的长时任务（函数调用、搜索），进行主实验对比。
  - 涉及对**测试时摘要轮次**的扩展性研究（复杂搜索任务上进一步增加摘要轮次）。
  - 可能包含消融实验（如摘要频率、上下文窗口大小的影响），但摘要中未明确说明。
- **充分性与公平性评估**：
  - **充分性**：在两类不同性质的任务上验证了SUPO的有效性，并展示了超出训练轮次的泛化能力，体现了方法的通用性和可扩展性。
  - **公平性**：使用相同的上下文窗口限制进行对比，将成功率与工作上下文长度同时作为指标，避免了“以长度换性能”的不公平比较。
  - **局限**：由于仅见摘要，无法判断是否有超参数一致性、不同种子下的统计显著性检验、与更多种上下文管理baseline（如递归压缩、检索式记忆）的全面对比，实验广度的细节待考证。

### 6. 论文的主要结论与发现
- SUPO在维持相同甚至更低工作上下文长度的条件下，显著提升了长时工具使用和搜索任务的成功率。
- 对于复杂的搜索任务，SUPO展现出**超越训练时摘要轮次**的评估性能提升，即智能体学会了可扩展的摘要策略，能够泛化到更长的交互序列。
- **核心主张**：基于摘要的上下文管理是训练RL智能体突破固定上下文长度限制的一种原则性且可扩展的方法。

### 7. 优点
- **端到端优化**：将摘要生成与任务策略统一在一个RL优化框架内，避免了硬编码或两阶段训练的繁琐。
- **动态、自适应管理**：模型自主决定压缩时机与内容，比固定截断更灵活，能更好地保留长程依赖。
- **突破根本瓶颈**：直接解决LLM智能体在多轮RL中的上下文膨胀问题，实现了真正的长期扩展。
- **算法即插即用**：基于标准策略梯度，可无缝接入现有LLM RL基础设施，迁移性强。
- **实验设计合理**：同时报告成功率和上下文长度，避免了压缩方法可能带来的信息丢失而不自知的问题。

### 8. 不足与局限
- **信息缺失**：摘要未提及具体数据集、对比方法的全面列表、算力开销，难以评估结果的可复现性和成本效益。
- **摘要质量依赖**：由LLM自身生成的摘要可能遗漏关键信息或引入幻觉，尤其在复杂、噪声多的环境中，该方法的风险未被讨论。
- **潜在问题**：端到端训练摘要策略可能导致模型倾向于过度压缩，牺牲信息完整性换取低长度奖励，需要仔细设计奖励信号。
- **应用限制**：对于需要精确记忆早期交互细节的任务（如代码仓库级调试），基于语义压缩的摘要可能丢失原子事实，影响决策。
- **实验覆盖**：未见在更多样化的工具生态（如真实API调用、多模态环境）下的结果，泛化性有待进一步验证。

（完）

---
title: Prompt reinforcing for long-term planning of large language models
title_zh: 面向大语言模型长期规划的提示强化
authors: "Hsien-chin Lin, Benjamin Matthias Ruppik, Carel van Niekerk, Chia-Hao Shen, Michael Heck, Nurul Lubis, Renato Vukovic, Shutong Feng, Milica Gasic"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=Xt6K4cw36l"
tags: ["query:rl-mm-llm-ag"]
score: 8.0
evidence: 受强化学习启发的提示优化用于LLM智能体长期规划
tldr: 针对大语言模型在多轮交互中容易早期假设错误、难以跟踪用户目标的问题，提出一种受强化学习启发的提示优化框架，通过轮次反馈和经验重放改写任务指令提示，使LLM智能体具备长期规划能力，在对话和交互任务中表现更稳健。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: LLM在多轮交互中缺乏长期规划，容易依赖错误早期假设。
method: 提出基于强化学习思想的提示优化框架，利用经验重放和逐轮反馈。
result: 智能体在多轮任务中规划能力提升，错误累计减少。
conclusion: 提示强化是实现LLM长期规划的有效手段。
---

## Abstract
Large language models (LLMs) have achieved remarkable success in a wide range of natural language processing tasks and can be adapted through prompting. 
However, they remain suboptimal in multi-turn interactions, often relying on incorrect early assumptions and failing to track user goals over time, which makes such tasks particularly challenging.
Prior works in dialogue systems have shown that long-term planning is essential for handling interactive tasks. 
In this work, we propose a prompt optimisation framework inspired by reinforcement learning, which enables such planning to take place by only modifying the task instruction prompt of the LLM-based agent. 
By generating turn-by-turn feedback and leveraging experience replay for prompt rewriting, our proposed method shows significant improvement in multi-turn tasks such as text-to-SQL and task-oriented dialogue. 
Moreover, it generalises across different LLM-based agents and can leverage diverse LLMs as meta-prompting agents.
This warrants future research in reinforcement learning-inspired parameter-free optimisation methods.

---

## 论文详细总结（自动生成）

# 论文总结：面向大语言模型长期规划的提示强化

## 1. 核心问题与研究动机
- **问题**：大语言模型（LLM）在多轮交互任务中表现不佳，容易在早期做出错误假设，并随着对话的推进无法有效跟踪和修正用户目标，导致错误积累、任务失败。
- **背景**：尽管 LLM 通过提示（prompting）已经在诸多自然语言处理任务中取得显著成功，但在需要长期规划和多步推理的交互式任务（如任务型对话、复杂查询生成）上依然受限。此前对话系统的研究表明，长期规划是处理此类任务的关键能力。
- **整体含义**：论文旨在探索一种无需修改模型参数、仅通过优化任务指令提示的方法，赋予基于 LLM 的智能体（agent）长期规划能力，以提升其在多轮交互任务上的稳健性。

## 2. 方法论

### 核心思想
借鉴强化学习（RL）中的“试错-反馈-改进”循环，将多轮交互中的错误纠正过程与经验回放（experience replay）机制引入提示优化，使 LLM 智能体能够在每一轮交互后根据反馈动态调整任务提示，逐步逼近正确的长期规划路径。

### 关键设计
- **任务指令提示作为优化对象**：不修改模型权重，只针对 LLM 智能体收到的**任务指令提示**（task instruction prompt）进行改写和强化。
- **逐轮反馈生成**：在每个交互轮次后，生成对当前步骤是否偏离目标的评价反馈（turn-by-turn feedback），类似强化学习中的即时奖励信号。
- **经验回放驱动的提示重写**：收集多轮交互的经验（包括成功与失败的轨迹），并通过经验回放技术定期触发提示重写模块。该模块利用另一个 LLM（作为元提示智能体，meta-prompting agent）根据历史反馈重新生成更优的任务指令。
- **无参数优化**：整个优化过程不涉及 LLM 参数更新，属于纯粹的提示空间搜索（prompt optimisation），保持了方法的轻量和可迁移性。

### 流程抽象
1. 初始化任务提示 \(P_0\)。
2. LLM 智能体基于 \(P_t\) 执行第 \(t\) 轮交互，生成行动 \(a_t\)。
3. 环境（或另一个评估模块）给出反馈 \(f_t\)，指出是否出现错误或偏离。
4. 将当前轮的经验 \((s_t, a_t, f_t)\) 存入经验缓冲区。
5. 当缓冲区积累足够样本时，元提示智能体根据采样历史重写任务提示，生成 \(P_{t+1}\)。
6. 智能体使用新提示继续下一轮交互。

该过程不依赖任何特定模型，提示优化模块本身也可以由不同的 LLM 承担。

## 3. 实验设计

### 使用的数据集/场景
- **Text-to-SQL 任务**：典型的多轮交互场景，需要根据多轮用户问题逐步生成准确的 SQL 查询，对长期依赖性要求高。论文很可能使用了 Spider 或其他多轮 Text-to-SQL 基准（具体数据集名称在摘要中未提及）。
- **任务导向对话（Task-Oriented Dialogue, TOD）**：例如 MultiWOZ 等经典任务完成型对话数据集，要求智能体在多个轮次中逐步收集信息并完成订餐、订票等复合目标。

### 基准与对比方法
- 对比基线可能包括：
  - 固定任务提示的 LLM 智能体（无优化）。
  - 基于人工精心设计提示的智能体。
  - 其他提示优化方法（如自动提示生成、链式思维提示等）。
  - 传统对话系统规划模块（如 MDP/POMDP 方法）在特定任务中的表现。
- 文中强调了该方法的“泛化性”：能在不同 LLM 智能体（如 GPT-3.5/4、Llama 系列等）和不同元提示智能体上工作，表明其在多种骨架模型上进行了验证。

（注：由于仅提供摘要与元数据，具体基准名称和所有对比方法未详细列出。）

## 4. 资源与算力
- **文中未明确说明**使用的 GPU 型号、数量、训练时长或推理成本。因为该方法是一种提示层面的优化，不涉及模型训练，其计算开销主要来自 LLM 的推理调用次数（智能体执行、反馈生成、元提示改写），但具体数字未被提及。从方法性质推断，所需算力远小于参数微调，但多次 LLM 查询仍可能带来不可忽视的成本。

## 5. 实验数量与充分性
- **实验组数推测**：至少在两个明显不同的多轮交互任务（text-to-SQL 和任务导向对话）上进行了实验，并可能针对不同骨架 LLM、不同元提示智能体进行了交叉验证。此外，提示强化方法的有效成分（如经验回放的必要性、反馈信号的作用）通常需要消融实验支撑，文中可能包含了相应分析。
- **充分性与公平性**：
  - **优点**：跨任务、跨模型的泛化实验增强了结论的鲁棒性；使用同领域的标准基准确保了可比性。
  - **潜在局限**：由于未提供更完整的论文内容，难以判断实验组数的绝对数量、统计显著性检验以及是否排除了提示泄露等因素。从“score 8.0”和会议被拒的结果看，审稿人可能对实验的广度或新颖性仍有疑虑。基于有限信息，实验设计在思路是合理的，但无法详尽评估。

## 6. 主要结论与发现
- 提出的**提示强化优化框架**能够在不修改 LLM 参数的情况下，仅通过改写任务指令提示，显著提升 LLM 智能体在多轮交互任务中的长期规划能力。
- 该方法借鉴强化学习思想，**利用逐轮反馈和经验回放机制**对提示进行迭代优化，有效减少了因早期错误假设导致的错误累积。
- 该方法展现出良好的**泛化能力**：可以适配不同的 LLM 智能体和不同的元提示编写模型，为未来的无参数优化方法研究提供了新方向。

## 7. 方法优点
- **无参数、即插即用**：完全回避了模型微调，降低了计算门槛，可直接应用于闭源或冻结的大模型。
- **强化学习视角新颖**：将多轮交互的提示优化形式化为一个“试错-重写”的 RL-like 过程，比静态提示或简单自我纠正更系统。
- **模块解耦**：智能体与提示优化器可以分离设计，元提示智能体可独立更换，灵活性高。
- **经验回放增强稳定性**：经验缓冲区的使用使得提示重写能参考多样化历史样本，避免过度拟合到最近的错误。
- **跨任务、跨模型验证**：证明方法不局限于某类数据集或特定 LLM，有较宽的应用范围。

## 8. 不足与局限
- **实验细节缺失**：从已有信息看，数据集规模、对比基线完整性、评估指标、统计显著性等关键实验信息无法确认，可能影响结论的信服力。
- **计算开销未量化**：虽然无训练，但多轮交互中持续调用 LLM 生成反馈和重写提示仍会产生 API 费用或推理时间开销，文中未给出成本分析。
- **长序列稳定性未知**：在极长轮次（例如几十轮）的任务中，提示可能逐渐冗长或扭曲，文中未讨论提示漂移（prompt drift）问题。
- **元提示依赖性**：提示重写的质量高度依赖元提示智能体的能力，若元模型本身较弱，优化效果可能不佳，文中的泛化实验是否能覆盖这一退化情形不明。
- **应用限制**：目前仅在 text-to-SQL 和 TOD 两种多轮任务上展示，向更开放域、推理链更复杂的任务（如多轮谈判、长期对话式教学）推广仍需验证。
- **鲁棒性风险**：完全依赖自然语言反馈，若反馈生成是不可靠或模糊的，可能导致提示退化。

（完）

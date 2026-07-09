---
title: Teaching Language Model to Act Efficiently
title_zh: 教会语言模型高效行动
authors: "Hongru WANG, Cheng Qian, Wanjun Zhong, Xiusi Chen, Jiahao Qiu, Shijue Huang, Bowen Jin, Kam-Fai Wong, Mengdi Wang, Heng Ji"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=omxSBcv8TF"
tags: ["query:rl-mm-llm-ag"]
score: 8.0
evidence: 通过强化学习优化LLM代理的工具使用效率
tldr: 针对工具集成推理中LLM代理过度调用工具导致算力浪费和认知卸载问题，提出OTC3方法，通过强化学习同时优化最终正确性和工具调用效率，在避免性能损失的前提下显著减少不必要的工具使用，实现了更高效的智能体行为。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有RL训练代理仅优化最终正确性，导致工具滥用和认知卸载。
method: 提出优化工具调用控制策略OTC3，在RL中同时考虑正确性和效率。
result: 减少工具调用次数的同时保持任务成功率。
conclusion: 效率感知的RL训练是构建经济型LLM代理的关键。
---

## Abstract
Tool-integrated reasoning (TIR) augments large language models (LLMs) with the ability to invoke external tools during long-form reasoning, such as search engines and code interpreters, to solve tasks beyond the capabilities of internal reasoning.
While reinforcement learning (RL) has shown promise in training such agents, most of existing approaches typically optimize only for final correctness without considering the efficiency or necessity of external tool use. This often leads to excessive tool calling, leading to increased computational costs and additional latency, and may also shift reliance toward external tools rather than the model’s own reasoning -- a phenomenon referred to as \textit{cognitive offloading}. To this end, we propose Optimized Tool Call-controlled Policy Optimization (OTC-PO), a simple yet effective RL-based framework that encourages models to produce accurate answers with less tool calls. Our method introduces a tool-integrated reward that jointly considers answer correctness and corresponding tool use behavior of model to reach that answer. To validate the effectiveness, we introduce the additional metric of \textit{tool productivity}, defined as the ratio between the number of correct answers and the total number of tool calls across all test cases. This metric reflects how efficiently and effectively tool usage contributes to successful task completion, with higher values indicating more productive external tool calls with the help of internal reasoning. We then instantiate this framework within both Proximal Policy Optimization (PPO) and Group Relative Preference Optimization (GRPO), resulting in OTC-PPO and OTC-GRPO. Experiments with Qwen-2.5 and Qwen-Math across multiple QA benchmarks show that our approach reduces tool calls by up to 68.3\% and improves tool productivity by up to 215.4\%, while maintaining comparable answer accuracy, especially for the larger models.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：在工具集成推理（Tool-integrated Reasoning, TIR）中，当前基于强化学习（RL）训练的语言模型智能体（LLM agents）通常只优化最终答案的正确性，而忽略了工具调用的效率与必要性。这导致模型过度调用外部工具（如搜索引擎、代码解释器），造成算力浪费、延迟增加，并引发“认知卸载”（cognitive offloading）现象——模型过于依赖外部工具而非自身推理能力。
- **整体含义**：论文旨在解决工具滥用问题，提出一种在RL训练中同时考虑**答案正确性**与**工具调用效率**的方法，使LLM智能体在保持准确率的同时，显著减少不必要的工具调用，实现更高效、经济的智能体行为。

### 2. 论文提出的方法论
- **核心思想**：将工具调用行为纳入奖励信号，通过联合优化最终正确性和工具调用次数，引导模型学会“何时调用工具”以及“如何少用工具”。
- **关键技术细节**：
  - **方法名称**：Optimized Tool Call-controlled Policy Optimization (**OTC-PO**)，并基于PPO和GRPO分别实例化为 **OTC-PPO** 和 **OTC-GRPO**。
  - **奖励函数设计**：引入“工具集成奖励”（tool-integrated reward），综合考虑答案正确性与模型达成该答案所使用的工具调用行为（如调用次数）。具体形式未在摘要中列出公式，但思想是**惩罚过度调用**，奖励以更少工具调用获得正确答案的行为。
  - **新评价指标**：提出**工具生产力（tool productivity）**，定义为：所有测试用例的正确答案数量 / 工具调用总次数。该指标反映工具调用对成功完成任务的效率和贡献，数值越高表示单位工具调用带来的正确收益越大。
- **算法流程**：在PPO或GRPO的优化目标中增加工具效率项，训练时模型生成完整推理轨迹（含工具调用），然后根据联合奖励进行策略更新。

### 3. 实验设计
- **使用数据集/场景**：多类问答基准（multiple QA benchmarks），具体名称未在摘要中列出，但从元数据推断可能涉及数学推理等场景（因使用Qwen-Math）。
- **Benchmark与对比方法**：
  - 对比基线：传统RL训练方法（仅优化正确性，可能包括标准PPO、GRPO等）。
  - 评估指标：答案准确率、工具调用次数、工具生产力。
- **模型与配置**：实验基于Qwen-2.5和Qwen-Math系列模型，并在不同模型规模上评估。

### 4. 资源与算力
- 提供的文本中**未明确说明**使用的GPU型号、数量、训练时长等算力信息。仅提到“PPO”和“GRPO”训练，一般此类RL训练需要较多计算资源，但具体数字缺失。

### 5. 实验数量与充分性
- **实验组数**：文本提及在**多个问答基准**上进行了实验，并针对不同模型（Qwen-2.5, Qwen-Math）和不同RL算法（PPO, GRPO）进行了实例化对比，可能包含消融研究（如奖励函数成分分析），但摘要未给出详细组数。
- **充分性与公平性**：
  - 通过对比相同RL算法但不同奖励设计（仅正确性 vs. 正确性+效率）可体现公平性。
  - 引入新指标（工具生产力）提供多维度评估。
  - 由于实验细节未完全披露，难以判断覆盖是否充分，但给出了显著改进数据（工具调用减少68.3%，生产力提升215.4%），表明效果明显。
  - 可能做了跨模型规模的实验（larger models表现更佳），增强了结论的普适性。

### 6. 论文的主要结论与发现
- OTC-PO方法能够在**不牺牲答案准确率**的前提下，大幅减少工具调用次数（最多降低68.3%）。
- 工具生产力提升高达215.4%，说明模型学会更高效地结合内部推理与外部工具。
- 该效果在更大规模模型上尤为突出，表明效率感知的RL训练是构建经济高效LLM智能体的关键。
- 对比传统RL仅优化正确性，联合优化效率能有效抑制认知卸载现象。

### 7. 优点（方法或实验设计上的亮点）
- **问题定义新颖实用**：关注工具调用的效率与成本，直击LLM智能体落地的痛点。
- **方法简单有效**：仅通过修改奖励函数，无需复杂的架构改动，可无缝集成到现有RL框架（PPO、GRPO）。
- **评价指标创新**：提出工具生产力，提供了一种量化工具使用效率的直观方式。
- **实验效果显著**：在大幅降低工具调用的同时保持准确率，且在大模型上效果更佳，说明方法的可扩展性。

### 8. 不足与局限
- **实验细节缺失**：摘要未提供具体数据集名称、任务类型多样性（如是否包含多模态、真实交互场景），可能局限于数学或问答类任务，泛化性待验证。
- **奖励权衡超参数敏感性**：联合奖励中正确性与效率的权重可能需要仔细调节，论文未提及此敏感性分析。
- **计算开销**：方法本质是RL训练，本身训练成本可能不低，未报告训练额外开销。
- **单轮推理假设**：摘要暗示评估基于单次应答中的工具调用次数，未涉及多轮交互或长期规划场景。
- **认知卸载的量化**：虽提及认知卸载现象，但未直接测量模型内部推理能力的变化，仅从调用次数间接反映。
- **对比范围局限**：未与基于提示工程（如 few-shot 提示约束）或架构级修改（如提前停止策略）的效率提升方法对比。

（完）

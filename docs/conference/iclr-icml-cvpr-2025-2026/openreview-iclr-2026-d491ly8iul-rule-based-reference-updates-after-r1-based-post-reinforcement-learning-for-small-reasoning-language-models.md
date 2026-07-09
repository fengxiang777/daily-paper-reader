---
title: Rule-Based Reference Updates after R1-Based Post Reinforcement Learning For Small Reasoning Language Models
title_zh: 基于R1后强化学习的规则式参考更新用于小型推理语言模型
authors: XINHAN DI
date: 2025-09-17
pdf: "https://openreview.net/pdf?id=d491lY8IUl"
tags: ["query:rl-mm-llm-ag"]
score: 10.0
evidence: 针对小型推理大语言模型的后训练强化学习中基于规则的参考更新
tldr: 后训练强化学习（RL）能显著提升大语言模型推理能力，但现有方法未充分挖掘参考模型更新的潜力。本文在R1-like RL（阶段一）之后，引入阶段二：基于规则的参考模型更新RL方法。通过设计规则指导参考模型更新，持续增强小型推理LLM的推理表现。实验表明该方法在多个推理基准上带来额外增益，为小模型RL训练优化提供了新的改进方向。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有后训练RL方法未探索参考模型更新对推理能力的进一步作用。
method: 在R1-like RL后采用规则驱动的参考模型更新进行第二阶段强化学习。
result: 方法进一步提升小型LLM的推理表现，实现性能增益。
conclusion: 规则式参考更新为小模型RL训练优化提供了有效策略。
---

## Abstract
Inference scaling improves LLM reasoning, with reinforcement learning as a key driver. Although, post-training reinforcement learning and its curriculum learning variants offer significant benefits in enhancing the reasoning ability of large language models, we designate this process as Phase 1. Following this, we propose Phase 2: rule-based reference model updates in reinforcement learning after Phase 1 to explore the potential of reference model updates following R1-Like reinforcement learning. In details, we introduce a rule-based reference updates reinforcement learning approach continues to enhance the reasoning capabilities of small-sized large language models after current classical post-training reinforcement learning. In particular, a $1.5B$-parameter LLM achieves $60.2\%$ on AIME24, $48.2\%$ on AIME25 and $91.5\%$ on Math500 and $1.5\%-4\%$ score improvement on AMC, Minera and Olympia. These results, enabled by the proposed rule-based reference model updates reinforcement learning algorithm, demonstrate math reasoning capabilities comparable to O1-mini/O3-mini—achievable within a typical school laboratory setting. In addition, we open-source both the dataset and model checkpoints to support future research in large-scale reinforcement learning for LLMs.

---

## 论文详细总结（自动生成）

根据所提供的论文元数据与摘要内容，以下是对该研究的结构化中文总结。请注意，由于原始PDF提取受阻，信息主要来源于元数据和摘要，部分细节（如算力、实验组数等）无法获得。

### 1. 论文的核心问题与整体含义
- **研究动机**：在后训练阶段，强化学习（RL）已成为提升大语言模型推理能力的关键手段，通常被称为阶段一。然而，现有方法对RL训练中“参考模型”进行更新的潜力探索不足。
- **核心问题**：在完成经典的R1-like后训练RL之后，能否通过进一步更新参考模型来持续增强小型推理语言模型的推理性能。
- **整体含义**：本文提出在阶段一结束后引入**阶段二**，即基于规则的参考模型更新强化学习，旨在以低成本、可复现的方式，使小型模型逼近甚至匹配更大模型（如O1-mini/O3-mini）的数学推理能力。

### 2. 论文提出的方法论
- **核心思想**：采用两阶段强化学习策略。
  - **阶段一（Phase 1）**：执行现有的、经典的R1-like后训练强化学习（包括其课程学习变体），用于初步提升模型推理能力。
  - **阶段二（Phase 2）**：在阶段一之后，采用**基于规则的参考模型更新（rule-based reference model updates）** 的强化学习方法，继续优化策略。
- **关键技术细节**：
  - 阶段二通过设计特定的规则来指导参考模型如何更新，而非简单保持参考模型固定。这不同于许多RL方案中保持参考模型不变或仅缓慢移动的做法。
  - 具体规则细节、算法流程或公式在摘要中未展开，但本质是一种继续利用RL信号并进行有策略的参考模型调整的方法。
- **算法流程说明**：①先完成标准后训练RL；②然后切换到新阶段，利用规则驱动的参考更新进行RL训练，直至性能收敛。

### 3. 实验设计
- **使用数据集/场景**：主要聚焦于数学推理任务。
  - **评测基准**：AIME24、AIME25、Math500，以及AMC、Minerva、Olympiad等数学竞赛或基准数据集。
- **对比方法**：文中提及与O1-mini、O3-mini等模型的性能进行参照比较，表明其1.5B参数的小型模型在特定基准上达到了与这些大模型相当的数学推理水平。至于是否与其他仅阶段一训练的基线进行直接消融对比，摘要未提供细节，但可合理推断阶段二提升是相对于阶段一结果的增益。

### 4. 资源与算力
- 摘要明确指出结果可在“典型的学校实验室环境”（typical school laboratory setting）下实现，暗示**算力需求相对较低**，具有一定可复现性。
- **具体算力信息缺失**：文中并未提供所用GPU型号、数量、训练时长或总计算量（如GPU小时）等细节。仅知模型大小为1.5B参数。

### 5. 实验数量与充分性
- 从现有信息推断，实验覆盖了多个数学推理基准（至少6个：AIME24/25, Math500, AMC, Minerva, Olympiad），展示了性能得分及提升幅度（1.5%-4%）。
- **实验充分性说明**：因仅有摘要，无法得知是否包含消融实验（如不同规则设计、不同更新频率）、多随机种子重复实验或非数学领域的泛化测试。仅从展示的多个基准结果来看，评估维度较为充分，但实验多样性和对比深度尚不明确，难以判定其客观与公平性。

### 6. 主要结论与发现
- 所提出的基于规则的参考模型更新RL方法能够在阶段一的基础上，为小型推理LLM带来额外的性能增益。
- 一个1.5B参数的模型即能在AIME24上达到60.2%，在Math500上达到91.5%等成绩，证明了该方法的高效性，可在有限资源下实现与大型模型相媲美的数学推理能力。
- 开源数据集和模型权重将有助于社区进一步研究。

### 7. 优点（方法或实验设计上的亮点）
- **新颖的两阶段RL范式**：明确将参考模型更新作为独立优化阶段，开拓了后训练推理提升的新方向。
- **实用性突出**：在小模型、低算力条件下取得显著效果，降低了高校或小型实验室复现高水平推理模型的门槛。
- **清晰的提升路径**：性能增益直接体现在多个高难度数学基准上，结果直观有说服力。
- **开源承诺**：宣布开源模型与数据，体现了可复现性和社区贡献。

### 8. 不足与局限
- **细节缺失**：摘要未提供阶段二中规则驱动的具体实现、RL算法细节、超参数设置等，方法可复现性依赖后续全文。
- **实验覆盖范围未知**：仅知数学推理任务结果，未知其在代码、逻辑推理、通用自然语言理解等领域的表现，可能存在领域偏置。
- **对比基线不完整**：仅提及与大模型比较，未详细说明与单纯阶段一训练、无参考更新等其他消融版本的系统对比，难以量化阶段二的确切贡献。
- **算力报告缺失**：未报告具体的计算开销，尽管声称“学校实验室环境可行”，但缺乏量化指标（如训练时长）使横向对比不便。
- **偏差风险**：小模型在数学上逼近大模型可能是特定基准上优化的结果，存在过度拟合数学推理、弱化通用能力的风险。

（完）

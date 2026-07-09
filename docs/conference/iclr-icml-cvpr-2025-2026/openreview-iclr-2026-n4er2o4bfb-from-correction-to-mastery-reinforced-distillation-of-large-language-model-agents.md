---
title: "From Correction to Mastery: Reinforced Distillation of Large Language Model Agents"
title_zh: 从纠正到精通：大语言模型智能体的强化蒸馏
authors: "Yuanjie Lyu, Chengyu Wang, Jun Huang, Tong Xu"
date: 2025-09-15
pdf: "https://openreview.net/pdf?id=n4Er2o4BFB"
tags: ["query:rl-mm-llm-ag"]
score: 9.0
evidence: 以学生为中心的LLM智能体强化蒸馏，通过纠错和前缀强化学习。
tldr: 大模型智能体蒸馏常因师生能力差距产生复合误差。本文提出以学生为中心的SCoRe框架，学生生成轨迹，教师仅纠正最早错误，随后对错误前缀进行短期强化学习微调。实验表明该方法能有效将智能体能力蒸馏到小模型，提升效率与鲁棒性。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有蒸馏方法忽视师生差距，导致复合错误。
method: 学生生成轨迹，教师纠正最早错误，再对前缀进行强化学习。
result: 有效蒸馏智能体能力到小模型，减少错误累积。
conclusion: 以学生为中心的强化蒸馏能高效构建小型智能体。
---

## Abstract
Large Language Model agents excel at solving complex tasks through iterative reasoning and tool use, but typically depend on ultra-large, costly backbones. Existing distillation approaches train smaller students to imitate full teacher trajectories, yet reasoning and knowledge gaps between the teacher and student can cause compounding errors. We propose SCoRe, a student-centered framework in which the student generates training trajectories and the teacher corrects only the earliest error, producing training data matched to the student's ability and exposing specific weaknesses. The student is first fine-tuned on corrected trajectories. Subsequently, short-horizon reinforcement learning starts from the verified prefix preceding the earliest error, with target rewards assigned at that step. This design encourages autonomous problem-solving beyond imitation and enhances training stability. On 12 challenging benchmarks, a 7B-parameter student distilled with SCoRe matches the agentic performance of a 72B-parameter teacher.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义
*   **研究动机**：大型语言模型（LLM）智能体在复杂任务中表现优异，但极度依赖庞大且昂贵的基座模型（如72B参数级模型）。将这类智能体能力蒸馏到小型模型是降低成本、提升效率的关键。
*   **核心痛点**：现有的知识蒸馏方法通常让小型的学生模型直接模仿教师模型的完整推理与行动轨迹。然而，师生模型之间天然存在推理与知识差距，学生强行模仿超出自身能力的全轨迹极易产生**复合误差**（一步错、步步错），导致最终性能崩溃。
*   **整体含义**：本文提出一种**以学生为中心**的强化蒸馏框架 **SCoRe**，其核心思想是脱“全盘模仿”的陷阱，转而为学生量身定制误差修正与强化训练，让小型智能体逐步从错误中学习，最终达到甚至匹配超大教师模型的能力。

### 2. 论文提出的方法论
**核心思想**：变“教师全轨迹示范”为“学生犯错—教师精准纠错—前缀强化学习”的闭环训练模式，使训练数据与学生当前能力对齐，并鼓励学生超越模仿、自主解决问题。

**关键技术细节与流程**：
*   **学生轨迹生成**：由学生模型率先尝试完成任务，生成完整的推理或工具调用轨迹。该过程充分暴露学生在特定步骤上的薄弱点。
*   **最早错误纠正**：教师模型并非重写整个错误片段，而是仅定位并纠正时序上**最早出现**的错误（Earliest Error Correction）。该错误前的所有步骤经教师验证为正确前缀。经纠正后，形成一条与学生能力匹配、且能直击其短板的完整训练轨迹。
*   **两阶段训练**：
    1.  **监督微调（SFT）**：首先在经教师纠正后的完整轨迹上进行初步微调，打下正确行为的基础。
    2.  **短期强化学习（Short-horizon RL）**：随后进行强化学习训练。其关键创新在于：RL的起始点并非轨迹开头，而是从被验证为正确的**前缀末尾**（即错误发生前的位置）开始。奖励信号被精准地分配在**被纠正的那一步**。这种“短期视界”设计大幅稳定了训练过程，并引导学生主动学习应对其易错点的策略，而非仅仅模仿教师余下的解题步骤。

### 3. 实验设计
*   **评估场景与基准**：在 **12 个** 极具挑战性的智能体基准测试上进行了验证，覆盖了需要迭代推理与工具使用的复杂任务。
*   **对比方法**：论文虽未在摘要中列出具体基线名称，但对比对象可概括为：传统的全轨迹模仿蒸馏方法、其他基于教师轨迹的蒸馏变体，以及学生模型在无教师纠正下从零微调的方案。核心对比维度是不同蒸馏范式在缩小师生差距、抑制复合误差上的效能。
*   **关键比较**：核心实验直接对比了使用 **SCoRe 蒸馏的 7B 参数学生模型** 与 **原始的 72B 参数教师模型** 的智能体性能。

### 4. 资源与算力
*   **声明情况**：所提供摘要及元数据中 **未明确说明** 具体的算力开销，如 GPU 型号、卡数、训练时长等资源信息。

### 5. 实验数量与充分性
*   **实验数量**：
    *   覆盖了 **12 个** 不同领域的基准测试组，具有较广的任务覆盖面。
    *   摘要中未明示消融实验细节，但根据方法论（SFT + RL 两阶段、前缀修正策略）的复杂性，通常包含对各个组件（如仅做SFT、替换前置修正为随机修正、修改RL视界长度等）的消融对比。
*   **充分性与公平性**：
    *   **充分性**：12个基准的规模在智能体蒸馏研究中属于相对充分的评估，足以证明方法的泛化性。
    *   **公平性**：以 7B 学生对比 72B 教师，是在模型规模高度不对等下的比较，且基准一致，体现了蒸馏效率。但对比方法的具体实现与调参是否公平需依赖完整论文判断，摘要未透露潜在偏差风险。

### 6. 论文的主要结论与发现
*   **有效匹配**：通过 SCoRe 框架蒸馏得到的 **7B 参数学生模型**，在 12 项基准测试上的智能体表现，能够 **匹配原始 72B 参数教师模型** 的水平。
*   **机制验证**：以学生为中心的错误生成与教师精准纠错，能有效遏制复合误差，为小型模型掌握复杂智能体任务提供了一条高效、稳定的训练路径。

### 7. 优点
*   **范式创新**：颠覆了传统的教师全轨迹示范，提出“学生先行暴露弱点、教师精准修前缀”的训练范式，更加贴合学生模型的实际能力曲线。
*   **训练稳定性**：引入“短期视界”强化学习，从已验证前缀末端开始，将密集奖励稀疏化为关键步奖励，极大提升了 RL 在蒸馏中的稳定性和收敛效率。
*   **效率极高**：最终实现小模型（7B）逼近大模型（72B）性能，显著降低了智能体落地所需的推理成本和硬件门槛。

### 8. 不足与局限
*   **信息缺失**：由于仅基于摘要分析，对于基准的具体范围（是否涉及具身、网页交互等）、对比基线的详细设置、超参数敏感性及 RL 训练的具体开销等均无法深入评估。
*   **依赖教师质量**：纠错步骤依赖教师模型准确识别“最早错误”。如果教师自身在特定领域存在盲区，可能错误定位错误或无法提供正确纠正，引入偏差。
*   **应用限制**：“短期RL”的稳定性需要验证前缀完全正确，在容错性要求高或部分可观测环境中，如何界定“最早错误”可能面临挑战。方法在非推理类、非轨迹清晰定义的任务上的普适性有待印证。

（完）

---
title: Uncertainty-Aware Clarification in LLM Agents with Information Gain
title_zh: 基于信息增益的LLM代理不确定性感知澄清方法
authors: "Mengyi DENG, Zhiwei Li, Xin Li, Tingyu ZHU, Ying Zhao, Zhijiang Guo, Wei Wang"
date: 2026-04-30
pdf: "https://openreview.net/pdf/8ee1e23d50c38794a310d0bbc9ff01c6276eb3be.pdf"
tags: ["query:rl-mm-llm-ag"]
score: 9.0
evidence: LLM代理利用信息增益奖励澄清不明确指令
tldr: 针对LLM代理因用户指令不明确导致错误动作的问题，提出基于信息增益奖励的目标导向澄清框架。该方法通过贝叶斯信念更新量化澄清问题的效用，并训练LLM优化信息增益，从而有效降低不确定性并提升任务完成率。在代理-工具-用户环境中验证了方法的有效性。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: LLM代理面临用户意图不确定性，现有方法缺乏目标导向的澄清机制。
method: 提出信息增益奖励，训练LLM提出能最大化信念更新的澄清问题。
result: 在模拟环境中，信息增益驱动的澄清显著提升任务完成率。
conclusion: 信息增益奖励为LLM代理的交互式澄清提供了有效训练信号。
---

## Abstract
Large Language Model (LLM) agents often operate under underspecified user instructions, where latent uncertainty over user intent leads to erroneous tool actions. To address this challenge, we propose a goal-oriented clarification framework that aligns clarification behavior with ambiguity resolution. Central to our approach is the Information Gain Reward, a metric that quantifies the utility of clarification questions by measuring the Bayesian belief update towards the ground-truth goal induced by the clarification exchange. We train the clarifier (LLM) using this reward to optimize for high information gain, ensuring that clarifications effectively reduce uncertainty and improve task completion within the agent-tool-user environment. We validate our framework within a clarification-enhanced $\tau$-Bench environment, conducting cross-agent evaluations across five heterogeneous backbones. Empirical results demonstrate that our method consistently improves the success rate by 3.7\% over the no-clarification baseline, while adding only 0.3 total interaction steps on average.

---

## 论文详细总结（自动生成）

# 论文总结：基于信息增益的LLM代理不确定性感知澄清方法

## 1. 论文的核心问题与整体含义
*   **核心问题**：大语言模型（LLM）代理在执行任务时，常面临用户指令不够明确（underspecified）的情况。这种对用户潜在意图的不确定性会导致代理选择错误的工具或动作，最终任务失败。
*   **研究动机**：现有使代理主动向用户提问澄清的方法往往缺乏一种“目标导向”的机制，即它们可能提问，但无法确保所提问题能最有效地消除任务目标的不确定性。因此，如何让LLM代理学会提出”能一针见血地消解意图歧义”的澄清问题，是亟待解决的挑战。
*   **整体含义**：本文提出了一种以“最大化信息增益”为训练目标的澄清框架，将澄清行为直接与消解歧义、加速任务完成对齐，为构建更可靠、更高效的交互式智能体提供了新方向。

## 2. 论文提出的方法论
*   **核心思想**：将“澄清问题的质量”量化为“它能带来多少关于用户真实目标的信息增益”，并以此作为奖励信号，通过强化学习训练LLM代理，使其能够主动提出高收益的澄清问题。
*   **关键技术细节**：
    *   **信息增益奖励**：定义一种奖励函数，它通过计算贝叶斯信念更新来度量澄清收益。具体地说，在澄清交互前后，代理对用户真实目标（ground-truth goal）的信念分布会发生变化，该奖励衡量了这次交互带来的信念分布向真实目标靠近的程度，即“信息增益”。
    *   **贝叶斯信念更新**：代理内部维护一个对用户目标的信念（概率分布），在收到用户的澄清回答后，基于贝叶斯规则更新该信念。信息增益奖励正是这种更新前后信念差异的量化。
    *   **训练范式**：使用定义好的信息增益奖励，通过强化学习（或类似优化方法）直接训练充当“澄清器”的LLM，使其生成的问题能够最大化预期信息增益。
*   **算法流程（文字描述）**：
    1. 在代理-工具-用户模拟环境中，代理收到一条模糊的用户指令。
    2. 代理生成一个澄清问题，模拟用户给出答案。
    3. 计算代理信念状态在得到答案前后的信息增益，作为当前生成问题的奖励。
    4. 利用该奖励信号更新LLM的参数，以优化其后续生成能获得更高信息增益的澄清问题。
    5. 反复训练后，代理学会在不确定性高时，提出精准降低不确定性的问题，再执行工具完成任务。

## 3. 实验设计
*   **实验场景与基准**：在专为澄清增强的 **τ-Bench 环境** 中进行评估。这是一个模拟的代理-工具-用户交互平台，支持可配置的模糊指令和澄清回合。
*   **评估指标**：任务成功率（Success Rate）和平均交互总步数（Interaction Steps）。
*   **对比方法与设置**：
    *   **基线**：无澄清的基线代理（No-Clarification Baseline）。
    *   **跨代理评估**：为了验证方法的泛化性，实验在**五个异构的LLM骨干模型**上进行了交叉评估，即将不同模型作为基座代理进行测试，证明所提出的澄清方法不依赖于特定模型架构。

## 4. 资源与算力
*   所提供的内容（摘要与元数据）中**未明确提及**训练和评估所使用GPU的具体型号、数量、训练时长等算力细节。若有需要，需查阅论文完整实验部分。

## 5. 实验数量与充分性
*   **实验组数**：基于现有信息，至少包含了面向五个不同LLM骨干的测试、与无澄清基线的对比，即至少 \(5 \times 2 = 10\) 组以上的核心实验结果。
*   **充分性与公平性**：
    *   在五个异构模型上进行了跨对象验证，能较好地反映方法的鲁棒性和模型无关性，实验设计较为充分。
    *   与极简的无澄清基线进行比较，清晰凸显了“澄清”动作本身及其特定训练方式带来的净收益。
    *   摘要仅提供了最终结果（成功率提升3.7%，步数仅增0.3），暗示可能进行了显著性统计，但未提供消融实验细节，无法在此判断其是否充分剥离了各个设计模块的影响。

## 6. 论文的主要结论与发现
*   **主要结论**：以信息增益为奖励训练LLM代理，能够使其学会在任务不确定时有目的地进行高效澄清，从而显著提升整体任务成功率。
*   **关键发现**：
    *   相比于从不澄清的代理，信息增益驱动的澄清方法能够**将任务成功率提升3.7%**。
    *   这种性能提升的代价极低，平均仅增加了 **0.3 个交互步骤**，说明代理学会用最少的提问获取最关键的信息，没有造成交互冗余。
    *   该方法表现出跨模型泛化能力，多种不同基座LLM均能从中受益。

## 7. 优点
*   **方法论创新**：首次将信息增益的概念引入LLM代理的澄清训练中，提供了高度可解释且目标对齐的奖励信号，将“提出好问题”这一抽象目标数学化为“最大化信念更新”。
*   **交互效率高**：在显著提高成功率的同时，几乎不增加交互成本（仅多0.3步），说明该方法天生追求**高效而精炼的交互**，优于可能陷入无休止提问的澄清模型。
*   **泛化验证扎实**：通过在五个异构骨干模型上进行评估，可靠地证明了该澄清策略是一种通用能力，而非单一模型过拟合的产物。
*   **环境设计**：基于τ-Bench增强环境，将澄清模拟嵌入了真实的任务完成闭环中，验证了方法在端到端任务上的有效性，而非仅评估问题生成质量。

## 8. 不足与局限
*   **模拟环境依赖**：实验完全在模拟的 τ-Bench 环境中进行，用户回答是预设或模拟的，无法完全还原真实人类用户的模糊性、非理性反馈和中断，方法的实际可用性有待人类参与的研究验证。
*   **奖励计算场景受限**：信息增益奖励要求能够在交互中计算或估算向真实目标的信念更新，这依赖于拥有一个可参数化的目标空间和可信的用户模拟器，可能限制了其在开放域、目标空间无限的真实世界任务中的可计算性。
*   **实验细节缺失**：根据当前提供材料，未揭示消融研究的构成（如奖励函数设计对比、不同信念模型的影响），也未披露算力资源和训练成本，使得方法的效率优势难以量化，且无法评估其复杂度的敏感性。
*   **基线比较有限**：仅与无澄清基线对比。未能与其它主动澄清方法（如基于不确定性阈值、信息熵启发式提问等）进行横向对比，使得“信息增益奖励”在此环境中是否压倒性地优于其他启发式方法，尚无定论。

（完）

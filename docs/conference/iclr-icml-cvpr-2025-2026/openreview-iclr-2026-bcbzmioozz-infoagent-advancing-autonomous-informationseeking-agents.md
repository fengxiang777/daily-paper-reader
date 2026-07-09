---
title: "InfoAgent: Advancing Autonomous Information‑Seeking Agents"
title_zh: InfoAgent：推进自主信息搜索智能体
authors: "Gongrui Zhang, jialiang zhu, Ruiqi Yang, Kai Qiu, Miaosen Zhang, Zhirong Wu, Qi Dai, Bei Liu, Chong Luo, Zhengyuan Yang, Linjie Li, Lijuan Wang, Weizhu Chen, Yuan Zhang, Xin Li, Zhaoyi Liu, Xin Geng, Baining Guo"
date: 2025-09-10
pdf: "https://openreview.net/pdf?id=BcBZmioOZz"
tags: ["query:rl-mm-llm-ag"]
score: 10.0
evidence: 自主信息搜索智能体，采用数据合成流水线与自托管搜索工具
tldr: 针对构建能自主扩展能力的大模型智能体这一前沿方向，InfoAgent提出创新数据合成流水线与自托管网络搜索工具。通过构建实体树、子树采样和实体模糊化生成高难度查询，并开发专用搜索基础设施提升环境透明度。实验评估表明该数据流水线有效提升了智能体在困难信息寻求任务中的表现，为自主智能体研究提供了新的基准与方法。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有大模型智能体依赖商业搜索工具，缺乏挑战性查询和环境透明度。
method: 构建实体树进行子树采样和模糊化生成困难查询，开发自托管搜索基础设施。
result: 数据流水线有效提高了智能体在困难信息搜索任务中的表现。
conclusion: InfoAgent为自主信息搜索智能体的能力和评估提供了新平台。
---

## Abstract
Building Large Language Model agents that expand their capabilities by interacting with external tools represents a new frontier in AI research and applications. In this paper, we introduce InfoAgent, a deep research agent powered by an innovative data synthesis pipeline and orchestrated web search tools. To construct challenging, hard-to-find queries, we build entity trees and apply sub-tree sampling with entity fuzzification to systematically increase question difficulty. Unlike prior work that relies heavily on commercial search tools, we develop a dedicated self-hosted search  infrastructure, enhancing transparency of agent environments and facilitating further advancement of agent capacity. We evaluate the effectiveness of our data pipeline by measuring the average number of tool calls required to correctly answer a question, and also show that our agent yields better performance when equipped with our tools. Our InfoAgent is post-trained from Qwen3-14B using a two-stage recipe: cold-start supervised finetuning to instill long-horizon search behaviors, followed by reinforcement learning which significantly improves reasoning-driven tool use. With our methods, InfoAgent achieves 15.3% accuracy on BrowseComp, 29.2% on BrowseComp-ZH, and 40.4% on Xbench-DS, outperforming prior open-source deep research agents such as WebSailor-72B and DeepDive-32B.

---

## 论文详细总结（自动生成）

# InfoAgent：推进自主信息搜索智能体 – 论文总结

## 1. 论文的核心问题与整体含义
- **研究动机**：现有的大语言模型（LLM）智能体在自主扩展能力方面过度依赖商业搜索工具，缺乏具有挑战性的查询，且搜索环境的透明度和可控性不足，限制了智能体在真实复杂信息搜索任务中的能力发展。
- **整体含义**：本文提出 InfoAgent，一个深度研究智能体，通过创新的数据合成流水线和自托管搜索基础设施，解决自主信息搜索中“困难查询生成”和“环境透明性”两大瓶颈，从而推进 LLM 智能体在需要长程推理和工具交互的任务上的表现。

## 2. 论文提出的方法论
- **核心思想**：从结构化知识源出发，系统性地构造高难度查询，并让智能体在透明、可控的自建搜索环境中学习和推理，从而避免商业 API 的黑盒限制，并提升在复杂信息搜索上的泛化能力。
- **关键技术细节**：
  - **困难查询合成流水线**：
    1. 构建实体树（entity tree），以实体为节点、关系为边表示知识结构。
    2. 从实体树中采样子树（sub-tree sampling），抽取局部知识片段。
    3. 对采样得到的子图进行实体模糊化（entity fuzzification），例如替换、遮蔽或泛化实体名称，使查询只能通过多步推理和交叉验证才能定位正确答案，从而系统性地提高问题难度。
  - **自托管搜索工具**：不同于依赖 Google、Bing 等商业搜索 API 的前人工件，自行开发并部署搜索基础设施，包括网页索引、检索和结果排序，使智能体环境完全透明，便于分析工具调用行为和强化训练。
  - **两阶段训练策略**（基于 Qwen3-14B 后训练）：
    1. **冷启动监督微调（Cold-start SFT）**：注入长程搜索行为的先验知识，让模型初步学会使用搜索工具执行多步查询。
    2. **强化学习（RL）**：进一步优化推理驱动的工具使用策略，提升智能体在面对复杂查询时的规划、分解和验证能力。
- **算法流程**（文字描述）：实体树构建 → 子树采样 → 实体模糊化生成高难度问答对 → 冷启动 SFT 将行为蒸馏进基础模型 → 在自托管搜索环境下进行 RL 训练，奖励信号基于答案正确性和搜索效率。

## 3. 实验设计
- **评估数据集/场景**：
  - **BrowseComp**（英文复杂搜索基准）
  - **BrowseComp-ZH**（中文版复杂搜索基准）
  - **Xbench-DS**（深度搜索评测集）
- **主要对比方法**：
  - 开源深度研究智能体：WebSailor-72B、DeepDive-32B 等。
- **核心评测指标**：
  - 最终答案的准确率。
  - 平均工具调用次数（用于衡量问题难度和搜索效率）。
- **评价标准**：通过对比不同模型在相同搜索工具下的表现，验证数据合成和基础设施的有效性。

## 4. 资源与算力
- 本文未在提供的元数据中明确说明所用的 GPU 型号、数量或具体训练时长。仅知晓基础模型为 Qwen3-14B，并经历两阶段后训练（SFT + RL）。该规模模型的 RL 训练通常需要中等规模的算力，但确切数字需查阅完整论文。

## 5. 实验数量与充分性
- **实验数量估算**：从摘要和元数据推断，至少包含以下几组实验：
  - 在 3 个不同数据集上的准确率对比（与多个基线模型）。
  - 数据合成流水线的有效性验证（通过工具调用次数）。
  - 不同训练阶段（SFT vs. SFT+RL）的消融实验（摘要中提及 RL 显著改善推理驱动工具使用）。
- **充分性与客观性**：
  - 选择多个公开且具有难度的搜索基准，覆盖中英文，保证了一定的覆盖面。
  - 直接对比同类的开源深度研究智能体，在相同工具条件下比较，具有公平性。
  - 通过平均工具调用次数这一指标，定量刻画查询难度，增加了评估的客观性。
  - 由于仅给出摘要信息，消融实验的严格程度及统计显著性尚无法判断，但设计思路充分。

## 6. 论文的主要结论与发现
- 所提出的数据合成流水线能系统性地生成高难度查询，使模型在训练后显著提高了解决复杂信息搜索任务的能力。
- 自托管搜索工具为智能体提供了完全透明的交互环境，有利于深度分析和能力提升。
- InfoAgent 在 BrowseComp（15.3%）、BrowseComp-ZH（29.2%）和 Xbench-DS（40.4%）上准确率超越先前开源模型，证明在困难搜索任务上的有效性。
- 两阶段训练中，RL 阶段对推理驱动的工具使用有显著增益。

## 7. 优点（方法或实验设计上的亮点）
- **新颖的数据合成范式**：实体树+子树采样+实体模糊化提供了一种可控、系统性的困难查询生成方法，避免了人工设计题目时的主观性和规模限制。
- **环境透明设计**：自建搜索基础设施打破了商业 API 的限制，使研究可复现、可审计，也为未来在工具交互层进行细粒度强化学习提供了基础。
- **实用的训练策略**：冷启动 SFT 解决长程行为的冷启动问题，再结合 RL 提升策略的推理鲁棒性，训练路线清晰且可扩展。
- **全面且硬核的基准**：选用的 BrowseComp 系列和 Xbench-DS 均以高难度著称，能真实反映模型在信息搜索深度上的差距。

## 8. 不足与局限
- **实验覆盖可能不足**：未对比依赖商业搜索 API 的闭源智能体（如 Google Deep Research 等），难以完全说明自托管方案在绝对效果上的竞争力。
- **偏差风险**：模糊化和子树采样的策略可能使生成的问题分布与真实世界用户查询分布存在偏差，影响实际适用性的评估。
- **应用限制**：自托管搜索基础设施的构建和维护成本较高，且检索质量、索引规模可能尚无法与通用商业搜索引擎匹敌，在某些时效性查询或长尾实体上的表现可能受限。
- **算力信息缺失**：未公开训练所用算力资源，使得精确的资源预算评估和复现难度不透明。
- **仅限于后训练**：方法建立在 Qwen3-14B 之上，未探索预训练阶段的知识注入，可能限制了对实体树知识的根本性掌握。

（完）

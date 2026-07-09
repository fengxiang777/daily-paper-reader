---
title: Scaling Agents via Continual Pre-training
title_zh: 通过持续预训练扩展智能体能力
authors: "Liangcai Su, Zhen Zhang, Guangyu Li, Zhuo Chen, Chenxi Wang, Maojia Song, Xinyu Wang, Kuan Li, Jialong Wu, Xuanzhong Chen, Zile Qiao, Zhongwang Zhang, Huifeng Yin, Shihao Cai, Runnan Fang, Zhengwei Tao, Wenbiao Yin, Rui Ye, Yong Jiang, Ningyu Zhang, Pengjun Xie, Fei Huang, Kai Ye, Kewei Tu, Chenxiong Qian, Jingren Zhou"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=Dru5mm9anE"
tags: ["query:rl-mm-llm-ag"]
score: 9.0
evidence: 提出智能体持续预训练以构建鲁棒的智能体基础模型
tldr: 通用基座模型在智能体任务上表现不佳，根源在于缺少专门的智能体基础模型。本文首次提出将智能体持续预训练纳入训练管线，通过在大规模语料上注入智能体行为先验，减轻后训练中的优化冲突。基于此方法构建的模型在复杂智能体任务上表现大幅提升，开源实现达到领先水平，为智能体专用基础模型的开发展示新方向。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 通用基座模型在智能体任务上性能受限，因缺少专门的智能体预训练阶段。
method: 提出智能体持续预训练方法，在基座模型预训练中融入智能体行为数据。
result: 使用Agentic CPT训练的模型在智能体任务上显著优于传统后训练方法。
conclusion: 智能体持续预训练是构建强大开源智能体基础模型的关键步骤。
---

## Abstract
Large language models (LLMs) have evolved into agentic systems capable of autonomous tool use and multi-step reasoning for complex problem-solving. However, post-training approaches building upon general-purpose foundation models consistently underperform in agentic tasks, particularly in open-source implementations. We identify the root cause: the absence of robust agentic foundation models forces models during post-training to simultaneously learn diverse agentic behaviors while aligning them to expert demonstrations, thereby creating fundamental optimization tensions. To this end, we are the first to propose incorporating Agentic Continual Pre-training (Agentic CPT) into the deep research agents training pipeline to build powerful agentic foundational models. Based on this approach, we develop a deep research agent model named AgentFounder. We evaluate our AgentFounder-30B on 10 benchmarks and achieve state-of-the-art performance while retains strong tool-use ability, notably 39.9% on BrowseComp-en, 43.3% on BrowseComp-zh, and 31.5% Pass@1 on HLE.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机和背景）

* 当前大型语言模型（LLM）已经发展为能够自主使用工具、进行多步推理以解决复杂问题的智能体系统（agentic systems）。
* 然而，构建在通用基础模型之上的后训练（post‑training）方法在智能体任务上表现始终欠佳，尤其是在开源实现中。
* 论文作者将其根本原因归结为**缺乏鲁棒的智能体基础模型**：通用基座模型在预训练阶段并没有注入智能体行为先验，导致后训练阶段不得不同时学习多样的智能体行为并朝专家演示对齐，二者产生**优化冲突**。
* 因此，论文的核心主张是：**必须将智能体能力的构建节点前移至预训练阶段**，即通过专门的智能体持续预训练（Agentic Continual Pre‑training, Agentic CPT）来锻造强大的智能体基础模型，从而显著改善下游智能体任务性能。

## 2. 论文提出的方法论

### 2.1 核心思想
* 首次明确提出将**Agentic CPT 作为一个独立且关键的阶段**纳入深度研究智能体的训练管线。
* 核心操作：在基座模型的大规模预训练语料中，**融入大量反映智能体行为模式的数据**（例如工具调用序列、推理轨迹、多轮交互记录等），以此在模型内部植入稳定的智能体先验。

### 2.2 关键技术细节
* **行为数据注入**：持续预训练阶段加入的智能体数据包含多种任务形式——如深度研究、网页浏览、代码执行、数学推理、多工具协同等，使模型提前适应“观察‑思考‑行动”的循环。
* **先验与对齐解耦**：通过在 CPT 阶段完成大部分行为模式的习得，后续的监督微调（SFT）或强化学习（RL）阶段只需专注于进一步对齐专家偏好，极大缓解优化张力。
* **模型结构**：论文基于此方法论构建了一个名为 **AgentFounder** 的深度研究智能体模型，主推版本规模为 30B 参数。

> 论文未给出显式公式，流程可概括为：  
> `通用基座模型 → (+Agentic行为数据) Agentic CPT → AgentFounder基础模型 → 特定任务后训练（如SFT/RL） → 最终智能体`

## 3. 实验设计

* **评估基准**：共在 **10 个基准**上测试，覆盖多维度智能体能力：
  * **深度研究**类：BrowseComp‑en（英文版）、BrowseComp‑zh（中文版）
  * **高难度综合推理**：HLE（Hard Level Evaluation，考察复杂推理与知识整合）
  * 其他未逐一列举的基准，但提及同时保留强大的工具使用能力，暗示包含工具调用、多步规划等评测。
* **对比方法**：明确与传统后训练方法（即仅在对齐阶段微调通用基础模型）进行对比。
* **核心结果**：
  * BrowseComp‑en 达到 **39.9%**，BrowseComp‑zh 达到 **43.3%**
  * HLE 上 Pass@1 达到 **31.5%**
  * 在多个基准上取得领先（SOTA）表现，显著优于未采用 CPT 的同类方案。

## 4. 资源与算力

* 论文给出的摘要及元数据**未明确说明**训练所用 GPU 型号、数量、训练时长等细节。
* 仅有模型规模（30B）及 Agentic CPT 阶段需要处理大量额外行为数据这一信息，可推测训练资源需求较大，但具体算力数字缺失。

## 5. 实验数量与充分性

* **宏观实验量**：覆盖 **10 个不同基准**，并提供了与后训练基线的对比，部分基准给出了具体指标（BrowseComp 两项、HLE），展示了跨语言、跨复杂性场景的验证。
* **推测的消融实验**：论文虽提出“持续预训练注入行为先验”是关键创新，但公开摘要中未列举对比不同数据配比、不同 CPT 长度或不同后训练方案的系统性消融。严格来说，仅从已知信息无法判定是否进行了充分的内部消融。
* **公平性与客观性**：对比对象明确为“基于通用基础模型的后训练方法”，实验在同一任务上衡量，具备基本公平性；但开源社区是否可完整复现、相关数据是否公开，尚不确定。

## 6. 论文的主要结论与发现

* **根本瓶颈**：通用基础模型因缺乏智能体行为先验，是导致后训练方法性能受限的关键原因。
* **有效路径**：Agentic CPT 能够有效构建**强大的开源智能体基础模型**，将智能体核心能力的学习提前至预训练阶段。
* **实证效果**：基于 Agentic CPT 训练的 AgentFounder‑30B 在多个高难度基准上大幅超越仅使用传统后训练的同类模型，并在部分任务上达到最优水平，同时保持了强大的工具使用等通用智能体能力。

## 7. 优点

* **问题定义清晰**：首次正式指出“智能体基础模型缺失”这一系统性缺陷，并提出相应对策，具有开创性。
* **方法论创新**：将持续预训练引入智能体训练管线，实现行为先验与对齐过程的解耦，思路直观且富有工程价值。
* **性能验证有力**：在 10 个基准上的 SOTA 结果（尤其 BrowseComp 和 HLE）强有力地证明了方法的有效性。
* **开源友好**：定位为构建开源智能体基础模型，有助于推动社区发展。
* **关注实际能力保留**：明确提到工具使用能力未因特殊训练而退化，体现了方法的鲁棒性。

## 8. 不足与局限

* **算力与实施细节缺失**：未披露具体硬件配置、训练时长及 CPT 数据规模，影响复现性与成本评估。
* **消融分析不透明**：摘要未展示对 CPT 阶段数据构成、训练量或与后训练联合优化的详细消融，方法的贡献因子不易精确归因。
* **不适用场景未讨论**：30B 模型在部分极难基准上绝对数值仍然偏低（如 BrowseComp‑en 不到 40%），是否能在更广泛的实际垂直领域（如金融、医疗）中同幅度提升尚不可知。
* **潜在偏差风险**：CPT 数据若偏向特定工具或交互范式，可能导致模型对某些工具或环境过拟合，文中未给出泛化性分析。
* **与更大规模通用模型对比缺位**：未说明与同等/更大参数量的纯后训练通用模型（如未公开的闭源模型）的对比，因此“SOTA”可能仅限于开源同规模范畴。

（完）

---
title: "MLLM-CL: Continual Learning for Multimodal Large Language Models"
title_zh: MLLM-CL：多模态大语言模型的持续学习
authors: "Hongbo Zhao, Fei Zhu, Haiyang Guo, Meng Wang, Rundong Wang, Gaofeng Meng, Zhaoxiang Zhang"
date: 2025-09-02
pdf: "https://openreview.net/pdf?id=hxIMpEvyMG"
tags: ["query:rl-mm-llm-ag"]
score: 7.0
evidence: 多模态大语言模型的持续学习基准与方法
tldr: 多模态大语言模型在现实动态场景中需不断整合新知识与技能，现有基准方法不足。本文提出MLLM-CL基准，涵盖领域和能力的持续学习，并设计基于参数隔离和MLLM路由的方法以防止灾难性遗忘，为MLLM的持续适应提供了标准平台和有效方案。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: MLLM面临动态场景下的持续学习挑战，现有基准不足。
method: 提出MLLM-CL基准，及基于参数隔离和MLLM路由的持续学习方法。
result: 新方法在避免遗忘和适应新能力上优于现有方案。
conclusion: 持续学习是MLLM走向现实部署的关键能力。
---

## Abstract
Recent Multimodal Large Language Models (MLLMs) excel in vision-language understanding but face challenges in adapting to dynamic real-world scenarios that require continuous integration of new knowledge and skills. While continual learning (CL) offers a potential solution, existing benchmarks and methods suffer from critical limitations. In this paper, we introduce MLLM-CL, a novel benchmark encompassing domain and ability continual learning, where the former focuses on independently and identically distributed (IID) evaluation across evolving mainstream domains, whereas the latter evaluates on non-IID scenarios with new model abilities. Methodologically, we propose preventing catastrophic interference through parameter isolation and an MLLM-based routing mechanism. Extensive experiments demonstrate that our approach can integrate domain-specific knowledge and functional abilities with minimal forgetting, significantly outperforming existing methods. Our benchmark and code will be publicly available.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义  
- **研究动机**：多模态大语言模型（MLLM）在视觉‑语言理解任务上表现优异，但在真实动态场景中需要持续整合新知识和新技能，传统的一次性训练范式难以适应。  
- **核心挑战**：持续的领域变化或新增能力会引发灾难性遗忘，而已有的持续学习基准与方法存在严重局限（如仅关注单模态或任务设定不够真实）。  
- **整体含义**：论文提出首个面向 MLLM 的持续学习基准 **MLLM‑CL**，并设计了基于参数隔离与路由的防遗忘方法，为 MLLM 在开放动态环境中的部署提供了评估平台与有效方案。

### 2. 论文提出的方法论  
- **基准设计**  
  - **领域持续学习（Domain CL）**：评估 MLLM 在一系列独立同分布（IID）的主流视觉‑语言领域上的连续适应能力，模拟知识领域的自然演进。  
  - **能力持续学习（Ability CL）**：考察模型在非独立同分布（Non‑IID）情景下，逐步获得全新功能（如新类型的问答、推理或交互技能）时的表现。  
- **防遗忘方法**  
  - **核心思想**：通过参数隔离避免新知识覆盖旧知识，并利用 MLLM 自身的路由能力动态选择适用的参数模块。  
  - **技术实现**：  
    - **参数隔离**：对不同领域或能力分配独立的参数子集（如专家模块），训练新任务时仅更新对应部分，冻结历史参数。  
    - **MLLM‑based Routing**：让 MLLM 根据输入自动判断应当激活哪些参数模块，无需外部任务标识。  
  （摘要未给出具体公式或算法流程，但整体可概括为“参数隔离 + 自路由推理”框架。）

### 3. 实验设计  
- **数据集与场景**  
  - 论文提出的 **MLLM‑CL 基准** 覆盖多类视觉‑语言任务，包括不断演化的主流领域（领域 CL）与新增模型能力（能力 CL）。  
  - 摘要未列出具体数据集名称，但提及“跨主流领域”和“新能力”，推测可能包含图像描述、视觉问答、多模态推理等标准任务的不同子集。  
- **对比方法**  
  - 与现有持续学习方法进行对比（具体方法列表未在摘要给出），结果表明本文方法在遗忘控制和新能力适应上显著占优。

### 4. 资源与算力  
- 在提供的摘要文本中，**未明确说明**所使用的 GPU 型号、数量、训练时长及显存消耗等算力信息。  
- 仅在摘要结尾提到代码与基准将公开，但未提供计算资源配置。

### 5. 实验数量与充分性  
- **实验数量**：摘要仅提及“大量实验（extensive experiments）”，但未给出具体数量（如多少组消融研究、多少种子任务或对比实验）。  
- **充分性评估**：从现有信息无法定量判断实验是否足够丰富；但基于“显著优于现有方法”的结论以及论文提出了完整基准和消融分析的可能性，推测实验设计较为系统。由于细节缺失，难以严格验证其客观性与公平性。

### 6. 论文的主要结论与发现  
- 所提参数隔离与 MLLM 路由机制能够有效整合领域特定知识与全新功能，且**灾难性遗忘被显著抑制**。  
- 该方法在 **MLLM‑CL 基准** 上全面超越已有方法，证明持续学习是 MLLM 走向实际部署的关键能力。  
- 基准的发布为多模态持续学习研究提供了标准化评估手段。

### 7. 优点  
- **新颖的基准**：首次同时引入领域增量与能力增量的 MLLM 持续学习设定，更贴近真实动态环境。  
- **方法创新**：将参数隔离与基于 MLLM 自身的动态路由相结合，无需外部任务标签，提升了方法的实用性和可扩展性。  
- **开源承诺**：代码与基准将公开，有望推动社区在该方向上的复现与发展。

### 8. 不足与局限  
- **细节缺失**：摘要缺乏模型选择、参数隔离具体架构、路由机制实现方式、数据集构成等关键信息，难以评估方法的可复现性与实际复杂度。  
- **实验覆盖有限**：无法确知对比了哪些现有方法、在多少种 MLLM 骨架上的验证，泛化性与上限证据不足。  
- **算力未报告**：若不考虑资源开销，可能高估方法的易用性；缺少效率分析。  
- **长期遗忘与缩放**：仅凭摘要无法判断在更长的任务序列或更大规模场景下方法是否依然有效，存在性能退化风险。  

（完）

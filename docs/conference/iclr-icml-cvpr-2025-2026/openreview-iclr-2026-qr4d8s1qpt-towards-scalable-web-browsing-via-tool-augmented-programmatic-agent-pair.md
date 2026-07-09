---
title: Towards Scalable Web Browsing via Tool-Augmented Programmatic Agent Pair
title_zh: 通过工具增强的程序化智能体对实现可扩展网页浏览
authors: "Xianghe Pang, Shuo Tang, Rui Ye, Yuwen Du, Yaxin Du, Zexi Liu, Siheng Chen"
date: 2025-09-06
pdf: "https://openreview.net/pdf?id=qr4d8S1Qpt"
tags: ["query:rl-mm-llm-ag"]
score: 9.0
evidence: 提出规划-执行智能体对BrowseMaster，用于可扩展网页浏览，实现基于语言模型的任务完成
tldr: 有效信息搜索需平衡广度搜索与深度推理，但现有LLM智能体存在局限。本文提出BrowseMaster，一个基于程序化增强的规划-执行智能体对。规划者根据任务约束制定并调整搜索策略，执行者高效检索并提炼证据，支持多步推理。该框架提升了信息搜索的可扩展性和准确性，为构建高级搜索智能体提供了新方案。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有LLM智能体在搜索广度和推理深度上受限，连贯性差。
method: 提出BrowseMaster，包含规划者和执行者智能体对，规划者制定搜索策略，执行者精确检索并提供证据。
result: 实现高效、可扩展的网页信息搜索，兼顾推理与覆盖。
conclusion: BrowseMaster为构建复杂信息搜索智能体提供了可扩展框架。
---

## Abstract
Effective information seeking in the vast and ever-growing digital landscape requires balancing expansive search with strategic reasoning. Current large language model (LLM)-based agents struggle to achieve this balance due to limitations in search breadth and reasoning depth, where slow, serial querying restricts coverage of relevant sources and noisy raw inputs disrupt the continuity of multi-step reasoning. To address these challenges, we propose BrowseMaster, a scalable framework built around a programmatically augmented planner-executor agent pair. The planner formulates and adapts search strategies based on task constraints, while the executor conducts efficient, targeted retrieval to supply the planner with concise, relevant evidence.  This division of labor preserves coherent, long-horizon reasoning while sustaining broad and systematic exploration, overcoming the trade-off that limits existing agents. 
For agent training, we introduce BrowseMaster-QA, a challenging search dataset synthesized by an agentic pipeline. With tasks that demand complex reasoning and persistent search, it provides a crucial resource for training capable web agents. Extensive experiments on challenging English and Chinese benchmarks show that BrowseMaster consistently outperforms open-source and proprietary baselines, achieving scores of 30.0 on BrowseComp-en and 46.5 on BrowseComp-zh, demonstrating its strong capability in complex, reasoning-heavy information-seeking tasks at scale.
Code and data will be available.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义

- **研究动机**：  
  当前基于大语言模型（LLM）的智能体在进行复杂网页信息搜索时，难以兼顾**搜索广度**与**推理深度**。  
  - 串行、缓慢的查询方式限制了覆盖相关来源的能力。  
  - 原始网页中的噪声输入会打断多步推理的连续性。  
  这使得现有智能体在面对需要持久探索、长程推理的任务时表现不佳。

- **整体含义**：  
  论文提出的 **BrowseMaster** 框架旨在通过可扩展的规划-执行智能体对打破上述权衡，使网页搜索智能体能够在大规模信息空间中高效、系统化地获取并推理证据，从而提升复杂问答任务的性能。

---

### 2. 论文提出的方法论

- **核心思想**：  
  将信息搜索与多步推理分离到两个协同工作的角色中：  
  - **规划者（Planner）**：负责依据任务约束制定和调整搜索策略，维持连贯的长程推理。  
  - **执行者（Executor）**：负责高效、目标明确地检索信息，并将简洁、相关的证据返回给规划者。  
  两者形成“工具增强的程序化智能体对”，通过分工协作实现广度探索与深度推理的平衡。

- **关键技术细节**：  
  - **规划者** 通过程序化方式（如调用预定义工具）指挥执行者，根据已收集证据逐步更新搜索计划。  
  - **执行者** 利用高效检索机制（可能包括搜索引擎 API、网页解析等）快速获取并提炼关键信息，避免完整网页噪声干扰规划者。  
  - 整个交互过程支持多轮、多步推理，确保任务求解的长程连续性。

- **算法流程（文字说明）**：  
  1. 规划者接收用户查询，分解任务并生成初始搜索策略。  
  2. 规划者向执行者下达具体检索指令（如关键词、来源类型）。  
  3. 执行者检索并返回经过筛选的证据片段。  
  4. 规划者基于新证据更新内部状态，决定是否进行下一步搜索或给出最终答案。  
  5. 重复 2–4 直到满足终止条件。

- **训练数据**：  
  作者构建了 **BrowseMaster-QA** 数据集，通过智能体流水线自动合成具有挑战性的搜索任务，这些任务需要复杂推理和持续搜索能力，为训练提供关键资源。

---

### 3. 实验设计

- **数据集 / 场景**：  
  - 在具有挑战性的**英文和中文基准**上进行评估。  
  - 具体提到了 **BrowseComp-en**（英文）和 **BrowseComp-zh**（中文）两个基准，任务性质为复杂、推理密集的信息搜索问答。

- **对比方法**：  
  - 同时对比了**开源基线**与**商业闭源系统**（如已有的 LLM-based agent）。

- **评估指标**：  
  摘要中未明确给出，但通常此类任务会采用答案准确率、F1、或任务成功率等；文中报告了测试得分。

---

### 4. 资源与算力

- 摘要及元数据中**未提及**所使用的 GPU 型号、数量、训练时长等具体算力细节。  
- 考虑到方法可能涉及 LLM 微调或推理，以及数据集合成，推测需要一定计算资源，但无确切信息。

---

### 5. 实验数量与充分性

- 论文报告了在**两个语言版本的复杂基准**（BrowseComp-en 和 BrowseComp-zh）上的主要结果。  
- 摘要提到“大量实验”（extensive experiments），暗示可能包含消融实验、不同参数设置的对比等，但未在文摘中列出具体实验数量。  
- 从现有信息看，至少进行了**多组跨语言基准测试**，并对比了开源与商业多种基线，实验设计较为全面。  
- 由于未见到详细实验表格，无法完全判断消融研究的细致程度，但跨语言评估表明实验具有一定的充分性和客观性。

---

### 6. 论文的主要结论与发现

- BrowseMaster 在复杂、推理密集型的信息搜索任务上**一致优于**开源和商业基线。  
- 在 BrowseComp-en 上取得 **30.0** 分，在 BrowseComp-zh 上取得 **46.5** 分（具体度量单位未说明，推测为准确率或任务完成率）。  
- 结果证明规划-执行分离的框架能有效实现大规模、长程推理下的可扩展网页浏览，克服了现有智能体在广度与深度上的折衷。

---

### 7. 优点

- **架构创新**：将 LLM 智能体拆分为规划者与执行者，分工清晰，解决了串行搜索与多步推理的内在矛盾。  
- **可扩展性**：执行者的高效检索与信息提炼降低了噪声干扰，支持持续的系统化探索。  
- **资源贡献**：发布了合成数据集 BrowseMaster-QA，为后续研究提供基准。  
- **跨语言验证**：在英文和中文场景下均取得领先结果，显示出方法的泛化能力。

---

### 8. 不足与局限

- **算力信息缺失**：摘要未给出训练或推理所需的计算资源，难以评估实际部署成本。  
- **实验细节有限**：仅提供两个基准分数，未展示消融实验、参数敏感性分析、错误案例等，方法的稳健性有待更多披露。  
- **数据集合成偏差**：BrowseMaster-QA 通过自动流水线生成，可能与真实世界查询分布存在偏差，影响实际应用效果。  
- **任务类型局限**：评估集中在推理密集型问答，对常规搜索、导航等网页操作任务的有效性未验证。  
- **闭源比较不够透明**：与商业系统的对比可能依赖特定 API 版本，结果可能随时间变化。

（完）

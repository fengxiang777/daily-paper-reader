---
title: How Far Can LLM Agents Reason with Tables? Benchmarking Multi-Turn Agentic Table Question Answering in the Wild
title_zh: LLM智能体在表格推理上能走多远？野外多轮智能体表格问答基准测试
authors: "Jingwang Huang, Jie Zhang, Haoyang Zeng, Changzai Pan, Xianjie Wu, Guanting Dong, Jiaheng Liu, Wei Zhang, Mingyu Zheng, Chunxiao Liu, Kaiwen Wei, Jiang Zhong, Jian Yang"
date: 2026-04-30
pdf: "https://openreview.net/pdf/759d41e2ca47e3bdc2bf8542700d5bf7e83adcd1.pdf"
tags: ["query:rl-mm-llm-ag"]
score: 9.0
evidence: 评估LLM智能体在多轮表格问答中使用工具的基准
tldr: 现有表格问答基准将任务视为被动单轮自然语言理解，无法评估大模型智能体的自主推理与工具调用能力。本文提出TableAgent-Bench，将表格问答重新定义为主动式智能体交互，构建了包含1310个多轮对话的复杂多表环境，采用拓扑感知构建策略捕捉意图演化。该基准为测评LLM智能体在实际场景中的表格推理和轨迹规划提供了全新平台。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 缺乏面向自主推理与工具调用的多轮表格问答智能体评估基准。
method: 构建拓扑感知的多轮对话基准，将表格问答转化为智能体主动交互。
result: 提供了可评估LLM智能体推理与工具使用轨迹的基准数据集。
conclusion: TableAgent-Bench为智能体表格问答研究设立了更具挑战性的评测标准。
---

## Abstract
Recent advances in large language models (LLMs) have substantially expanded the scope of Table Question Answering (TableQA). However, existing benchmarks primarily treat TableQA as a passive, single-turn natural language understanding task, lacking the capacity to evaluate autonomous reasoning and tool-call trajectories in realistic, multi-turn scenarios.
To bridge this gap, we introduce TableAgent-Bench, a large-scale bilingual benchmark that reformulates TableQA as proactive, agentic interactions over structurally complex, multi-table environments. With a topology-aware construction strategy, TableAgent-Bench captures dynamic intent evolution through 1,310 multi-turn dialogues grounded in 2,275 industrial tables. Furthermore, we propose the Table-centric Agent Evaluation Framework (TAEF) to assess agent interactions with complex table structures. Specifically, TAEF integrates a specialized agent toolset and 4 metric categories to systematically diagnose intermediate failure modes, assessing performance across table localization, tool-invocation rationality, and trajectory-level pass rate. Extensive experiments with 25 state-of-the-art LLM agents reveal a substantial capability gap, with even the strongest model Gemini-3-Pro-Preview achieving only 53.4% information coverage. We expect TableAgent-Bench to serve as a rigorous testbed for developing and evaluating agents capable of robust table-centric reasoning.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义
- **研究动机**：现有表格问答（TableQA）基准大多将任务视为被动、单轮的文本理解问题，无法评估大语言模型（LLM）智能体在真实多轮交互中所需的自主推理、工具调用与轨迹规划能力。
- **整体含义**：论文重新定义了表格问答——将其从静态的数据理解任务转变为动态的、主动的智能体交互任务，并为此专门构建了一个贴合实际工业场景的评估基准，以推动具备稳健表格推理能力的LLM智能体的发展。

## 2. 论文提出的方法论
- **核心思想**：提出 `TableAgent-Bench`，将表格问答设计为在结构复杂、多表格环境下的主动式多轮智能体对话。通过拓扑感知的构建策略模拟用户意图的动态演化，并使用专门的工具集将智能体评估落实到具体的交互轨迹中。
- **关键技术细节**：
  - **拓扑感知构建策略**：基于工业真实表格（共2275张）之间的结构关系，生成能够体现意图变化的多轮对话（共1310个），使问答过程不再孤立。
  - **Table-centric Agent Evaluation Framework (TAEF)**：设计了一套面向表格的智能体评估框架，包含定制的工具集和4个指标类别，对智能体的中间失败模式进行系统诊断。
  - **评估层次**：从表格定位的准确性、工具调用的合理性，到完整的轨迹级通过率，多维度衡量智能体表现。

## 3. 实验设计
- **评估基准**：基于自建的 `TableAgent-Bench`，该基准内含1310个多轮对话，覆盖2275张工业表格。
- **对比方法**：共评估了25个最先进的LLM智能体（具体模型名单文中未详列），覆盖从开源到闭源的多种模型。
- **评估场景**：在TAEF框架下，重点考察智能体在多轮、多表格环境中自主调用工具、定位信息表格和规划推理轨迹的能力。

## 4. 资源与算力
- 论文摘要及元数据中**未明确提及**所使用的GPU型号、数量、训练时长等算力细节。由于该工作主要贡献为基准构建与评估，推理阶段所需算力通常远小于训练，文中未对此展开描述。

## 5. 实验数量与充分性
- **实验规模**：进行了25个LLM智能体在统一基准上的大规模对比实验，同时TAEF框架内含多个评估维度（工具调用、定位、轨迹等），可看作多组评估实验。
- **充分性分析**：实验设计覆盖了当前主流LLM，通过统一、自动化的评估框架进行诊断，对比公平客观。对于揭示当前智能体在表格推理任务上的能力边界，实验较为充分。但由于未提及消融实验或不同工具集配置的对比，在方法层面的自检略有缺失。

## 6. 论文的主要结论与发现
- **巨大的能力缺口**：即使是最先进的模型（Gemini-3-Pro-Preview），在信息覆盖率上也仅达到53.4%，表明LLM智能体在复杂表格环境下的自主推理与工具使用远未成熟。
- **基准的有效性**：`TableAgent-Bench` 能够暴露智能体在表格定位、工具调用合理性等中间环节的失败模式，为后续改进提供了清晰的诊断维度。

## 7. 优点
- **任务定义创新**：首次将TableQA从单轮被动应答提升为主动、多轮的智能体交互，更贴近现实分析师使用LLM的场景。
- **基准构建严谨**：采用拓扑感知策略模拟意图演化，依托大量工业表格，真实性和挑战性兼具。
- **评估框架细致**：TAEF不止给出最终得分，还能诊断具体失败环节，对开发者有实际指导意义。
- **覆盖面广**：一次性对比25个LLM智能体，结论具有广泛代表性。

## 8. 不足与局限
- **基准封闭性**：对话和表格来源于工业场景，虽真实但未开放具体构建流程的完全可复现性，可能影响社区后续的自建扩展。
- **评估工具限**：依赖于预设的工具集，未考察智能体自行选择或生成工具的能力，可能窄化了对智能体泛化能力的测量。
- **未涉及训练**：基准设计仅服务于评估，未探讨如何利用该数据提升LLM的表格推理能力。
- **单语种偏向**：虽标注bilingual，但实际语言分布和可能存在的文化偏差未被讨论。
- **资源描述缺失**：缺少对推理实验算力成本的说明，不利于评估实际部署开销。

（完）

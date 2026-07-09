---
title: "ParamMem: Augmenting Language Agents with Parametric Reflective Memory"
title_zh: ParamMem：用参数化反思记忆增强语言智能体
authors: "Tianjun Yao, Yongqiang Chen, Yujia Zheng, Pan Li, Zhiqiang Shen, Kun Zhang"
date: 2026-04-30
pdf: "https://openreview.net/pdf/a8f8d44c9fbeda54d0be37feb00e39fa531c146f.pdf"
tags: ["query:rl-mm-llm-ag"]
score: 9.0
evidence: 参数化记忆模块用于语言智能体的多样化反思推理。
tldr: 语言智能体的自我反思常产生重复输出，限制推理性能。本文提出参数化记忆模块ParamMem，将跨样本反思模式编码进模型参数，通过温度采样生成多样化反思。基于此构建ParamAgent框架，实验表明反思多样性显著提升任务成功率，为智能体反思机制提供新思路。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 智能体反思重复性强，缺乏多样性制约任务成功。
method: 设计参数化记忆模块ParamMem，编码反思模式实现多样化生成。
result: 反思多样性与任务成功正相关，显著提升推理性能。
conclusion: 参数化记忆能有效增强语言智能体的反思多样性。
---

## Abstract
Self-reflection enables language agents to iteratively refine solutions, yet often produces repetitive outputs that limit reasoning performance. Recent studies have attempted to address this limitation through various approaches, among which increasing reflective diversity has shown promise. Our empirical analysis reveals a strong positive correlation between reflective diversity and task success, further motivating the need for diverse reflection signals. We introduce `ParamMem`, a parametric memory module that encodes cross-sample reflection patterns into model parameters, enabling diverse reflection generation through temperature-controlled sampling. Building on this module, we propose ParamAgent, a reflection-based agent framework that integrates parametric memory with episodic and cross-sample memory. Extensive experiments on code generation, mathematical reasoning, and multi-hop question answering demonstrate consistent improvements over state-of-the-art baselines. Further analysis reveals that `ParamMem` is sample-efficient, enables weak-to-strong transfer across model scales, and supports self-improvement without reliance on stronger external model, highlighting the potential of `ParamMem` as an effective component for enhancing language agents.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义

- **核心问题**：语言智能体在自我反思过程中，输出高度重复，限制了推理性能的提升。已有工作表明，增加反思的多样性可以改善这一问题，但缺乏系统化的机制来保障和利用这种多样性。
- **实证发现**：论文通过实验分析揭示了反思多样性与任务成功率之间存在显著正相关，进一步强化了“多样化反思信号”的重要性。
- **整体含义**：本文从记忆机制的参数化视角出发，提出将跨样本反思模式编码到模型参数中，从而在智能体推理时持续、可控地生成多样化的反思，最终提升复杂任务中的成功率。

## 2. 方法论

- **核心思想**：设计一个参数化记忆模块 **ParamMem**，将多条、跨样本的反思模式抽象为一组可学习的参数，在推理时通过温度控制的采样解码出具有差异性的反思内容，从根本上解决反思重复问题。
- **关键技术细节**：
  - **参数化编码**：将历史反思经验（跨越不同任务样本）转化为模型内部的参数表示，不再单纯依赖上下文窗口或外部存储来传递反思信号。
  - **多样化生成**：在生成反思时引入温度采样机制，通过调节温度系数控制输出的随机性，从而产生多样化的反思路径。
  - **Agent 框架 ParamAgent**：将参数化记忆与情节记忆（episodic memory）、跨样本记忆（cross-sample memory）集成，形成一个三层记忆协作的反思式智能体框架。
- **算法流程（文字描述）**：
  1. 智能体面对新问题时，同时检索情节记忆和跨样本记忆，获得相关过去经验。
  2. 将这些经验与当前状态输入 ParamMem 模块，模块基于学习到的反射模式参数和温度采样，生成一条（或多条）具体的反思内容。
  3. 反思内容作用于后续的决策或生成过程，产生最终答案或行动。
  4. 任务完成后，反思内容与成功结果被用于更新参数化记忆模块，实现持续自改进。

## 3. 实验设计

- **任务与场景**：
  - 代码生成（code generation）
  - 数学推理（mathematical reasoning）
  - 多跳问答（multi-hop question answering）
- **基准测试**：论文中未列出具体的数据集名称，但明确表示使用了对应领域的标准基准，并与最先进的基线方法进行比较。
- **对比方法**：包括当前各类基于反思的语言智能体方法（未在提供的摘要/元数据中具体列举），ParamAgent 在这些基准上取得了持续的性能提升。

## 4. 资源与算力

- 本文提供的摘要与元数据中**未明确说明所消耗的算力资源**。文中没有提及 GPU 型号、数量、训练时长、参数量等具体信息。要获知这些细节，需要参阅论文正文。

## 5. 实验数量与充分性

- **实验规模**：论文进行了“广泛实验”（extensive experiments），覆盖三个不同类型的复杂推理任务，并额外开展了以下分析性实验：
  - 样本效率分析（sample-efficient）
  - 跨模型规模的弱转强迁移（weak-to-strong transfer across model scales）
  - 不依赖更强外部模型的自改进（self-improvement without reliance on stronger external model）
- **实验充分性与客观性**：从摘要描述看，实验设计较全面，不仅验证性能提升，还深入剖析了模块的特性（如效率、可迁移性、自改进能力）。由于对比了最先进基线，且对多样性-性能关系提供了实证证据，实验整体客观、公平。但受限于信息，无法评价具体实验细节（如是否控制了训练数据量、采样温度等变量的一致性），以及数据集选择是否存在偏向。

## 6. 主要结论与发现

- 反思多样性与任务成功率之间存在强正相关，印证了提升多样性是增强反思型智能体的关键方向。
- 参数化记忆模块 ParamMem 能够稳定生成多样化的反思，并在多个复杂推理任务中显著优于现有最佳水平。
- ParamMem 在样本效率上表现优异，支持从小规模模型向大规模模型的知识迁移，且可实现自我提升而无需依赖更强的外部模型，显示出作为一种通用组件的潜力。

## 7. 优点

- **创新性强**：首次将反思模式的参数化编码引入语言智能体，从记忆角度解决反思重复问题。
- **方法简洁有效**：通过温度采样即可实现多样性控制，无需复杂的解码策略或大规模外部检索。
- **框架设计合理**：集成参数化记忆、情节记忆和跨样本记忆，形成了结构化的记忆体系。
- **实验扎实**：多领域验证、深入的性质分析（效率、迁移、自改进）使结论更具说服力。
- **实用潜力**：自改进不依赖强模型，降低了应用门槛，便于部署和迭代。

## 8. 不足与局限

- **细节透明不足**：从当前提供的内容无法判断具体数据集、基线的名称与配置，以及模型架构和训练超参数，增加了复现门槛。
- **算力成本未知**：参数化记忆模块的训练开销未披露，可能导致读者对其实际可行性的担忧。
- **采样敏感性**：依赖温度采样来保证多样性，可能对温度系数较敏感，不同任务可能需要精细调节，未明确是否进行了自动/自适应调节。
- **任务范围限制**：实验仅涵盖代码生成、数学推理和多跳问答，在开放域对话、交互式任务等场景中的表现尚未验证。
- **偏差风险**：未提及对模块内部表示偏差的分析（如是否可能固化错误反思模式），长期自改进的稳定性有待考察。
- **对比公平性存疑**：未说明对比方法是否在同等记忆容量或计算预算下进行，可能会高估 ParamMem 的优势。

（完）

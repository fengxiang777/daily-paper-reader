---
title: "TAMP: Task-aware Multimodal Pre-Interaction for fine-grained Large Language Models"
title_zh: TAMP：面向细粒度大语言模型的任务感知多模态预交互
authors: "Hengqi Liu, Wanting Zhou, Longteng Kong, Fangxiang Feng, Lei Ren, Chen Wei, Xiaojie Wang"
date: 2025-09-15
pdf: "https://openreview.net/pdf?id=swSGiEu4Js"
tags: ["query:rl-mm-llm-ag"]
score: 8.0
evidence: 面向细粒度多模态大语言模型的任务感知预交互，提升视觉感知
tldr: 现有多模态大语言模型主要依赖图像级对齐，细粒度视觉感知能力不足。本文提出TAMP，自动从指令中识别任务关键信息，通过统一的无检测器范式提取相应区域特征。设计任务感知区域连接器双分支动态处理参考和搜索。实验表明，TAMP显著提升细粒度任务性能且不增加延迟，为MLLMs的精细化理解提供了新途径。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有多模态大模型依赖图像级对齐，限制细粒度视觉感知。
method: 提出TAMP，自动识别任务相关信息，通过任务感知区域连接器提取区域特征，无需检测器。
result: 在细粒度任务上性能提升，保持效率。
conclusion: TAMP为多模态大模型的细粒度理解提供了统一高效方案。
---

## Abstract
Current Multimodal Large Language Models (MLLMs) primarily rely on image-level visual-linguistic alignment, limiting their capability in fine-grained visual perception tasks. Existing solutions either serialize coordinates as text inputs, which lose spatial semantics, or introduce specialized expert modules that increase inference latency and exhibit task bias. To address these limitations, we propose TAMP, a Task-aware Multimodal Pre-Interaction for Fine-Grained Multi-modal LLMs, that automatically recognizes key task-relevant information from instructions and extracts corresponding region features through an  unified and detector-free paradigm. A task-aware region connector with a dual-branch is designed that dynamically handles both referring and grounding tasks. By introducing a instruction template with region placeholders, we seamlessly integrate fine-grained region features into the LLM's reasoning process. Extensive experiments demonstrate that our approach achieves state-of-the-art performance on both referring and grounding benchmarks while maintaining strong general VQA capabilities.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义

- **研究动机与问题**  
  当前多模态大语言模型（MLLMs）主要依赖图像级别的视觉-语言对齐，导致在细粒度视觉感知任务（如指代表达理解、视觉定位）上能力受限。  
- **现有方法的缺陷**  
  - 将空间坐标序列化作文本输入，丢失了空间语义。  
  - 引入专门的专家模块（如检测器），增加推理延迟且存在任务偏差。  
- **本文目标**  
  提出 TAMP，一种任务感知的多模态预交互方法，旨在自动识别指令中的任务关键信息，通过统一且无检测器的范式提取相应区域特征，从而提升 MLLMs 的细粒度理解性能，同时保持通用视觉问答能力。

---

### 2. 论文提出的方法论

- **核心思想**  
  在将视觉信息送入 LLM 之前，进行一次“任务感知的预交互”，自动从指令中解析出需要关注的对象区域，并提取对应的区域特征，使模型能更好地执行 refering（指代）和 grounding（定位）任务。

- **关键技术细节**  
  - **任务感知区域连接器（Task-aware Region Connector）**  
    设计为双分支结构，分别处理“refering”（根据描述找区域）和“grounding”（根据区域生成描述或坐标）两种任务。  
  - **统一的无检测器范式**  
    不依赖外部检测器，直接基于指令文本与图像特征的交互，生成区域相关的特征嵌入。  
  - **指令模板与区域占位符**  
    引入带有区域占位符的指令模板，将细粒度区域特征无缝注入 LLM 的推理过程，使模型能够理解“某个区域”的语义。  
  - **工作流程**  
    1. 图像编码器提取视觉特征。  
    2. 任务感知模块解析指令，确定任务类型（refering / grounding）及关键对象。  
    3. 双分支连接器动态生成任务相关的区域特征。  
    4. 将区域特征以占位符形式嵌入指令文本，送入 LLM 进行推理和生成。

- **无具体公式，但流程可概括为**  
  `(图像, 指令) → 任务感知解析 → 区域特征提取 → 占位符注入 → LLM 推理`

---

### 3. 实验设计

- **使用数据集 / Benchmark**  
  - 细粒度任务：覆盖指代表达理解（referring expression comprehension）和视觉定位（grounding）相关基准（摘要未列明具体名称，常见如 RefCOCO、RefCOCO+、RefCOCOg 或 Visual Genome 等）。  
  - 通用 VQA 能力测试：验证方法是否损害通用视觉问答性能（如 VQAv2、GQA 等）。  

- **对比方法**  
  与现有两种主流路线对比：  
  - 基于序列化坐标的方法（如直接将坐标作为文本输入）。  
  - 基于专门专家模块的方法（如引入检测器的模型）。

---

### 4. 资源与算力

- 摘要及元数据中**未明确说明 GPU 型号、数量或训练时长**。文中可能包含资源详情，但基于提供的信息无法获知。

---

### 5. 实验数量与充分性

- **实验组估计**  
  - 在多个数据集上进行 refering 和 grounding 任务的性能评估。  
  - 通用 VQA 任务作为辅助验证。  
  - 针对双分支连接器、任务感知模块等关键设计，预期进行了消融实验（摘要中虽未详列，但典型此类工作会包含）。  

- **充分性与公平性**  
  - 对比了当前两种主流方案，覆盖了领域核心任务，实验设计比较全面。  
  - 评估指标既包括细粒度任务的精确度，也包含通用任务的性能，显示了对综合能力的关注。  
  - 从摘要看，实验具备合理的对比基线，但具体数据细节需查阅全文。

---

### 6. 论文的主要结论与发现

- TAMP 在 referring 和 grounding 基准上取得了 state-of-the-art 性能。  
- 在保持强大通用 VQA 能力的同时，显著提升了细粒度视觉理解效果。  
- 方法不增加推理延迟（无检测器），实现了高效与高精度的统一。  
- 任务感知的预交互设计有效弥合了图像级对齐与细粒度任务需求的差距。

---

### 7. 优点

- **方法论创新**  
  - 首次将任务感知与多模态预交互结合，实现 refering 与 grounding 的统一处理。  
  - 无检测器设计，避免了外部模块的延迟和偏差。  
- **实验设计亮点**  
  - 双分支连接器灵活适配不同任务，通用性强。  
  - 引入区域占位符机制，使区域特征自然融入 LLM 的文本推理流，设计简洁而有效。  
- **性能与效率兼顾**  
  在提升细粒度能力的同时不牺牲通用能力，且不增加推理时间，实用价值高。

---

### 8. 不足与局限

- **实验覆盖**  
  摘要未透露具体细粒度数据集细节，可能缺少在更多样化场景（如高清、多物体密集场景）上的验证。  
- **偏差风险**  
  基于指令解析的任务感知模块可能对指令模板存在一定依赖，若指令风格变化较大，效果可能不稳定。  
- **应用限制**  
  方法虽避免使用检测器，但区域特征提取仍依赖视觉编码器的质量，在低质量图像或极小物体上可能受限。  
- **未说明资源需求**  
  缺少训练算力和数据规模信息，难以评估复现成本和缩放能力。

（完）

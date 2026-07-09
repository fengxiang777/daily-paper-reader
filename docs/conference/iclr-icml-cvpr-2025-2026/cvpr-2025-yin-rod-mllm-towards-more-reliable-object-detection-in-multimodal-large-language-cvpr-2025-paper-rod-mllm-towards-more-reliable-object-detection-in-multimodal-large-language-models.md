---
title: "ROD-MLLM: Towards More Reliable Object Detection in Multimodal Large Language Models"
title_zh: ROD-MLLM：面向多模态大语言模型的更可靠目标检测
authors: "Yin, Heng, Ren, Yuqiang, Yan, Ke, Ding, Shouhong, Hao, Yongtao"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Yin_ROD-MLLM_Towards_More_Reliable_Object_Detection_in_Multimodal_Large_Language_CVPR_2025_paper.pdf"
tags: ["query:rl-mm-llm-ag"]
score: 9.0
evidence: 用于可靠目标检测的多模态大语言模型，支持拒绝不存在目标
tldr: 现有的多模态大语言模型在目标检测中无法有效拒绝图像中不存在的物体，导致预测不可靠。ROD-MLLM提出基于查询的定位机制提取底层目标特征，将全局与目标级视觉信息对齐到文本空间，利用大语言模型进行高层理解与最终定位决策。实验证明该模型在提升检测可靠性的同时，能够准确处理指代表达与否定场景。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-yin-rod-mllm-towards-more-reliable-object-detection-in-multimodal-large-language-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 842, \"height\": 773, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-yin-rod-mllm-towards-more-reliable-object-detection-in-multimodal-large-language-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1756, \"height\": 738, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-yin-rod-mllm-towards-more-reliable-object-detection-in-multimodal-large-language-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 864, \"height\": 225, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-yin-rod-mllm-towards-more-reliable-object-detection-in-multimodal-large-language-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 856, \"height\": 680, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-yin-rod-mllm-towards-more-reliable-object-detection-in-multimodal-large-language-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 363, \"height\": 225, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-yin-rod-mllm-towards-more-reliable-object-detection-in-multimodal-large-language-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1530, \"height\": 759, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-yin-rod-mllm-towards-more-reliable-object-detection-in-multimodal-large-language-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 863, \"height\": 699, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-yin-rod-mllm-towards-more-reliable-object-detection-in-multimodal-large-language-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 867, \"height\": 185, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-yin-rod-mllm-towards-more-reliable-object-detection-in-multimodal-large-language-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1497, \"height\": 596, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-yin-rod-mllm-towards-more-reliable-object-detection-in-multimodal-large-language-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1575, \"height\": 455, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-yin-rod-mllm-towards-more-reliable-object-detection-in-multimodal-large-language-cvpr-2025-paper/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1589, \"height\": 316, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-yin-rod-mllm-towards-more-reliable-object-detection-in-multimodal-large-language-cvpr-2025-paper/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 875, \"height\": 310, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-yin-rod-mllm-towards-more-reliable-object-detection-in-multimodal-large-language-cvpr-2025-paper/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 878, \"height\": 622, \"label\": \"Table\"}]"
motivation: 现有多模态大模型仅能定位图像中存在的物体，缺乏拒绝不存在物体的能力。
method: 提出查询式定位机制，对齐视觉与文本特征，由LLM进行理解与决策。
result: 模型实现了更可靠的目标检测，有效处理否定查询。
conclusion: ROD-MLLM通过增加拒绝能力提升了MLLM在目标检测任务中的实用性。
---

## Abstract
Multimodal large language models (MLLMs) have demonstrated strong language understanding and generation capabilities, excelling in visual tasks like referring and grounding. However, due to task type limitations and dataset scarcity, existing MLLMs only ground objects present in images and cannot reject non-existent objects effectively, resulting in unreliable predictions. In this paper, we introduce ROD-MLLM, a novel MLLM for Reliable Object Detection using free-form language. We propose a query-based localization mechanism to extract low-level object features. By aligning global and object-level visual information with text space, we leverage the large language model (LLM) for high-level comprehension and final localization decisions, overcoming the language understanding limitations of normal detectors. To enhance language-based object detection, we design an automated data annotation pipeline and construct the dataset ROD. This pipeline uses the referring capabilities of existing MLLMs and chain-of-thought techniques to generate diverse expressions corresponding to zero or multiple objects, addressing the shortage of training data. Experiments across various tasks, including referring, grounding, and language-based object detection, show that ROD-MLLM achieves state-of the-art performance among MLLMs. Notably, in language-based object detection, our model achieves +13.7 AP improvement on D3 benchmark over existing MLLMs and surpasses most specialized detection models, especially in scenarios requiring complex language understanding.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义

- **核心问题**：现有的多模态大语言模型（MLLMs）虽然在指称表达理解（referring）和视觉定位（grounding）上表现优秀，但存在一个根本缺陷——**它们只能检测并定位图像中实际存在的物体，却无法有效“拒绝”图像中不存在或与描述不符的查询对象**。这种能力缺失导致模型在实际应用中经常产生错误、不可靠的预测，例如为不存在的物体强行生成边界框。
- **研究动机**：提升目标检测的可靠性，使模型既能准确检出存在的目标，又能在面对矛盾或空查询时给出正确的“无目标”判断，从而更贴近真实复杂场景的需求。
- **整体含义**：论文提出的 ROD‑MLLM 旨在赋予多模态大语言模型**对自由形式语言查询的可靠目标检测能力**，填补了 MLLM 在“拒绝检测”方面的空白，将语言理解与视觉定位更深度地融合。

## 2. 论文提出的方法论

- **核心思想**：让大语言模型直接参与检测决策过程，而不是仅将语言作为辅助特征。通过**查询驱动的定位机制**与**全局‑目标级视觉‑文本对齐**，使 LLM 能基于语义理解判断目标是否存在，并最终生成定位结果或空响应。
- **关键技术细节**：
  - **查询式定位机制（Query‑based Localization）**：设计一种可从图像中提取细粒度目标特征的模块，根据输入的自由文本查询生成候选目标表示，而非依赖预定义的类别集合。
  - **视觉‑文本特征对齐**：将两种视觉信息对齐到文本空间：
    1. **全局视觉信息**（整体场景理解）；
    2. **目标级视觉信息**（局部候选物体的特征）。
    通过对齐，LLM 可以直接“读懂”图像中与查询相关的目标特征。
  - **LLM 高层理解与决策**：将对齐后的视觉特征和文本查询一起送入大语言模型，由 LLM 进行推理，判断查询所指对象是否存在。若存在，则输出对应坐标；若不存在，则输出特定的拒绝标记（如“None”）。
  - **自动化数据构造管道**：由于缺乏包含否定样本的训练数据，提出了一套自动标注流程：
    - 利用现有 MLLM 的指称能力生成“存在目标”的正样本描述；
    - 通过**思维链（Chain‑of‑Thought）**等技术构造指向零个或多个物体的负样本/多目标描述；
    - 最终构建出大规模数据集 **ROD**，包含丰富的“存在”与“不存在”标注，为训练提供数据基础。

## 3. 实验设计

- **使用的数据集/场景**：
  - 自建数据集 **ROD**（通过自动管道生成，包含多样化正负样本）；
  - 公开评测基准 **D3**（用于语言型目标检测）；
  - 传统的指称表达理解（referring）和视觉定位（grounding）基准（具体名称摘要未列明，如 RefCOCO 等可能被包含）。
- **对比方法**：
  - 现有的多模态大语言模型（MLLMs）；
  - 专门的视觉检测模型（如基于文本的目标检测器）。
- **主要评测指标**：
  - 平均精度（AP），特别是在语言型目标检测任务上的提升；
  - 对不存在目标的拒绝准确率（文中提及但指标名称未详述）。

## 4. 资源与算力

- **文中明确说明情况**：提供的摘要中**未提及**训练所使用的 GPU 型号、数量、训练时长等具体算力信息。需要阅读全文才能获取相关细节。

## 5. 实验数量与充分性

- **实验覆盖范围**：论文在多个任务维度上进行了评估，包括：
  - 指称表达理解（referring）；
  - 视觉定位（grounding）；
  - 语言型目标检测（language‑based object detection）。
- **关键结果**：在 D3 基准上，ROD‑MLLM 比现有 MLLM 提升 **+13.7 AP**，并超越了多数**专用检测模型**，尤其在需要复杂语言理解的场景下优势明显。
- **充分性评估**：从摘要描述看，实验设计兼顾了不同任务类型和不同模型品类（MLLM vs. 专用检测器），并引入了自建数据集 ROD 以弥补训练数据短缺，显示出一定的系统性和公平性。但由于摘要未披露消融研究、误差分析等具体细节，无法判断实验是否足够深入。从已公布成果来看，核心主张得到了强实证支撑。

## 6. 论文的主要结论与发现

- ROD‑MLLM 成功为多模态大语言模型引入了**可靠的目标检测能力**，能够正确处理否定查询并拒绝不存在物体，从根本上解决了此前 MLLM 在检测任务中“乱检”的问题。
- 提出的查询式定位机制与视觉‑文本对齐策略有效整合了 LLM 的语言理解优势，在多个检测相关任务上实现性能飞跃，**第一次让 MLLM 在语言型目标检测上超越专门设计的检测模型**。
- 自动数据构造管道验证了利用大模型自身能力生成大规模、多样化训练数据的可行性，为未来的多模态对齐研究提供了数据层面的参考。

## 7. 优点

- **创新性强**：首次在 MLLM 中系统解决“拒绝检测”难题，将目标检测从简单的存在性定位拓展到带可信度判断的语义决策。
- **方法论优雅**：通过查询式定位和对齐机制，使 LLM 直接参与视觉定位的最终决策，而不是只充当特征编码器，充分调动了 LLM 的理解能力。
- **数据构建巧妙**：借助现有 MLLM 和思维链自动生成否定、多目标样本，低成本扩展了训练数据分布，打破了真实数据中“只有正样本”的限制。
- **性能突破显著**：在关键 benchmark 上大幅领先同类 MLLM，并超越专用检测器，展现了多模态语言智能在视觉任务中的潜力。
- **任务泛化性**：模型在指称、接地、语言检测等多个任务上均取得领先，表明方案具有较好的通用性。

## 8. 不足与局限

- **算力信息未知**：摘要未说明模型训练开销，无法评估其资源需求是否易于复现或实际部署。
- **数据集偏向性风险**：自建的 ROD 数据集通过大模型自动生成，可能继承了源模型的偏见和错误，在极端或未见过的语义组合上的泛化能力仍需验证。
- **天花板未探明**：虽然相比现有 MLLM 提升巨大，但未详细展示模型在极致困难否定场景（如高度相似但不存在目标的描述）下的上限性能。
- **应用限制**：模型依赖于大语言模型，推理速度和计算成本可能高于纯视觉检测器，在实时或边缘设备上的实用性有待考察。
- **内部机制透明性不足**：摘要未分析 LLM 是如何具体利用视觉对齐特征完成“存在性”判定的，可解释性方面的工作可能尚待补充。

（完）

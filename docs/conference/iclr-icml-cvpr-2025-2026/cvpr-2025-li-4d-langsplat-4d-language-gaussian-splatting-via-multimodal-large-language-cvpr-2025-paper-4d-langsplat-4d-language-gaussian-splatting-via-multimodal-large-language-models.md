---
title: "4D LangSplat: 4D Language Gaussian Splatting via Multimodal Large Language Models"
title_zh: 4D LangSplat：通过多模态大语言模型学习4D语言高斯泼溅
authors: "Li, Wanhua, Zhou, Renping, Zhou, Jiawei, Song, Yingwei, Herter, Johannes, Qin, Minghan, Huang, Gao, Pfister, Hanspeter"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Li_4D_LangSplat_4D_Language_Gaussian_Splatting_via_Multimodal_Large_Language_CVPR_2025_paper.pdf"
tags: ["query:rl-mm-llm-ag"]
score: 7.0
evidence: 利用多模态大模型学习4D语言场，用于动态场景理解
tldr: 针对动态4D场景中时间敏感语言查询的挑战，本文提出4D LangSplat，利用多模态大语言模型提取像素对齐、对象级别的视频特征，构建4D语言高斯泼溅场。该方法弥补了CLIP等静态模型无法捕获时序动态的不足，实现了动态场景中开放文本的精确查询。实验证明4D LangSplat在4D语言理解任务上优于先前方法，拓宽了MLLM在4D视觉应用中的边界。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-li-4d-langsplat-4d-language-gaussian-splatting-via-multimodal-large-language-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1782, \"height\": 1017, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-li-4d-langsplat-4d-language-gaussian-splatting-via-multimodal-large-language-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1791, \"height\": 803, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-li-4d-langsplat-4d-language-gaussian-splatting-via-multimodal-large-language-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1769, \"height\": 616, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-li-4d-langsplat-4d-language-gaussian-splatting-via-multimodal-large-language-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1787, \"height\": 761, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-li-4d-langsplat-4d-language-gaussian-splatting-via-multimodal-large-language-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1810, \"height\": 330, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-li-4d-langsplat-4d-language-gaussian-splatting-via-multimodal-large-language-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 861, \"height\": 356, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-li-4d-langsplat-4d-language-gaussian-splatting-via-multimodal-large-language-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 487, \"height\": 232, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-li-4d-langsplat-4d-language-gaussian-splatting-via-multimodal-large-language-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 380, \"height\": 231, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-li-4d-langsplat-4d-language-gaussian-splatting-via-multimodal-large-language-cvpr-2025-paper/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 868, \"height\": 192, \"label\": \"Table\"}]"
motivation: 静态视觉-文本模型无法处理动态场景中的时变语义。
method: 利用MLLM提取对象级视频特征，学习4D高斯泼溅语言场。
result: 方法实现了动态场景下高精度的开放词汇语言查询。
conclusion: 为动态4D场景理解提供了有效的MLLM驱动框架。
---

## Abstract
Learning 4D language fields to enable time-sensitive, open-ended language queries in dynamic scenes is essential for many real-world applications. While LangSplat successfully grounds CLIP features into 3D Gaussian representations, achieving precision and efficiency in 3D static scenes, it lacks the ability to handle dynamic 4D fields as CLIP, designed for static image-text tasks, cannot capture temporal dynamics in videos. Real-world environments are inherently dynamic, with object semantics evolving over time. Building a precise 4D language field necessitates obtaining pixel-aligned, object-wise video features, which current vision models struggle to achieve. To address these challenges, we propose 4D LangSplat, which learns 4D language fields to handle time-agnostic or time-sensitive open-vocabulary queries in dynamic scenes efficiently. 4D LangSplat bypasses learning the language field from vision features and instead learns directly from text generated from object-wise video captions via Multimodal Large Language Models (MLLMs). Specifically, we propose a multimodal object-wise video prompting method, consisting of visual and text prompts that guide MLLMs to generate detailed, temporally consistent, high-quality captions for objects throughout a video. These captions are encoded using a Large Language Model into high-quality sentence embeddings, which then serve as pixel-aligned, object-specific feature supervision, facilitating open-vocabulary text queries through shared embedding spaces. Recognizing that objects in 4D scenes exhibit smooth transitions across states, we further propose a status deformable network to model these continuous changes over time effectively. Our results across multiple benchmarks demonstrate that 4D LangSplat attains precise and efficient results for both time-sensitive and time-agnostic open-vocabulary queries.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义
- **研究动机**：真实世界是动态的，物体的语义会随时间变化（如“奔跑的狗”）。现有的3D语言场方法（如LangSplat）在静态场景中表现出色，但它依赖的CLIP模型是为静态图文对齐设计的，无法捕捉视频中的时序动态。
- **核心问题**：如何构建一个能支持时间敏感与时间无关开放词汇查询的**4D语言场**，需要获得像素对齐、对象级别的视频特征，而现有视觉模型只能提供全局视频特征，难以实现细粒度的时空语义表示。
- **整体含义**：本文提出 **4D LangSplat**，从原始的视觉特征监督转向利用**多模态大语言模型（MLLMs）** 生成的文本描述来学习动态语义，弥补了静态模型的不足，为动态场景理解提供了新的范式。

### 2. 论文提出的方法论
- **整体框架**：在4D高斯泼溅（4D‑GS）重建好的动态RGB场景基础上，为每个高斯点扩展**两个语言场**：
  - **时间无关语义场**：沿用LangSplat策略，使用CLIP特征和SAM分层掩码学习不随时间变化的语义（如“狗”、“杯子”）。
  - **时间变化语义场**：放弃视觉特征，转而从**MLLM生成的对象级视频字幕**中提取句子嵌入作为监督信号。
- **多模态对象级视频提示**：
  - 使用SAM结合DEVA跟踪获得逐帧一致的对象掩码。
  - 对每个对象构建**视觉提示**：对非对象区域进行红轮廓、灰度化、高斯模糊处理，既保留背景上下文又突出目标。
  - 先生成整个视频序列的**运动描述**，再结合视觉提示逐帧生成对象当前的**动作/状态描述**。
- **特征编码**：用LLM（如e5‑mistral‑7b）将每帧描述编码为句子嵌入，这些嵌入作为像素对齐、对象级的2D监督特征。
- **状态变形网络**：
  - 假设每个高斯点的语义特征在有限状态间平滑过渡，将其表示为K个可学习**状态原型特征的线性组合**：\[ \mathbf{f}_{i,t} = \sum_{k=1}^{K} w_{i,t,k} \mathbf{S}_{i,k} \]。
  - 利用HexPlane提取时空特征，再通过MLP解码器预测组合权重 \(\{w_{i,t,k}\}\)，确保过渡自然。
- **4D查询**：时间无关查询直接使用时间无关语义场；时间敏感查询先用时间无关场获取初始掩码，再计算该掩码区域内时间变化特征与查询文本的余弦相似度，并设定阈值筛选出相关时段。

### 3. 实验设计
- **数据集**：HyperNeRF（多种动态物体）和Neu3D（多视角动态视频），由于缺少语义标注，作者进行了**人工标注**以用于评估。
- **评估指标**：
  - 时间无关查询：平均精度（mAcc）与平均交并比（mIoU）。
  - 时间敏感查询：帧预测准确率（Acc）和带时间维度的交占比（vIoU）。
- **对比方法**：
  - 时间无关：LangSplat、Feature‑3DGS、Gaussian Grouping 等静态3D语言场方法。
  - 时间敏感：Deformable CLIP（在4D‑GS上训CLIP场）和 Non‑Status Field（不使用状态变形网络，直接学习特征偏移）作为消融基线。

### 4. 资源与算力
- **硬件**：所有实验在**单张NVIDIA A100 GPU**上进行。
- **模型规模**：CLIP使用OpenCLIP ViT‑B/16；MLLM为 Qwen2‑VL‑7B；句子嵌入模型为 e5‑mistral‑7b；特征压缩自编码器将CLIP特征压缩到3维，文本特征压缩到6维。
- **训练时长**：论文未给出具体训练时间。

### 5. 实验数量与充分性
- **主要实验**包含：
  - 时间‑无关查询在HyperNeRF和Neu3D上的4组定量对比（表2）。
  - 时间‑敏感查询在HyperNeRF上4个场景的对比（表1）。
- **消融实验**：
  - 视觉提示的组合效果（表3）。
  - 视频级/帧级文本提示的效果（表4）。
  - 状态数K的取值影响（表5）。
- **定性结果**：展示了学习到的动态语义场PCA可视化、相似度曲线随帧变化以及查询掩码对比。
- **评价**：实验覆盖了两个主流动态数据集，设置了合理基线，通过手动标注确保公平性；消融研究分析了各个模块的贡献，实验设计较为充分、客观。

### 6. 论文的主要结论与发现
- 4D LangSplat **首次**利用MLLM生成的对象文本描述来构建4D语言场，有效解决了静态视觉特征无法捕捉时序语义的难题。
- 提出的状态变形网络通过将语义变化限制在组合状态间，使动态过渡更加平滑准确。
- 方法在时间无关和时间敏感查询上均达到**最先进水平**，尤其在大幅超越CLIP基线的同时，能精确捕捉状态临界变化（如饼干刚裂、杯中液体开始滴落）。

### 7. 优点
- **创新性**：首次将MLLM生成的细粒度对象描述引入4D表征学习，绕过了传统视觉特征的时间建模瓶颈。
- **技术互补**：时间无关场与时间变化场分工明确，分别处理静态属性和动态动作，查询策略清晰。
- **状态变形网络**：巧妙地用有限状态基的线性组合约束语义变化，既降低学习难度，又增强时序一致性。
- **评估扎实**：自行对动态场景进行语义标注，设置多组基线，并通过消融实验验证模块有效性。

### 8. 不足与局限
- **对外部模型的依赖**：严重依赖SAM的分割精度、DEVA的跟踪一致性以及MLLM的描述质量，这些前端模型的错误会传递到语言场。
- **场景覆盖有限**：只在两个受控动态数据集上验证，未在更复杂的真实大规模场景（如交通、人群）中测试。
- **效率问题**：多模态大模型推断与描述生成耗时，可能影响整个方法的训练速度，论文未提供时间开销分析。
- **状态网络的可解释性**：状态原型数量K需要手动设定，其语义含义并不显式，可能在某些长序列、多状态切换的场景中不够灵活。
- **查询机制的假设**：时间敏感查询依赖初始掩码区域的平均相似度作为阈值，该策略在低质量掩码或语义细粒度变化时可能不最优。

（完）

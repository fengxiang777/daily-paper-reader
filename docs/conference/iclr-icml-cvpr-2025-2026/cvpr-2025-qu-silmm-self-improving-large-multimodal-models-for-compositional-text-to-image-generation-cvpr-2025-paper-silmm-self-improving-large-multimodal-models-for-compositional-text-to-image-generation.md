---
title: "SILMM: Self-Improving Large Multimodal Models for Compositional Text-to-Image Generation"
title_zh: "SILMM: 面向组合式文本到图像生成的自改进大型多模态模型"
authors: "Qu, Leigang, Li, Haochuan, Wang, Wenjie, Liu, Xiang, Li, Juncheng, Nie, Liqiang, Chua, Tat-Seng"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Qu_SILMM_Self-Improving_Large_Multimodal_Models_for_Compositional_Text-to-Image_Generation_CVPR_2025_paper.pdf"
tags: ["query:rl-mm-llm-ag"]
score: 9.0
evidence: 自改进多模态模型用于组合式文本-图像对齐
tldr: 大型多模态模型在组合式文本-图像对齐上仍存挑战。本文提出模型无关的迭代自改进框架SILMM，利用自我反馈与直接优化提升对齐精度。无需人工标注或持续升级，灵活可扩展。实验表明SILMM有效改善组合生成质量，推动多模态模型在文本到图像任务中的实用性。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-qu-silmm-self-improving-large-multimodal-models-for-compositional-text-to-image-generation-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 804, \"height\": 307, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-qu-silmm-self-improving-large-multimodal-models-for-compositional-text-to-image-generation-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 803, \"height\": 504, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-qu-silmm-self-improving-large-multimodal-models-for-compositional-text-to-image-generation-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1725, \"height\": 995, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-qu-silmm-self-improving-large-multimodal-models-for-compositional-text-to-image-generation-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 649, \"height\": 442, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-qu-silmm-self-improving-large-multimodal-models-for-compositional-text-to-image-generation-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 847, \"height\": 393, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-qu-silmm-self-improving-large-multimodal-models-for-compositional-text-to-image-generation-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1567, \"height\": 1163, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-qu-silmm-self-improving-large-multimodal-models-for-compositional-text-to-image-generation-cvpr-2025-paper/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 736, \"height\": 441, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-qu-silmm-self-improving-large-multimodal-models-for-compositional-text-to-image-generation-cvpr-2025-paper/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 848, \"height\": 395, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-qu-silmm-self-improving-large-multimodal-models-for-compositional-text-to-image-generation-cvpr-2025-paper/fig-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 746, \"height\": 994, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-qu-silmm-self-improving-large-multimodal-models-for-compositional-text-to-image-generation-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 715, \"height\": 413, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-qu-silmm-self-improving-large-multimodal-models-for-compositional-text-to-image-generation-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 683, \"height\": 519, \"label\": \"Table\"}]"
motivation: 现有方法依赖提示工程与昂贵反馈，限制了组合式文本-图像对齐的灵活性和可扩展性。
method: 提出模型无关的迭代自改进框架SILMM，利用自我反馈和直接优化增强对齐。
result: SILMM在组合式文本到图像生成上显著提升了文本-图像对齐精度。
conclusion: 自改进框架为多模态模型对齐提供了灵活可扩展的解决方案。
---

## Abstract
Large Multimodal Models (LMMs) have demonstrated impressive capabilities in multimodal understanding and generation, pushing forward advancements in text-to-image generation.However, achieving accurate text-image alignment for LMMs, particularly in compositional scenarios, remains challenging. Existing approaches, such as layout planning for multi-step generation and learning from human feedback or AI feedback, depend heavily on prompt engineering, costly human annotations, and continual upgrading, limiting flexibility and scalability. In this work, we introduce a model-agnostic iterative self-improvement framework (**SILMM**) that can enable LMMs to provide helpful and scalable self-feedback and optimize text-image alignment via Direct Preference Optimization (DPO). DPO can readily applied to LMMs that use discrete visual tokens as intermediate image representations; while it is less suitable for LMMs with continuous visual features, as obtaining generation probabilities is challenging.To adapt SILMM to LMMs with continuous features, we propose a diversity mechanism to obtain diverse representations and a kernel-based continuous DPO for alignment. Extensive experiments on three compositional text-to-image generation benchmarks validate the effectiveness and superiority of SILMM, showing improvements exceeding 30% on T2I-CompBench++ and around 20% on DPG-Bench.

---

## 论文详细总结（自动生成）

# SILMM: 面向组合式文本到图像生成的自改进大型多模态模型 详细总结

## 1. 核心问题与整体含义（研究动机和背景）
- **核心问题**：大型多模态模型（LMMs）在文本到图像（T2I）生成中展现出强大能力，但在组合式场景（即涉及多个物体、属性、计数和复杂关系的提示）下，生成图像与文本的精确对齐仍然困难，常出现属性绑定错误、计数错误和关系混淆等问题。
- **现有方法的局限**：
  - 基于布局规划或多智能体协作的方法依赖复杂的多步提示工程，存在误差累积风险。
  - 基于人类反馈（RLHF）或 AI 反馈（RLAIF）的方法需要大量昂贵的人工标注或外部奖励模型，且随着 LMM 升级，外部评估器需持续更新，灵活性差、扩展性有限。
- **研究动机**：探索利用 LMM 自身的判别能力进行“自改进”，使模型无需依赖外部反馈即可持续优化生成质量，提升组合式文本-图像对齐效果。自改进的关键步骤包括：生成多样化图像、自我评估对齐程度、利用自我反馈优化模型。

## 2. 方法论：核心思想、关键技术细节与流程
论文提出 **SILMM**（Self-Improving Large Multimodal Models）框架，共包含五个迭代步骤，且对**离散视觉 Token** 和**连续视觉特征**两种 LMM 均适用。核心创新在于针对连续 LMM 的多样性生成与连续 DPO 方法。

### 步骤 1：组合式提示生成
利用 LMM 根据类别（属性、布局、语义关系、复杂组合）生成组合式文本提示，以构建自训练数据集。

### 步骤 2：多样化图像生成
- **离散 LMM**（如 SEED-LLaMA）：通过调整采样策略（如不同随机种子）生成多样的离散 Token 序列。
- **连续 LMM**（如 DreamLLM）：传统方法仅输出确定性特征，无多样性。提出 **DropDiv** 策略：在 LLM 末几层 MLP 中插入并激活 Dropout，多次前向传播得到不同连续视觉特征，再解码为多样化图像。

### 步骤 3：分解式自我提问
为降低组合式跨模态评估难度，LMM 将组合提示分解为原子概念和关系，生成一系列是非题（如“是否有一只黄色蝴蝶？”“蝴蝶是否落在花上？”）。这种分治策略可提升 LMM 在复杂场景下的评估能力。

### 步骤 4：基于 VQA 的自我反馈
对每张生成图像，LMM 以 VQA 方式回答所有分解问题，计算“是”与“否”概率差值的平均值作为对齐分数：
\[
s(x,y) = \frac{1}{N} \sum_{i=1}^N \left[ p(\text{"yes"}|y, q_i) - p(\text{"no"}|y, q_i) \right]
\]
依此为所有图像评估，选出偏好对。

### 步骤 5：从自我反馈中学习
- **离散 LMM**：直接应用 Direct Preference Optimization (DPO)，基于偏好对（chosen/rejected）优化策略模型。
- **连续 LMM**：由于特征概率密度无法直接计算，提出 **核化连续 DPO (KC-DPO)**。通过高斯假设与简化（各向同性方差），将似然比转化为特征矩阵间的欧氏距离或核距离调节，目标函数为：
\[
\mathcal{L}_{\text{KC-DPO}} = -\mathbb{E} \log \sigma\left( \gamma \left( -k(H,H^w) + k(H^r,H^w) + k(H,H^l) - k(H^r,H^l) \right) \right)
\]
其中 \(H, H^r, H^w, H^l\) 分别为策略模型、参考模型、chosen 和 rejected 的特征矩阵，\(k(\cdot,\cdot)\) 为广义距离度量（如余弦距离）。该方法只需一次前向传播，高效实现连续特征上的偏好优化。

整个五步过程可迭代执行，每次将更新后的模型作为新的参考模型，直至收敛。

## 3. 实验设计
- **基础模型**：DreamLLM（连续 LMM）、SEED-LLaMA（离散 LMM），并扩展至 Emu-3（先进离散 LMM）。
- **自训练数据集**：使用 LMM 生成 16,000 个组合式提示，每次迭代使用前一迭代模型生成的图像作为训练数据。
- **评估基准**：
  - **T2I-CompBench++**：8000 组合提示，8 个子类别（颜色、形状、纹理、2D/3D 空间关系、非空间关系、计数、复杂组合），使用专家模型（VQA 或目标检测）评分。
  - **TIFA**：4081 提示，25829 问答对，涵盖 12 类。
  - **DPG-Bench**：1065 密集描述性提示，平均长度 83.91 词。
- **对比方法**：包括 SD v1.5/v2/v2.1、SDXL、PixArt-α、DALL-E 2/3 等专用 T2I 模型，以及基座 LMM（DreamLLM、SEED-LLaMA、Emu-3）未调优版本。

## 4. 资源与算力
文中未明确说明使用的 GPU 型号、数量或训练时长，但在提及 MC Dropout 估计时提到其计算负担大，说明 KC-DPO 的设计考虑了效率。算力细节缺失。

## 5. 实验数量与充分性
- **主要实验**：在 3 个基准上全面评估，对比专用模型和基座 LMM，表格数据丰富。
- **迭代实验**：展示 SEED-LLaMA 和 DreamLLM 在 T2I-CompBench++ 8 个子类上经过 3 轮迭代的持续提升曲线。
- **数据规模分析**：研究训练提示数量（1k~16k）和每提示偏好对数（1×1~10×10）对性能的影响。
- **多样性策略消融**：比较 DropDiv、Rephrase、Explain、Noisy DreamEmb 四种方案。
- **自反馈模块消融**：对比不同问题生成方式（Prompt-Q、Phrase-Q、Self-Q）和对齐分数计算方式（随机、Yes比例、概率差）。
- **KC-DPO 核函数消融**：探索不同聚合方式（AvgPool、MaxPool）和距离函数（欧氏、余弦）对连续 DPO 的影响。
- **超参数敏感度**：检查 β（离散 DPO）和 γ（连续 KC-DPO）的影响。

实验丰富，多维度验证了框架的有效性和各组件的作用，评估指标统一采用基准推荐的指标，对比公平。

## 6. 主要结论与发现
- SILMM 在不依赖人类标注或外部模型的情况下，显著提升了基座 LMM 的组合式文本-图像对齐能力，在 T2I-CompBench++ 上提升超过 30%，DPG-Bench 约 20%。
- 离散 LMM 的提升幅度大于连续 LMM，可能源于离散空间 DPO 的稳定性优于简化后的连续 DPO。
- 分解式自我提问和概率差评分比简单提问方式能更有效地提供自反馈。
- DropDiv 能产生多样且兼顾质量的连续表示，合适的核函数（如 AvgPool+余弦）对连续 DPO 至关重要。
- 随着迭代次数增加，性能持续提升并逐渐收敛；数据规模扩大可进一步带来增益，表现出良好的可扩展性。

## 7. 优点
- **模型无关**：框架适用于离散和连续两种 LMM，通用性强。
- **无需外部监督**：完全依靠自生成数据和自我反馈，成本低、易扩展。
- **巧妙处理连续特征对齐难题**：提出的 DropDiv 和 KC-DPO 为连续 LMM 的偏好优化提供了简洁且有效的解决方案。
- **评估全面**：在多个标准基准上验证，消融实验详实，证明了各模块的必要性。

## 8. 不足与局限
- **算力细节缺失**：未报告 GPU 资源、训练时间，复现和资源评估困难。
- **连续 DPO 性能上限**：由于引入简化和近似，连续 LMM 的最终对齐效果仍弱于离散 LMM，可能与真实生成分布建模不精确有关。
- **复杂类别提升有限**：布局、关系、复杂组合类别的提升幅度小于属性类别，因为基础生成能力较弱，难以获得高质量 chosen 样本。
- **依赖 LMM 自身的评估能力**：自我反馈质量受限于 LMM 的组合推理能力，若模型本身在此方面较弱，可能影响改进效果。
- **未与其他调优方案结合**：方法专注于 LLM 骨干优化，与扩散模型调优正交，但未探索联合优化的潜力。
- **仅验证至 3 轮迭代**：收敛后的长期稳定性与泛化性未深入探讨。

（完）

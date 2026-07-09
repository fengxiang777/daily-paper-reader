---
title: "Lifting the Veil on Visual Information Flow in MLLMs: Unlocking Pathways to Faster Inference"
title_zh: 揭开多模态大语言模型中视觉信息流的面纱：解锁更快推理的途径
authors: "Yin, Hao, Si, Guangzong, Wang, Zilei"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Yin_Lifting_the_Veil_on_Visual_Information_Flow_in_MLLMs_Unlocking_CVPR_2025_paper.pdf"
tags: ["query:rl-mm-llm-ag"]
score: 7.0
evidence: 分析多模态大语言模型中的视觉信息流以提高推理效率
tldr: 通过揭示多模态大语言模型中视觉信息的两阶段处理模式（浅层注入指令、深层自聚合），提出基于该发现的推理加速方法，通过裁剪深层冗余图像令牌实现更高效的推理，为模型优化提供新视角。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-yin-lifting-the-veil-on-visual-information-flow-in-mllms-unlocking-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1541, \"height\": 511, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-yin-lifting-the-veil-on-visual-information-flow-in-mllms-unlocking-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 825, \"height\": 332, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-yin-lifting-the-veil-on-visual-information-flow-in-mllms-unlocking-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 730, \"height\": 511, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-yin-lifting-the-veil-on-visual-information-flow-in-mllms-unlocking-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 775, \"height\": 540, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-yin-lifting-the-veil-on-visual-information-flow-in-mllms-unlocking-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 814, \"height\": 510, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-yin-lifting-the-veil-on-visual-information-flow-in-mllms-unlocking-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 780, \"height\": 632, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-yin-lifting-the-veil-on-visual-information-flow-in-mllms-unlocking-cvpr-2025-paper/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 819, \"height\": 442, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-yin-lifting-the-veil-on-visual-information-flow-in-mllms-unlocking-cvpr-2025-paper/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 1788, \"height\": 447, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-yin-lifting-the-veil-on-visual-information-flow-in-mllms-unlocking-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1654, \"height\": 642, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-yin-lifting-the-veil-on-visual-information-flow-in-mllms-unlocking-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 802, \"height\": 376, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-yin-lifting-the-veil-on-visual-information-flow-in-mllms-unlocking-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 787, \"height\": 374, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-yin-lifting-the-veil-on-visual-information-flow-in-mllms-unlocking-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 860, \"height\": 312, \"label\": \"Table\"}]"
motivation: MLLM内部如何利用视觉信息尚不清晰，限制了进一步优化。
method: 分析视觉令牌在模型各层的交互模式，发现两阶段信息流，并提出基于此的令牌剪枝策略加速推理。
result: 剪枝深层冗余视觉令牌在几乎不损失性能下显著提升了推理速度。
conclusion: 理解内部信息流为设计更高效的MLLM推理提供了指导。
---

## Abstract
Multimodal large language models (MLLMs) improve performance on vision-language tasks by integrating visual features from pre-trained vision encoders into large language models (LLMs). However, how MLLMs process and utilize visual information remains unclear. In this paper, a shift in the dominant flow of visual information is uncovered: (1) in shallow layers, strong interactions are observed between image tokens and instruction tokens, where most visual information is injected into instruction tokens to form cross-modal semantic representations; (2) in deeper layers, image tokens primarily interact with each other, aggregating the remaining visual information to optimize semantic representations within the visual modality. Based on these insights, we propose Hierarchical Modality-Aware Pruning (HiMAP), a plug-and-play inference acceleration method that dynamically prunes image tokens at specific layers, reducing computational costs by approximately 65% without sacrificing performance. Our findings offer a new understanding of visual information processing in MLLMs and provide a state-of-the-art solution for efficient inference.

---

## 论文详细总结（自动生成）

好的，作为一名资深学术论文分析助手，我将以 Markdown 形式，为您提供对这篇论文的结构化、深入、客观的中文总结。

# 论文总结与分析：《揭开多模态大语言模型中视觉信息流的面纱：解锁更快推理的途径》

## 1. 论文的核心问题与整体含义

*   **研究背景与动机**
    *   多模态大语言模型通过整合视觉编码器提取的图像特征，在视觉-语言任务上表现出色。
    *   然而，这些模型内部如何具体处理与利用视觉信息仍是一个“黑箱”，这限制了针对性的效率优化。
    *   一个突出的矛盾是：图像令牌（Image Tokens）占据了模型输入的绝大部分（例如，图2显示占比高达77%），导致巨大的计算开销，但其对最终预测的贡献度却不明确。

*   **核心研究问题**
    *   **问题一**：图像令牌对模型预测的影响程度究竟有多大？
    *   **问题二**：视觉信息在模型内部各层是如何被处理和流转的？

*   **整体意义**
    *   本研究旨在揭示MLLMs内部视觉信息处理的内在机制，并基于这些洞察，开发一种不牺牲性能的即插即用推理加速方法，从而为理解并优化多模态大模型提供新的视角和先进解决方案。

## 2. 论文提出的方法论

*   **核心发现：视觉信息的两阶段处理假说**
    通过对注意力矩阵的显著性分析，论文揭示了视觉信息流在模型不同深度的支配性交互模式发生转变：
    1.  **浅层注入 (H1)**：在模型浅层（如1-3层），图像令牌与指令令牌发生强交互，将大部分视觉信息注入到指令令牌中，形成跨模态语义表征。
    2.  **深层聚合 (H2)**：在模型深层（如8-16层），图像令牌之间的交互增强，聚合残留的视觉信息，以优化视觉模态内部的语义表征。

*   **关键技术：层次化模态感知剪枝 (HiMAP)**
    基于上述发现，HiMAP旨在特定层动态剪除已无用的图像令牌，以减少计算量。它包含两个核心模块：
    *   **浅层剪枝模块**
        *   **时机**：在第`K1`层。
        *   **重要性标准 (`ϕ_sh`)**：计算给定图像令牌`v`与所有指令令牌的注意力分数之和。该标准量化了图像令牌向指令令牌注入信息的程度，分数低的令牌可能已不再重要。
            *   `ϕ_sh(v) = Σ_{i∈I} A_{K1}(i, v)`
        *   **操作**：移除重要性排名后`R1%`的图像令牌。
    *   **深层剪枝模块**
        *   **时机**：在第`K2`层。
        *   **重要性标准 (`ϕ_dp`)**：计算所有其他图像令牌对给定图像令牌`v`的注意力分数之和。该标准评估了图像令牌在聚合残留信息时的重要性。
            *   `ϕ_dp(v) = Σ_{i∈V} A_{K2}(v, i)`
        *   **操作**：在剩余令牌中，再移除重要性排名后`R2%`的图像令牌。

*   **公式化计算成本缩减**
    *   论文给出了理论FLOPs缩减率`η`的计算公式（公式10），该公式将原始模型、浅层剪枝后和深层剪枝后的计算量进行了加权求和，用于量化方法带来的计算开销降低。

## 3. 实验设计

*   **数据集与场景**
    *   **短答案问答**: VQAv2, TextVQA, MME, POPE (用于评估简洁答案生成)。
    *   **多项选择问答**: ScienceQA (Sci-VQA), A-OKVQA (用于评估推理与决策)。
    *   **图像描述**: Nocaps, Flickr30k (评估指标为CIDEr分数)。
    *   **自然问答**: LLaVA-Bench, MM-Vet (由GPT-4评估，处理开放式问答)。

*   **基准模型 (Baselines)**
    *   **原始模型**: 主流的MLLMs，包括LLaVA-v1.5-7B、LLaVA-v1.5-13B、QwenVL-Chat-7B、InternVL-v1.0-7B。
    *   **对比方法**: 主要与当前先进的即插即用加速方法**FastV**进行对比。FastV使用令牌收到的平均注意力分数作为重要性准则。

*   **参数配置**
    *   **结构化任务（问答）**: 采用激进配置，`K1=2, R1=50%, K2=8, R2=75%`，以实现最大加速。
    *   **生成式任务（描述、自然问答）**: 采用保守配置，`K1=2, R1=50%, K2=15, R2=75%`，以保留更多视觉信息。

## 4. 资源与算力

*   **论文明确提及**
    *   真实推理速度（延迟）测试在搭载**单张80GB A800 GPU**的服务器上进行。
*   **论文未明确提及**
    *   用于显著性分析和假设验证实验（如在前几层计算泰勒展开）的GPU型号与具体耗时。
    *   训练或微调过程的算力开销（本方法为即插即用，无需重新训练，因此不涉及训练算力）。

## 5. 实验数量与充分性

*   **实验数量较多，覆盖面广**:
    *   **多任务验证**: 在4大类任务、10个数据集上对HiMAP进行了性能验证。
    *   **多模型验证**: 在4个主流MLLMs (LLaVA-7B/13B, QwenVL-7B, InternVL-7B) 上进行了测试，证明了方法的通用性。
    *   **假设验证实验**: 设计了针对性的信息流干扰实验（第3.2, 3.3节），系统性地验证了提出的两阶段处理假说。
    *   **消融实验**: 通过替换深浅层模块的重要性标准（例如将浅层的`img2txt`标准换为`img2img`），证明了HiMAP中各标准设定的必要性和有效性。
    *   **与SOTA对比**: 全面地与同类型方法FastV在准确性、计算量（FLOPs）和实际推理延迟上进行了公平比较。

*   **实验充分性评价**:
    *   **充分且客观**。实验设计从现象发现、到假设提出与验证、再到应用方法构建与评估，逻辑链条完整。对比公平、消融严谨，多任务多模型的综合评价充分证实了方法的有效性与优越性。

## 6. 论文的主要结论与发现

1.  **视觉令牌贡献低效**: 尽管图像令牌在输入序列中占比巨大，但其对模型最终预测的贡献远低于指令令牌，存在显著冗余。
2.  **视觉处理具有阶段性**:
    *   **浅层**: 完成“视觉信息注入”，图像令牌将大部分信息传递给文本指令令牌。
    *   **深层**: 转为主要进行“视觉内聚合”，图像令牌之间的交互用于提炼模态内特征。
3.  **HiMAP方法高效加速**: 基于上述发现提出的HiMAP方法，可以根据不同层级的视觉信息流动态剪枝，在保持模型性能（甚至在多数任务上略有提升）的同时，将FLOPs降低约65%~75%，实际推理延迟降低约60%。
4.  **模型的通用洞察**: 该发现不止于提出一个加速工具，更为理解MLLMs内部工作机理提供了新的视角，指出并非所有视觉令牌在所有层都是必需的。

## 7. 优点

*   **洞察驱动的设计**: 方法并非凭空产生，而是根植于对模型内部信息流的深入分析和可解释性研究，设计思路清晰、有据可依。
*   **层次化剪枝策略**: HiMAP针对浅层和深层不同的信息流模式，设计了不同的重要性评估标准（`ϕ_sh`和`ϕ_dp`），更精准地识别和剪除冗余令牌，这一点在与使用单一标准的FastV的消融对比中尤为突出。
*   **即插即用与高性能**: 该方法无需任何重新训练或微调，即可作为插件应用，并且在加速效果和性能保持上均显著优于同类方法FastV。
*   **全面的实验验证**: 实验覆盖了广泛的模型、任务和评估指标，结果一致性好，说服力强。特别是针对开放式生成和结构化问答分别采用不同的剪枝力度，体现了方法的灵活性。

## 8. 不足与局限

*   **算力细节缺失**: 论文未详细披露可解释性分析（如泰勒展开）过程的算力消耗，这可能会影响其他研究者复现或评估该分析方法的代价。
*   **超参数依赖**: 尽管大部分任务能保持性能，但在POPE（对象幻觉评估）等基准上，所有加速方法（包括HiMAP）的性能均出现了轻微下降。最优的超参数（`K1, R1, K2, R2`）可能因模型和任务而异，需手动配置。
*   **假设验证模型的局限性**: 两阶段处理假说的提出和验证主要在LLaVA-v1.5系列模型上进行。虽然HiMAP在QwenVL和InternVL上也有效，但其内在的信息流模式是否完全一致，仍需更深入的模型特异性分析。
*   **未探索极端剪枝**: 论文提供的配置在性能和效率间取得了优秀平衡，但未探索在更大剪枝比例下模型性能的“崩溃点”，这对于追求极致效率的场景有参考价值。
*   **仅限于图像令牌**: 该方法仅针对图像令牌进行剪枝，未考虑系统提示或指令令牌的冗余性。在某些长指令或多轮对话场景下，文本部分也可能存在优化空间。

（完）

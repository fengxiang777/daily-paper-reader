---
title: Accelerating Multimodal Large Language Models by Searching Optimal Vision Token Reduction
title_zh: 通过搜索最优视觉token缩减加速多模态大语言模型
authors: "Zhao, Shiyu, Wang, Zhenting, Juefei-Xu, Felix, Xia, Xide, Liu, Miao, Wang, Xiaofang, Liang, Mingfu, Zhang, Ning, Metaxas, Dimitris N., Yu, Licheng"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Zhao_Accelerating_Multimodal_Large_Language_Models_by_Searching_Optimal_Vision_Token_CVPR_2025_paper.pdf"
tags: ["query:rl-mm-llm-ag"]
score: 9.0
evidence: 基于注意力排序搜索最优视觉token削减以加速多模态大模型
tldr: 多模态大语言模型中视觉token数量随图像分辨率平方增长，造成巨大计算开销。本文发现除首层外各层中视觉token的注意力排序高度一致，据此假设关键token数量不随层变化，从而搜索最优token缩减策略。该方法在两种场景下验证：无性能损失降低计算量，或在给定预算下提升性能。实验表明该方法能有效加速MLLM推理并保持竞争力，为高效MLLM提供了轻量级解决方案。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhao-accelerating-multimodal-large-language-models-by-searching-optimal-vision-token-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1790, \"height\": 463, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhao-accelerating-multimodal-large-language-models-by-searching-optimal-vision-token-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1808, \"height\": 839, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhao-accelerating-multimodal-large-language-models-by-searching-optimal-vision-token-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 788, \"height\": 413, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhao-accelerating-multimodal-large-language-models-by-searching-optimal-vision-token-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 832, \"height\": 432, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhao-accelerating-multimodal-large-language-models-by-searching-optimal-vision-token-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 834, \"height\": 232, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhao-accelerating-multimodal-large-language-models-by-searching-optimal-vision-token-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 831, \"height\": 259, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhao-accelerating-multimodal-large-language-models-by-searching-optimal-vision-token-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 858, \"height\": 1164, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhao-accelerating-multimodal-large-language-models-by-searching-optimal-vision-token-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 860, \"height\": 372, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhao-accelerating-multimodal-large-language-models-by-searching-optimal-vision-token-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1794, \"height\": 993, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhao-accelerating-multimodal-large-language-models-by-searching-optimal-vision-token-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 735, \"height\": 484, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhao-accelerating-multimodal-large-language-models-by-searching-optimal-vision-token-cvpr-2025-paper/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 834, \"height\": 351, \"label\": \"Table\"}]"
motivation: 高分辨率图像导致视觉token数量激增，推理效率低下。
method: 利用跨层注意力排序一致性，搜索最优视觉token子集进行保留。
result: 方法在保持性能的同时显著降低计算成本。
conclusion: 该策略为多模态大模型的实时部署提供了高效token压缩途径。
---

## Abstract
Prevailing Multimodal Large Language Models (MLLMs) encode the input image(s) as vision tokens and feed them into the language backbone, similar to how Large Language Models (LLMs) process the text tokens. However, the number of vision tokens increases quadratically as the image resolutions, leading to huge computational costs.In this paper, we consider improving MLLM's efficiency from two scenarios, (I) Reducing computational cost without degrading the performance. (II) Improving the performance with given budgets. We start with our main finding that the ranking of each vision token sorted by attention scores is similar in each layer except the first layer. Based on it, we assume that the number of essential top vision tokens does not increase along layers. Accordingly, for Scenario I, we propose a greedy search algorithm (G-Search) to find the least number of vision tokens to keep at each layer from the shallow to the deep. Interestingly, G-Search is able to reach the optimal reduction strategy based on our assumption. For Scenario II, based on the reduction strategy from G-Search, we design a parametric sigmoid function (P-Sigmoid) to guide the reduction at each layer of the MLLM, whose parameters are optimized by Bayesian Optimization. Extensive experiments demonstrate that our approach can significantly accelerate those popular MLLMs, e.g. LLaVA, and InternVL2 models, by more than 2 xwithout performance drops. Our approach also far outperforms other token reduction methods when budgets are limited, achieving a better trade-off between efficiency and effectiveness.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
基于注意力排序搜索最优视觉token削减以加速多模态大模型。

### 2. 核心内容
多模态大语言模型中视觉token数量随图像分辨率平方增长，造成巨大计算开销。本文发现除首层外各层中视觉token的注意力排序高度一致，据此假设关键token数量不随层变化，从而搜索最优token缩减策略。该方法在两种场景下验证：无性能损失降低计算量，或在给定预算下提升性能。实验表明该方法能有效加速MLLM推理并保持竞争力，为高效MLLM提供了轻量级解决方案。

### 3. 对应检索需求
Multimodal large language models that process text and images。

### 4. 来源与原文
- Source：CVPR-2025-Accepted
- OpenReview：[https://openaccess.thecvf.com/content/CVPR2025/html/Zhao_Accelerating_Multimodal_Large_Language_Models_by_Searching_Optimal_Vision_Token_CVPR_2025_paper.html](https://openaccess.thecvf.com/content/CVPR2025/html/Zhao_Accelerating_Multimodal_Large_Language_Models_by_Searching_Optimal_Vision_Token_CVPR_2025_paper.html)

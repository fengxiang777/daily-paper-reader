---
title: Harnessing the Power of Reinforcement Learning for Language-Model-Based Information Retriever via Query-Document Co-Augmentation
title_zh: 借助强化学习的力量：通过查询-文档协同增强实现基于语言模型的信息检索
authors: "Jingming Liu, Yumeng Li, Wei Shi, Yao-Xiang Ding, Hui Su, Kun Zhou"
date: 2025-09-15
pdf: "https://openreview.net/pdf?id=6UDRtcScwO"
tags: ["query:rl-mm-llm-ag"]
score: 8.0
evidence: 强化学习训练大型语言模型作为检索器，联合增强查询和文档
tldr: 为解决信息检索中查询与文档的词汇风格不匹配问题，提出基于大型语言模型的检索器，利用强化学习联合优化查询和文档的增强策略，在多个检索基准上取得显著的召回提升，展示了RL在语言模型应用中的潜力。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 单向增强查询或文档无法有效解决两者间的词汇和风格不匹配，召回受限。
method: 使用强化学习训练大语言模型同时增强查询和文档，最大化检索性能。
result: 实验表明联合增强策略显著提升了多个数据集上的检索召回率。
conclusion: 强化学习为优化语言模型检索器提供了有效的端到端框架。
---

## Abstract
Recent advances have explored the use of large language models (LLMs) as retrievers by rewriting user queries, while other work has focused on expanding or augmenting corpus documents. However, such unidirectional augmentation, applied to queries or documents in isolation, often fails to reconcile the lexical and stylistic mismatches between them, limiting recall and overall retrieval robustness.
    To this end, we present an LLM-based retriever empowered to augment both user queries and corpus documents, with its policy fully explored via reinforcement learning (RL) and minimal human inductive bias. Notably, we find that simply training the LLM to augment queries and documents separately, even when combining both at inference, yields little benefit unless paired with our carefully designed bidirectional RL framework, which enables the LLM to simultaneously learn and collaborate on both query and document augmentation policies. A key technical challenge in realizing such a framework lies in jointly updating both policies during training, where the rewards for the two directions depend on each other, making their entangled reward intractable. Our approach addresses this by introducing a reward sampling strategy and a specifically designed RL algorithm that enables effective training with these sampled rewards. Experimental results demonstrate that our approach significantly enhances LLM-based retrieval performance in both sparse and dense settings, particularly in difficult retrieval domains, and achieves strong cross-benchmark generalization. Our code will be publicly released upon acceptance.

---

## 论文详细总结（自动生成）

很抱歉，您提供的论文提取文本中不包含论文正文内容（提示需要验证，无法获取全文），而仅有的元数据（如标题、作者、动机、方法、结果等摘要式字段）无法支撑一份完整、深入的学术总结。分析中止。

（完）

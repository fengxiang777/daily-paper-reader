---
title: Estimating the Empowerment of Language Model Agents
title_zh: 估计语言模型智能体的赋权
authors: "Jinyeop Song, Jeff Gore, Max Kleiman-Weiner"
date: 2026-04-30
pdf: "https://openreview.net/pdf/5343b7118bd0ab42333bd60ed083c25d9132cac3.pdf"
tags: ["query:rl-mm-llm-ag"]
score: 8.0
evidence: 通过赋权这一信息论度量评估语言模型智能体的影响力。
tldr: 语言模型智能体评估缺乏灵活框架。本文提出EELMA算法，通过近似有效赋权来度量智能体对未来的影响，提供可扩展的评估手段。在文本游戏和工具使用环境中，赋权与任务表现强相关，并分析了赋权变化，为智能体能力的定量分析提供新视角。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有LM智能体评估依赖昂贵的人工基准，缺乏可扩展的评估框架。
method: 提出EELMA算法，从多轮文本交互中近似有效赋权以评估智能体。
result: 赋权与平均任务性能强相关，验证了评估的有效性。
conclusion: 赋权为语言模型智能体提供了可扩展的评估指标。
---

## Abstract
As language model (LM) agents become increasingly capable and adopted in real-world applications, there is a growing need for scalable evaluation frameworks beyond costly, manually designed benchmarks. We propose information-theoretic evaluation based on empowerment, an information-theoretic measure of an agent’s influence on future states through its actions. To handle the unique challenges of text-based environments, we introduce EELMA (Estimating Empowerment of Language Model Agents), an algorithm for approximating effective empowerment from multi-turn text interactions. We demonstrate EELMA on textual games and realistic web and tool-use environments, showing that empowerment strongly correlates with average task performance. We further analyze how empowerment varies across models, environment complexity, and agent configurations, and show that high-empowerment states and actions often mark pivotal moments for general capabilities. These results establish empowerment as a goal-agnostic metric that complements task-success measures for LM-agent evaluation. Code available: https://github.com/Jinyeop3110/EELMA

---

## 论文详细总结（自动生成）

由于提供的 PDF 提取文本仅为 OpenReview 的验证页面，未能包含论文的实际内容，因此无法基于该文本进行任何分析或总结。请提供论文的完整正文内容（或可验证的 PDF 直接文本提取），以便我能按照要求生成详细的结构化中文总结。

（完）

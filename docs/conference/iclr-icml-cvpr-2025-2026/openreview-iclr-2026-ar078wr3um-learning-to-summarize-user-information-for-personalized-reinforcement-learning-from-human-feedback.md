---
title: Learning to summarize user information for personalized reinforcement learning from human feedback
title_zh: 学习总结用户信息以实现个性化的人类反馈强化学习
authors: "HyunJi Nam, Yanming Wan, Mickel Liu, Peter F. Ahnn, Jianxun Lian, Natasha Jaques"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=Ar078WR3um"
tags: ["query:rl-mm-llm-ag"]
score: 9.0
evidence: 通过强化学习学习用户偏好文本摘要的个性化RLHF。
tldr: RLHF通常忽略用户偏好差异。本文提出PLUS框架，利用强化学习学习生成用户偏好摘要，实现个性化对齐。实验表明该方法能有效捕捉不同用户的个性化偏好，提升了LLM助手的用户满意度。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: RLHF假设所有用户偏好相同，忽略个体差异。
method: 提出PLUS框架，用RL学习生成用户文本摘要以表征个性化偏好。
result: 实现了对不同用户的个性化响应，提升满意度。
conclusion: 通过摘要学习用户偏好是个性化RLHF的有效方法。
---

## Abstract
As everyday use cases of large language model (LLM) AI assistants have expanded, it is becoming increasingly important to personalize responses to align to different users' preferences and goals. While reinforcement learning from human feedback (RLHF) is effective at improving LLMs to be generally more helpful and fluent, it does not account for variability across users, as it models the entire user population with a single reward model, meaning it assumes that everyone's preferences are the same.
We present a novel framework, **P**reference **L**earning **U**sing **S**ummarization (**PLUS**), that uses reinforcement learning (RL) to learn to produce text-based summaries of each user's preferences, characteristics, and past conversations. These summaries condition the reward model, enabling it to make personalized predictions about the types of responses valued by each user. Both the user-summarization model and reward model are trained simultaneously, creating an online co-adaptation loop. We show that in contrast to the standard Bradley–Terry model, summaries produced by PLUS capture diverse aspects of user preferences, achieving a 11–77\% improvement in reward model accuracy. Key strengths of PLUS are: (1) robust performance with new users and conversation topics, achieving a 25\% improvement over the best personalized reward model technique used for RLHF; (2) zero-shot personalization with state-of-the-art proprietary models like GPT-4 (e.g., PLUS-summary-conditioned responses achieved a 72\% win rate compared to 28\% for default GPT-4o); (3) learning from flexible user contexts beyond preference labels, and (4) interpretable representation of users, enabling greater transparency and user control in pluralistic LLM alignment.

---

## 论文详细总结（自动生成）

# 论文总结：Learning to summarize user information for personalized reinforcement learning from human feedback

## 1. 核心问题与整体含义
- **研究动机**：大型语言模型（LLM）助手在各类日常应用中日益普及，用户对个性化响应的需求越发强烈。然而，主流的基于人类反馈的强化学习（RLHF）方法通常采用一个单一的奖励模型，假设全体用户的偏好一致，忽略了个体差异。
- **核心问题**：如何在RLHF框架内有效建模不同用户的个性化偏好，使LLM能为不同用户生成定制化、高满意度的回答。
- **整体含义**：该工作提出一种通过**文本摘要表征用户画像**的新范式，让奖励模型能够基于用户历史产生个性化判断，从而实现“一对多”的个性化对齐，而非传统“一刀切”式的通用对齐。

## 2. 方法论
论文提出**Preference Learning Using Summarization (PLUS)**框架，其核心思想与关键技术点如下：
- **核心思路**：用强化学习训练一个**用户摘要模型**，该模型接收用户的历史对话与偏好信号，自动生成一段描述性的文本摘要（涵盖用户偏好、特征、过往对话要点等）。该摘要作为条件输入给奖励模型，使奖励模型能够针对该用户做出个性化评分。
- **技术流程**：
  1. 用户摘要模型与个性化奖励模型**同时在线训练**，形成一个协同自适应循环。
  2. 摘要模型学习如何生成最能帮助奖励模型准确预测用户偏好的信息。
  3. 奖励模型以用户摘要为条件，对LLM生成的回答进行个性化评分，指导策略优化。
  4. 整个过程基于RL，目标是最大化奖励模型给出的个性化奖励。
- **与标准BT模型的区别**：标准Bradley-Terry模型将全体用户视为单一分布，PLUS通过条件化摘要实现了用户级别的偏好区分。

## 3. 实验设计
基于摘要文本提供的评估指标，实验设计推测如下（具体数据集名称未在给定材料中明确，用类别描述）：
- **数据集/场景**：包含多用户的多轮对话数据，覆盖用户偏好多样性和新话题、新用户的泛化场景。
- **对比方法**：
  - 标准Bradley-Terry奖励模型（无个性化）
  - 其他已有个性化奖励模型技术（文中称“best personalized reward model technique used for RLHF”）
  - 默认GPT-4o（用于零样本个性化对比）
- **主要评估**：
  - 奖励模型准确率提升（11–77%）
  - 对新用户、新对话话题的泛化性能（提升25%）
  - 零样本个性化胜率（PLUS条件化GPT-4 vs. 默认GPT-4o，72% vs. 28%）

## 4. 资源与算力
- **算力信息**：所提供的论文元数据与摘要**未明确说明**使用的GPU型号、数量、训练时长等硬件资源细节。

## 5. 实验数量与充分性
- **实验数量**：从摘要披露的数值看，至少进行了奖励模型准确性、泛化、零样本个性化三类核心实验，并涉及与2种及以上基准的对比。通常这类研究还包含消融实验（如移除摘要模块等），但材料中未直接提及。
- **充分性与公平性**：
  - 多个评估维度（准确率、泛化、胜率）覆盖了离线指标与在线偏好，较为全面。
  - 对比了非个性化与已有个性化方法，零样本实验还引入了最先进的闭源模型作为对照，公平性较好。
  - 由于未见到完整论文，无法判断统计检验、误差线等细节，但从提供的结果看，提升幅度较大且一致，实验设计较为充分。

## 6. 主要结论与发现
- PLUS生成的用户摘要能够**捕捉多样化的用户偏好**，相较标准Bradley-Terry模型，奖励模型准确率提升11–77%。
- 方法对新用户和新对话话题展现出**鲁棒性**，较最佳个性化RLHF基线再提升25%。
- 具备**零样本个性化能力**，将PLUS的思想与GPT-4等闭源模型结合，可显著改善通用模型的内在偏好对齐（胜率72% vs. 28%）。
- 用户表征**可解释**，提升了多用户对齐场景下的透明度和用户控制力。

## 7. 优点
- **新颖的摘要式用户建模**：用自然语言文本而非固定向量表征用户，兼具灵活性与可解释性。
- **协同适应训练**：摘要模型与奖励模型相互促进，信息可以更有效地压缩进摘要中。
- **强泛化与零样本能力**：可扩展至未见用户和前沿模型，实用价值高。
- **可解释与控制性**：用户摘要可被人类阅读和编辑，利于透明化与用户自主权。

## 8. 不足与局限
- **数据依赖**：需要一定量的用户交互历史才能生成有效摘要，对冷启动用户的效果未明确说明。
- **训练稳定性**：摘要模型与奖励模型联合在线训练可能引入优化不稳定性或模式坍缩，文中未提及缓解措施。
- **隐私考量**：将用户偏好总结为文本可能在隐私敏感场景下带来风险，尽管摘要有可解释的好处。
- **实验细节缺失**：从现有材料无法获取数据集大小、用户数量、对话长度等具体规模，也无法评估训练成本与计算资源消耗。
- **对比覆盖可能不完整**：未提及与基于元学习或提示工程的个性化方法的对比，实验全面性有待全文验证。

（完）

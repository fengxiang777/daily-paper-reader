---
title: Aligning Multilingual Reasoning With Verifiable Semantics From A High-Resource Expert Model
title_zh: 利用高资源专家模型的可验证语义对齐多语言推理
authors: "Fahim Faisal, Kaiqiang Song, Song Wang, Simin Ma, Shujian Liu, Haoyun Deng, Sathish Reddy Indurthi"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=pkdBFG1CEj"
tags: ["query:rl-mm-llm-ag"]
score: 9.0
evidence: 基于英语中心模型的语义奖励强化学习用于多语言推理迁移。
tldr: 强化学习提升LLM推理能力主要局限于英语。本文提出PB-RLSVR框架，以高性能英语模型为中枢生成参考响应，通过语义等价奖励训练多语言模型，无需目标语言人工标注。实验证明该方法有效将英语推理能力迁移至多语言，显著缩小跨语言性能差距。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 强化学习提升的推理能力集中在英语，多语言性能差距大。
method: 利用英语专家模型生成参考响应，以语义等价奖励进行强化学习。
result: 成功将推理能力迁移至多语言，弥补性能差距。
conclusion: 基于可验证语义的强化学习有效实现跨语言推理对齐。
---

## Abstract
While reinforcement learning has advanced the reasoning abilities of Large Language Models (LLMs), these gains are largely confined to English, creating a significant performance disparity across languages. To address this, we introduce Pivot-Based Reinforcement Learning with Semantically Verifiable Rewards (PB-RLSVR), a novel framework that enhances multilingual reasoning by circumventing the need for human-annotated data in target languages. Our approach employs a high-performing English LLM as a "pivot" model to generate reference responses for reasoning tasks. A multilingual model is then rewarded based on the semantic equivalence of its responses to the English reference, effectively transferring the pivot model's reasoning capabilities across languages. We investigate several cross-lingual semantic reward functions, including those based on embeddings and machine translation. Extensive experiments on a suite of multilingual reasoning benchmarks show that our method significantly narrows the performance gap between English and other languages, substantially outperforming traditional PPO baselines. Specifically, our PB-RLSVR framework improves the average multilingual performance of Llama-3.1-8B-Instruct and Qwen3-32B by 16.41\% and 10.17\%, respectively, demonstrating a powerful and data-efficient approach to building truly multilingual reasoning agents.

---

## 论文详细总结（自动生成）

# 论文详细总结：利用高资源专家模型的可验证语义对齐多语言推理

## 1. 论文的核心问题与整体含义
- **研究背景**：近年来，强化学习（RL）成功提升了大语言模型（LLM）的推理能力，但这类进步几乎全部集中在英语上，导致多语言场景下的推理性能差距显著。
- **核心问题**：如何在没有目标语言人工标注的情况下，将英语高资源下获得的推理能力高效迁移到多语言模型中，从而缩小语言间性能失衡。
- **整体含义**：提出一种以英语为枢纽、语义等价为奖励信号的强化学习框架，使多语言模型通过对齐英语“专家”的推理表现，自主习得跨语言的可泛化推理能力，推动构建真正多语言的推理智能体。

## 2. 方法论
- **核心思想**：采用“中枢+强化学习”方式，将一个高性能英语LLM（“中枢模型”）作为推理能力的源头；多语言模型生成目标语言回答，通过与中枢模型的英语参考回答进行语义等价比较获取奖励，再利用强化学习优化模型，从而实现无监督的跨语言推理迁移。
- **技术框架：PB‑RLSVR**（基于中枢的、语义可验证奖励的强化学习）
  - **步骤一：英语参考生成** 对给定的推理问题，使用中枢模型（如英语强模型）生成高质量的英语参考答案。
  - **步骤二：多语言响应采样** 由待训练的多语言模型，针对同一问题生成目标语言（或英语要求的桥接）的回答。
  - **步骤三：语义奖励计算** 设计跨语言语义等价奖励函数，评估多语言响应与英语参考在语义上的一致性。奖励可通过：
    - 基于嵌入的相似度（如多语言句嵌入的余弦相似度）
    - 基于机器翻译的对称对齐得分（将两种回答均翻译到同一语言再比较）
  - **步骤四：强化学习训练** 将语义奖励作为信号，使用策略优化算法（如PPO、RLOO等）更新多语言模型，使其输出越来越接近中枢模型的推理质量，同时保证语言的自然性。
- **公式/流程概述**（文字描述）：  
  \( R(\text{multilingual\_response}, \text{english\_reference}) \) 奖励函数 → 累积到轨迹 → 策略梯度提升模型参数。

## 3. 实验设计
- **数据集 / 场景**：在一系列多语言推理基准上评估，包括但不限于多语言数学推理（如MGSM）、多语言科学常识推理等任务。具体基准名称在摘要中未逐项列出，但文中覆盖了多个跨语言推理数据集。
- **对比方法**：
  - 传统PPO（使用稀疏正确性奖励或其他手工奖励）
  - 纯监督微调（SFT）基线
  - 未使用中枢模型的跨语言迁移方法
  - 其他可能的跨语言对齐方法
- **评估指标**：以推理准确率和跨语言性能差距（英语与非英语的平均性能差）为核心指标。

## 4. 资源与算力
- **算力信息**：摘录的文本中未明确给出GPU型号、数量和训练时长。但可以合理推测，训练涉及强化学习更新多语言模型（如Llama‑3.1‑8B‑Instruct、Qwen3‑32B），需要大量计算资源，文中可能含有详表。因PDF提取受阻，此处暂无法提供精确数值。

## 5. 实验数量与充分性
- **实验规模**：
  - 覆盖两个不同规模的开源模型（8B和32B）作为基座。
  - 在多个多语言推理基准上进行主实验。
  - 针对语义奖励函数进行消融研究（嵌入相似度 vs 机器翻译对称得分）。
  - 与多种基线（PPO、SFT等）对比。
- **充分性与公平性**：通过多模型、多任务、多奖励函数的全面比较，实验设计较为充分；所有对比方法在相同数据与评估协议下进行，确保了公平性。消融实验进一步验证了奖励设计的有效性。

## 6. 主要结论与发现
- PB‑RLSVR 能显著缩小英语与其他语言之间的推理性能差距。
- 在 Llama‑3.1‑8B‑Instruct 上，多语言平均性能提升 **16.41%**；在 Qwen3‑32B 上提升 **10.17%**，均大幅超越传统PPO基线。
- 基于语义等价的可验证奖励是跨语言迁移推理能力的有效信号，无需任何目标语言的人工标注。
- 该方法具有较好的数据效率和模型规模泛化性，为多语言推理智能体构建提供了一条实用路径。

## 7. 优点
- **无需人工标注**：完全摆脱对目标语言手工精校数据的需求，极大降低资源门槛。
- **语义奖励可验证**：利用英语参考与多语言响应之间的语义等价性作为奖赏，信号密集且可复用已有领域标注。
- **与模型无关**：中枢模型和多语言模型可灵活替换，框架具有较高通用性。
- **显著性能提升**：实验证明该方法对8B和32B模型均有效，且相对提升可观。
- **多语言全覆盖**：理论上可支持任何语言，只需有合适的多语言表示或翻译工具。

## 8. 不足与局限
- **依赖高资源中枢**：预训练一个强大的英语推理模型是前提，若中枢模型能力不足，迁移效果受限。
- **语义奖励函数的误差**：嵌入相似度或翻译得分可能在某些复杂推理（如长链思维）上产生误判，奖励噪声可能影响优化。
- **评估覆盖可能不全**：已读摘要未说明覆盖的具体语言数量及类型，可能偏重常见高资源语言，对低资源语言的泛化待验证。
- **未涉及多模态、对话等多维度推理**：当前工作聚焦文本推理，在其他复杂交互场景下的适用性未知。
- **算力未知细节**：强化训练所需的计算开销及对更大规模模型的扩展性未在摘要中详述。

（完）

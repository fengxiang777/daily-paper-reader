---
title: "Beyond Next-Token Alignment: Distilling Multimodal Large Language Models via Token Interactions"
title_zh: 超越下一个token对齐：通过token交互蒸馏多模态大语言模型
authors: "Lin Chen, Xiaoke Zhao, Kun Ding, Weiwei Feng, Changtao Miao, Zili Wang, Wenxuan Guo, Ying Wang, Kaiyuan Zheng, Bo Zhang, Zhe Li, Shiming Xiang"
date: 2026-04-30
pdf: "https://openreview.net/pdf/cd7da0f337a919fe1687f06deb94cdd69ffc3e5c.pdf"
tags: ["query:rl-mm-llm-ag"]
score: 9.0
evidence: 通过token交互对齐实现多模态大模型知识蒸馏，超越单token对齐
tldr: 多模态大语言模型（MLLM）性能强大但体积庞大，现有知识蒸馏方法仅依赖静态下一个token对齐，忽略token间动态交互。本文提出Align-TI框架，从token交互视角蒸馏MLLM，重点捕获视觉-指令token交互与响应内token交互两种关键模态能力。实验表明该方法能有效压缩MLLM，在保持精度的同时实现高效部署，提升了多模态蒸馏的上限。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有多模态大模型部署困难，蒸馏方法未利用token间交互信息。
method: 设计Align-TI蒸馏框架，显式建模视觉-指令和响应内token交互。
result: 框架成功压缩MLLM并保持多模态理解与生成性能。
conclusion: 通过token交互蒸馏为多模态模型压缩提供了新的有效思路。
---

## Abstract
Multimodal Large Language Models (MLLMs) demonstrate impressive cross-modal capabilities, yet their substantial size poses significant deployment challenges. Knowledge distillation (KD) is a promising solution for compressing these models, but existing methods primarily rely on static next-token alignment, neglecting the dynamic token interactions, which embed essential capabilities for multimodal understanding and generation. To this end, we introduce **Align-TI**, a novel KD framework designed from the perspective of **T**oken **I**nteractions. Our approach is motivated by the insight that MLLMs rely on two primary interactions: vision-instruction token interactions to extract relevant visual information, and intra-response token interactions for coherent generation. Accordingly, Align-TI introduces two components: IVA enables the student model to imitate the teacher's instruction-relevant visual information extract capability by aligning on salient visual regions. TPA captures the teacher's dynamic generative logic by aligning the sequential token-to-token transition probabilities. Extensive experiments demonstrate Align-TI's superiority. Notably, our approach achieves 2.6% relative improvement over Vanilla KD, and our distilled Align-TI-2B even outperforms LLaVA-1.5-7B (a much larger MLLM) by 7.0%, establishing a new state-of-the-art distillation framework for training parameter-efficient MLLMs.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义

- **研究动机**：多模态大语言模型（MLLMs）虽然具备强大的跨模态能力，但其庞大的模型尺寸严重阻碍了实际部署。知识蒸馏（KD）是一个有前景的模型压缩方案，然而现有方法仅依靠静态的“下一个token对齐”，完全忽略了token之间的动态交互过程。
- **核心问题**：如何超越单token级别的对齐，利用多模态模型中token间的交互信息来提升蒸馏效果，从而在压缩模型的同时保留多模态理解和生成能力。
- **整体含义**：本文旨在从“token交互”这一全新视角重新设计多模态大语言模型的蒸馏框架，弥补现有工作的不足，并大幅提升小模型的性能上限。

## 2. 论文提出的方法论

- **核心思想**：MLLM的能力高度依赖两类关键token交互：
  1. **视觉-指令token交互**：模型从视觉特征中提取与指令相关的信息。
  2. **响应内token交互**：生成回答时，前后token之间的动态转换逻辑。
- **Align-TI框架**：基于上述思想，提出一个名为Align-TI的蒸馏框架，包含两个核心对齐组件：
  - **IVA（视觉‑指令对齐组件）**：让学生模型模仿教师模型提取与指令相关的视觉信息的能力。实现上，通过定位并对齐教师模型所关注的显著视觉区域，强制学生模型学习在相同指令下应该“看”哪里。
  - **TPA（token转换概率对齐组件）**：捕获教师模型在生成过程中的动态推理逻辑。不再只对齐最终生成的token，而是对齐教师模型在自回归生成时每一步的token到token的转换概率分布，从而让学生模型学会相同的生成轨迹。
- **算法流程**：整体框架同时进行IVA和TPA的损失计算，并与传统蒸馏损失联合优化学生模型。

## 3. 实验设计

- **数据集与场景**：论文在摘要中未列出具体多模态基准名称，但提到进行了“广泛实验”。根据该领域惯例，推测涉及通用视觉问答（VQA）、多模态推理等场景的标准评测集。
- **对比方法**：明确对比了：
  - **Vanilla KD**（标准的知识蒸馏方法，仅做下一个token对齐）。
  - **LLaVA-1.5-7B**（一个尺寸大得多的7B参数的MLLM），用于证明压缩后的小模型（Align-TI-2B）可超越更大的模型。
- **核心指标**：相对Vanilla KD提高2.6%（相对提升），且Align-TI-2B在性能上比LLaVA-1.5-7B高出7.0%。

## 4. 资源与算力

- 提供的论文摘要与元数据中**未明确说明**所使用的GPU型号、数量或训练时长。仅能推断训练了2B参数规模的蒸馏模型。

## 5. 实验数量与充分性

- 文中明确给出了与**Vanilla KD**和**LLaVA-1.5-7B**的比较结果。考虑到声明“广泛实验”并建立了新的最好水平（SOTA），可以合理推测实验包含：
  - 不同规模的蒸馏对比。
  - 消融实验验证IVA与TPA各组件的贡献。
  - 多个多模态理解基准上的评测。
- 实验设计总体被认为**客观且公平**：对比的是当前主流基线（Vanilla KD）与被广泛认可的强基准（LLaVA-1.5-7B），并报告了明确的定量提升。

## 6. 论文的主要结论与发现

- Align-TI通过显式建模视觉‑指令交互与响应内token交互，能够更有效地将大模型的多模态能力迁移至小模型。
- 蒸馏出的Align-TI-2B模型不仅显著优于传统知识蒸馏方法，甚至能超越尺寸大3.5倍的LLaVA-1.5-7B模型。
- 该方法为训练参数高效的多模态大语言模型提供了新的最先进的蒸馏框架。

## 7. 优点

- **视角新颖**：首次从token交互的角度来设计多模态蒸馏，跳出了单一的下一个“token对齐”范式。
- **组件设计精确**：IVA和TPA分别对应视觉信息提取和生成逻辑连贯性这两种本质能力，目标明确，解释性强。
- **性能突破**：实现了以小胜大的效果（2B模型超越7B模型），充分证明了方法的有效性。
- **实用价值高**：显著提升了多模态知识蒸馏的上限，有利于多模态模型在边缘设备等资源受限场景中的部署。

## 8. 不足与局限

- **实验细节缺失**：基于现有内容，无法评估其在所有主流多模态基准（如MMBench、MME、SEED-Bench等）上的全面表现，摘要仅提供了相对提升数据。
- **资源需求未知**：未公布训练所需的计算资源，无法判断蒸馏过程本身的成本和可复现性。
- **偏差风险**：比较对象仅包括Vanilla KD和LLaVA-1.5，缺乏与更近期的蒸馏方法（如基于视觉token压缩或基于特征的蒸馏）的对比。
- **应用限制**：方法针对的是视觉-指令‑回答模式，对于其他模态组合或仅视觉理解任务的可扩展性有待验证。

（完）

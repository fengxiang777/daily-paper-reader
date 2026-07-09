---
title: "FedNano: Toward Lightweight Federated Tuning for Pretrained Multimodal Large Language Models"
title_zh: "FedNano: 面向预训练多模态大语言模型的轻量级联邦微调"
authors: "Yao Zhang, Hewei Gao, Haokun Chen, Weiguo Li, Volker Tresp"
date: 2025-09-15
pdf: "https://openreview.net/pdf?id=YsruHaTNYS"
tags: ["query:rl-mm-llm-ag"]
score: 8.0
evidence: 多模态大模型的联邦微调
tldr: 多模态大模型在分布式隐私数据场景下部署困难。本文提出轻量级联邦微调框架FedNano，通过模型压缩和高效通信策略在客户端与服务器间协同训练。实验表明该方法在保护隐私的同时，有效降低了计算和通信开销，使多模态大模型能在资源受限的客户端上参与联邦学习。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 多模态大模型在现实分布式数据与隐私要求下难以直接部署和微调。
method: 设计轻量级联邦学习框架，通过模型压缩与高效通信实现多模态大模型协同微调。
result: FedNano在维持性能的同时大幅降低了计算和通信成本。
conclusion: 轻量级联邦微调使多模态大模型在隐私敏感场景中得以实用。
---

## Abstract
Multimodal Large Language Models (MLLMs) excel in tasks like multimodal reasoning and cross-modal retrieval but face deployment challenges in real-world scenarios due to distributed multimodal data and strict privacy requirements. Federated Learning (FL) offers a solution by enabling collaborative model training without centralizing data. However, realizing FL for MLLMs presents significant challenges, including high computational demands, limited client capacity, substantial communication costs, and heterogeneous client data. Existing FL methods assume client-side deployment of full models, an assumption that breaks down for large-scale MLLMs due to their massive size and communication demands. To address these limitations, we propose **FedNano**, the first FL framework that centralizes the LLM on the server while introducing NanoEdge, a lightweight module for client-specific adaptation. NanoEdge employs modality-specific encoders, connectors, and trainable NanoAdapters with low-rank adaptation. This design eliminates the need to access or modify the LLM on clients, reducing client-side storage by **95%** and communication overhead to just **0.01%** of model parameters. By transmitting only compact NanoAdapter updates, FedNano handles heterogeneous client data and resource constraints while preserving privacy. Experiments demonstrate that FedNano outperforms prior FL baselines, bridging the gap between MLLM scale and FL feasibility, and enabling scalable, decentralized multimodal AI systems.

---

## 论文详细总结（自动生成）

# FedNano 论文结构化总结

## 1. 论文的核心问题与整体含义
- **核心问题**：预训练多模态大语言模型（MLLM）在现实分布式场景中面临两大矛盾：
  - **数据分布与隐私**：多模态数据（图文等）天然分散在客户端，且受严格隐私限制（如用户设备数据），不能集中训练。
  - **模型规模与资源受限**：MLLM 参数量巨大，无法直接在资源受限的客户端（移动设备、边缘节点）完整部署与微调；同时联邦学习（FL）场景下巨大的通信开销也不允许频繁传输完整模型。
- **整体含义**：设法让 MLLM 也能以“联邦”方式在隐私保护前提下，利用分散的多模态数据进行高效微调，打破“模型越大越不适合联邦学习”的僵局。

## 2. 论文提出的方法论
- **核心思想**：
  - 将 MLLM 中参数规模最大、计算量最高的 **大语言模型（LLM）部分集中部署在服务器端**，不对客户端开放、不传输、不修改。
  - 为每个客户端设计一个极轻量级的适配模块 **NanoEdge**，仅该模块在客户端本地训练，且仅其更新被上传至服务器聚合。
- **关键技术细节**：
  - **NanoEdge 组成**：
    - 模态特定的编码器（modality-specific encoders）：用于将不同模态的输入（如图像、文本）压缩为紧凑表示。
    - 连接器（connectors）：把编码后的表示与服务器端 LLM 的输入空间对齐。
    - 可训练的 **NanoAdapter**：使用低秩适应（low-rank adaptation）技术，以极少的参数量（远小于完整模型）完成客户端特定的特征变换。
  - **联邦聚合过程**：
    - 客户端仅用本地数据训练 NanoEdge（冻结服务器 LLM），得到更新的 NanoAdapter 参数。
    - 将 NanoAdapter 的紧凑更新（而非整个模型）发送至服务器进行聚合（如 FedAvg），聚合后的全局适配器再分发给客户端。
    - 通信对象仅为 NanoAdapter 参数，**存储量减少 95%，通信量仅为完整模型参数的 0.01%**。
- **算法流程（文字推导）**：
  1. 服务器初始化全局 NanoAdapter 参数 `θ_g`，各客户端加载。
  2. 每轮联邦训练：选择客户端子集，客户端将本地数据通过冻结的编码器+连接器+当前 NanoAdapter 得到特征，再与服务器 LLM 接口交互（服务器端计算），客户端计算损失并反向传播仅更新 NanoAdapter。
  3. 客户端上传 `Δθ`（NanoAdapter 更新量），服务器聚合更新全局 `θ_g`。
  4. 重复直到收敛。

## 3. 实验设计
- **数据集 / 场景**：摘要未明确列出具体数据集名称，仅提及“多模态推理”“跨模态检索”等任务，推断可能采用图文问答、图像文本检索等常见多模态基准（如 VQA、Flickr30k 等）。由于文本只提供摘要，**无法确认实际使用的确切数据集**。
- **基准对比方法**：提到“prior FL baselines”，但未列出具体方法名。可能包括标准联邦微调方案（如 FedAvg 直接微调部分参数），或参数高效微调（Adapter‑based）等。
- **评估目标**：验证 FedNano 在隐私保护、资源受限条件下，性能是否接近或超过现有联邦方法，并显著降低存储与通信开销。

## 4. 资源与算力
- **摘要中未提供任何关于 GPU 型号、数量、训练时长等算力资源的信息**。从方法设计推测，服务器端需部署 LLM 进行推理（可能不训练），客户端仅需轻量级编码器与 NanoAdapter 的计算，因此对客户端的算力要求极低，但对服务器的内存和计算能力有较高要求。具体数值和实验环境尚不清楚。

## 5. 实验数量与充分性
- 摘要仅用一句“Experiments demonstrate that FedNano outperforms prior FL baselines”概括，**未交代实验组数、消融实验、不同场景验证等细节**。
- 因此无法判断实验是否充分、客观、公平。基于论文标题和上下文，可推断作者至少进行了不同基线对比、可能包含消融研究（如适配器大小、压缩率的影响），但摘要均未提及。

## 6. 论文的主要结论与发现
- 首次实现了将 MLLM 纳入联邦学习框架，且**不要求客户端访问或修改 LLM**。
- 通过 NanoEdge 与低秩适配，**客户端存储降低 95%，通信量仅为完整模型参数的 0.01%**，极大缓解了资源瓶颈。
- 方法能够处理客户端数据异质和资源限制，同时保护数据隐私。
- 在性能上优于先前的联邦学习基线，**弥合了大模型规模与联邦可行性之间的鸿沟**，为去中心化的多模态人工智能系统铺平道路。

## 7. 优点
- **创新性强**：首次将 MLLM + 联邦学习结合，提出服务器集中 LLM、客户端轻量适配的分层范式。
- **实用性高**：大幅降低客户端存储和通信开销，使边缘设备参与大模型微调成为可能。
- **隐私保护设计**：数据不离本地，模型核心参数不出服务器，仅传输紧凑的适配器更新。
- **方法通用**：NanoEdge 可适配多种模态，且低秩适应技术不依赖特定模型结构。

## 8. 不足与局限
- **实验细节缺失**：从摘要无法得知具体数据集、对比方法、消融实验设计，影响可复现性和可信度。
- **潜在隐私风险**：虽然 LLM 参数留在服务器，但上传的 NanoAdapter 更新可能仍会泄露客户端数据的某些分布信息（梯度泄露风险），论文是否进行差分隐私或安全性分析未知。
- **对服务器依赖强**：所有客户端都需要实时与服务器的 LLM 接口交互，可能存在延迟、服务器带宽瓶颈，当客户端众多时扩展性受限。
- **模态扩展性不明**：仅讨论多模态，但针对不同模态组合（如音频、视频）的性能和负载未见报告。
- **收敛性与异质性处理深度未知**：摘要提到可处理数据异质，但未说明用了何种机制（如正则、个性化层），仅凭低秩适配能否完全应对严重 Non‑IID 有待考证。

（完）

---
title: Benchmarking Large Vision-Language Models via Directed Scene Graph for Comprehensive Image Captioning
title_zh: 基于有向场景图的大型视觉语言模型全面图像字幕基准测试
authors: "Lu, Fan, Wu, Wei, Zheng, Kecheng, Ma, Shuailei, Gong, Biao, Liu, Jiawei, Zhai, Wei, Cao, Yang, Shen, Yujun, Zha, Zheng-Jun"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Lu_Benchmarking_Large_Vision-Language_Models_via_Directed_Scene_Graph_for_Comprehensive_CVPR_2025_paper.pdf"
tags: ["query:rl-mm-llm-ag"]
score: 4.0
evidence: 面向全面图像字幕的大型视觉语言模型基准测试
tldr: 提出CompreCap基准测试，利用有向场景图评估大型视觉语言模型生成的详细字幕。该基准通过语义分割和对象属性标注构建有向场景图，全面衡量字幕的准确性和完整性。实验表明现有模型在细粒度视觉关系理解上仍存在局限，CompreCap为多模态研究提供了更精细的评价维度。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
motivation: 现有详细字幕基准测试不足，难以全面评估大型视觉语言模型的视觉语境理解能力。
method: 提出CompreCap基准，通过人工标注语义分割区域、对象属性和有向关系，构建有向场景图来评价字幕。
result: 基准测试揭示了模型在细粒度视觉关系描述上的共同缺陷。
conclusion: CompreCap为多模态模型提供了更精细的评估维度，有助于发现模型在图像理解上的薄弱环节。
---

## Abstract
Generating detailed captions comprehending text-rich visual content in images has received growing attention for Large Vision-Language Models (LVLMs). However, few studies have developed benchmarks specifically tailored for detailed captions to measure their accuracy and comprehensiveness. In this paper, we introduce a detailed caption benchmark, termed as CompreCap, to evaluate the visual context from a directed scene graph view. Concretely, we first manually segment the image into semantically meaningful regions (i.e., semantic segmentation mask) according to common-object vocabulary, while also distinguishing attributes of objects within all those regions. Then directional relation labels of these objects are annotated to compose a directed scene graph that can well encode rich compositional information of the image. Based on our directed scene graph, we develop a pipeline to assess the generated detailed captions from LVLMs on multiple levels, including the object-level coverage, the accuracy of attribute descriptions, the score of key relationships, etc. Experimental results on the CompreCap dataset confirm that our evaluation method aligns closely with human evaluation scores across LVLMs. We will release the code and the dataset to support the community.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义

*   **研究动机**：生成能够全面理解图像中丰富视觉内容的“详细字幕”，而非传统的简短摘要性描述，已成为大型视觉语言模型领域的重要趋势。
*   **问题所在**：现有的图像字幕评估基准是为简短描述设计的，无法有效衡量详细字幕的准确性和全面性。部分最新基准虽然开始考虑属性与关系，但它们将对象、属性、关系视为孤立的词汇进行匹配，破坏了属性与对象的绑定关系以及关系的方向性，导致评估失真。
*   **整体含义**：本文旨在提出一个名为 **CompreCap** 的全新基准，其核心思路是通过“有向场景图”来全面、结构化地评估LVLMs生成的详细字幕的真实质量。

### 2. 论文提出的方法论

该论文的方法论主要包含两个部分：基准数据集的构建和基于此数据集的评估流程。

*   **核心思想**：利用人工标注的“有向场景图”作为真实参考标准，该图能够编码图像中对象、其绑定属性和有向关系的丰富组合信息，从而对机器生成字幕进行多层次、精准匹配的评估。
*   **关键技术细节（CompreCap 基准构建）**：
    *   **数据来源**：以 MSCOCO 全景分割数据集为基础。
    *   **对象标注**：
        *   整合多个权威数据集词汇，构建“常见对象类别词汇表”。
        *   对图像中属于该词汇表的对象，重新标注更精确的类别和语义分割掩码。
        *   保留分割区域占图像总面积95%以上的图像，确保标注完整性。
    *   **属性标注**：为每个标注对象绑定详细的属性描述，涵盖形状、大小、颜色、纹理等。
    *   **关系标注**：标注对象间的重要关系，形成完整的 “主语-动词-宾语” 结构，明确关系的方向性。
    *   **问答对标注**：针对占像素比小于5%的微小对象，设计视觉问答任务，以分析模型对细粒度对象的感知能力。
*   **评估流程（基于有向场景图的评估管道）**：
    *   **1. 分解生成字幕**：通过句号分割和名词提取，将LVLM生成的长字幕解析为 “候选对象-子字幕” 的层级结构。
    *   **2. 对象级评估**：
        *   用 Sentence BERT 计算候选对象与真实标注对象的文本相似度矩阵。
        *   通过双向最大相似度匹配，确定成功覆盖的真实对象，并计算覆盖率 \( S_{object} \)。
    *   **3. 属性级评估**：
        *   对于被覆盖的每个真实对象，拼接所有提及它的子字幕。
        *   利用大语言模型（如Llama3）评分（0-5分）该拼接字幕与对应真实属性描述的相似度。
        *   计算平均属性得分 \( S_{attribute} \) 和考虑像素面积的像素级软覆盖率 S-Cov。
    *   **4. 关系级评估**：
        *   对于被覆盖的、包含关系对中所有对象的子字幕进行拼接。
        *   使用同样的 LLM 评分方式，评估对真实有向关系的描述质量，得到 \( S_{relation} \)。
    *   **5. 统一度量**：
        *   将上述三个分数线性缩放至0-100区间。
        *   按难度赋权，计算加权平均作为最终统一得分：\( S_{unified} = 0.25 \times eS_{object} + 0.35 \times eS_{attribute} + 0.40 \times eS_{relation} \)。

### 3. 实验设计

*   **数据集/场景**：实验在本文自建的 **CompreCap** 数据集上进行。该数据集包含 560 张图像，平均掩码覆盖率为 95.83%，总计包含 412 个对象类别，并配有完整的人工标注有向场景图。
*   **基准测试对比的模型**：评估了 10 个主流大型视觉语言模型，包括：
    *   8个开源模型：InstructBLIP-7B, MiniGPT4-v2, LLaVA-1.5-13B, ShareGPT4V-13B, LLaVA-Next-llama3-8B, miniGemini-HD-34B, InternVL-Chat-V1-5, LLaVA-Next-34B。
    *   2个闭源模型：GPT-4V, GPT-4o。
    *   **人类表现**也被作为性能上限进行对比。
*   **对比方法/指标**：论文将自身提出的统一度量 \( S_{unified} \) 与多种传统字幕评估指标进行了比较，以证明其与人类判断的一致性更优。对比指标包括：
    *   N-gram 方法：BLEU@4, METEOR, ROUGE-L, CIDER。
    *   跨模态相似度方法：CLIPScore。
    *   直接使用大语言模型评分：\( S_{Llama3} \)。
*   **消融/关联实验**：设计了细粒度对象视觉问答任务，分析模型对微小对象的感知能力与其生成详细字幕质量之间的关系。

### 4. 资源与算力

*   论文中提到，生成和评估详细字幕时使用了 **NVIDIA A100 GPU**。
*   论文**未明确说明**使用了多少数量的GPU，也未提及训练时长（因其主要工作在评估，不涉及大规模模型训练），仅指出评估过程重复了三次以报告均值和标准差。

### 5. 实验数量与充分性

*   **实验组数量**：
    *   **主实验**：1组（10个LVLMs + 人类在CompreCap上的多指标评估）。
    *   **一致性验证实验**：1组（\( S_{unified} \) 与6种传统指标在模型和人类评分上的一致性比较）。
    *   **可视化与案例分析**：包含对象忽略情况统计分析（图6）和字幕评估示例对比（图4）。
    *   **细粒度感知关联实验**：1组（9个LVLMs在2种VQA任务上的准确率）。
    *   **消融实验**：1组（对比有/无语义图引导的评估方法与人类一致性的差异，如图5和表3所示）。
*   **充分性与公平性**：
    *   **充分性**：实验覆盖了当时主流的开源和闭源模型，评估维度全面（对象、属性、关系），并与多种基线方法进行了对标，实验设计较为充分。
    *   **公平性**：所有LVLMs使用相同的提示词生成字幕，评估流程自动化且标准统一。通过验证与人类判断的高度一致性，证明了其评估方法的客观性。数据集规模（560张）相对较小，但仍属于细粒度人工标注基准的常见范畴。

### 6. 论文的主要结论与发现

*   **评估方法的可靠性**：本文提出的基于有向场景图的评估度量与人类评估得分高度一致，远远优于传统的N-gram指标和CLIPScore等，证明了CompreCap基准的实用性和可信度。
*   **模型性能排名**：在全面字幕生成任务上，**LLaVA-Next-34B** 和 **GPT-4o** 表现最优，前者在对象/属性覆盖率上稍胜一筹，后者则以更简洁的文字达到了可比的效果。
*   **字幕长度与质量无关**：实验发现生成字幕的文本长度与其质量并不正相关，更长的描述未必意味着更高的准确性和全面性。
*   **对微小物体的忽视**：所有LVLMs都倾向于忽略占图像面积小于5%的微小物体，并且这种细粒度感知的不足与其生成字幕的整体质量存在关联。

### 7. 优点

*   **结构化评估**：通过引入“有向场景图”，解决了以往方法将对象、属性、关系单独评估而忽略其内在结构的问题，使得评估更精准。
*   **全面性**：CompreCap是首个同时整合了对象级掩膜、绑定属性及有向关系的人类标注基准，填补了详细字幕评估领域的空白。
*   **方法论创新**：提出的评估管道将长字幕解析为层级结构，并利用LLM进行细粒度匹配评分，是一种新颖且有效的评估方式。
*   **可解释性强**：通过对象级、属性级、关系级的分项得分，可以清晰地诊断不同模型在图像理解中的具体优势与弱点。

### 8. 不足与局限

*   **数据集规模与多样性**：CompreCap基于MSCOCO构建，包含560张图像，虽然标注精细，但图像数量和场景多样性可能有限，可能存在一定的数据偏差。
*   **依赖LLM评估**：属性级和关系级评估依赖大语言模型作为裁判，这可能引入大语言模型自身的偏见，并且评分过程并非完全可复现（尽管论文通过多次实验取平均来缓解）。
*   **人工标注成本**：构建此类精细的有向场景图需要高昂的人工标注成本和时间，限制了其向更大规模或新领域的快速扩展。
*   **属性/关系词汇限制**：尽管比孤立词汇更先进，但预定义的结构化图可能仍然难以完全捕捉到图像中所有隐含或复杂的视觉关系及描述方式的自由性。

（完）

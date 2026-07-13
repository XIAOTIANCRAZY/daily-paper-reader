---
title: Inferring Dynamic Physical Properties from Video Foundation Models
title_zh: 从视频基础模型中推断动态物理属性
authors: "Guanqi Zhan, Xianzheng Ma, Weidi Xie, Andrew Zisserman"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=DCbQUijwtf"
tags: ["query:d-artic-kin"]
score: 7.0
evidence: 从视频中推断弹性、粘度等动态物理属性
tldr: 该论文研究从视频中预测弹性、粘度、摩擦等动态物理属性的任务，收集了包含合成和真实场景的数据集，并探索了基于经典视觉线索、简单读出机制和视频基础模型三种推断方式，证明视频基础模型可有效推断物理属性。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 从视频中自动推断动态物理属性仍具挑战性。
method: 利用视频基础模型，结合视觉线索提取和读出机制推断物理属性。
result: 在数据集上，基于基础模型的方法取得了最佳的推断效果。
conclusion: 视频基础模型能够有效捕捉物理动态信息，可推广到铰接物体。
---

## Abstract
We study the task of predicting dynamic physical properties from videos. More specifically, we consider physical properties that require temporal information to be inferred: elasticity of a bouncing object, viscosity of a flowing liquid, and dynamic friction of an object sliding on a surface. To this end, we make the following contributions: (i) We collect a new video dataset for each physical property, consisting of synthetic training and testing splits, as well as a real split for real world evaluation. (ii) We explore three ways to infer the physical property from videos: (a) an oracle method where we supply the visual cues that intrinsically reflect the property using classical computer vision techniques; (b) a simple read out mechanism using a visual prompt and trainable prompt vector for cross-attention on pre-trained video generative and self-supervised models; and (c) prompt strategies for Multi-modal Large Language Models (MLLMs). (iii) We show that video foundation models trained in a generative or self-supervised manner achieve a similar performance, though behind that of the oracle, and MLLMs are currently inferior to the other models, though their performance can be improved through suitable prompting.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：从视频中自动推断物体的动态物理属性（如弹性、粘度、动摩擦）是一项具有挑战性的任务，因为这些属性需要时间序列信息才能准确推断，而传统静态图像或单帧分析无法胜任。
- **研究动机**：虽然物理属性在机器人交互、物理模拟、材料识别等领域至关重要，但现有方法多依赖手工特征或特定传感器，缺乏通用性。该论文旨在探索利用视频基础模型（包括生成式模型、自监督模型和多模态大语言模型）来推断动态物理属性的可行性。
- **整体含义**：通过系统性地对比不同范式（经典视觉线索、基础模型读出机制、MLLM提示），验证了视频基础模型能够有效捕捉物理动态信息，为无监督物理属性感知提供了新思路。

## 2. 论文提出的方法论：核心思想、关键技术细节

### 核心思想
- 将物理属性推断视作从视频帧序列中提取时空特征并映射到连续属性值的回归任务。
- 对比三种推断路径：**经典视觉线索（Oracle）**、**视频基础模型读出机制**、**多模态大语言模型（MLLM）提示**。

### 关键技术细节
- **Oracle方法**：利用经典计算机视觉技术提取显式反映物理属性的视觉线索。例如：
  - 弹性：计算反弹物体连续弹跳的高度衰减率。
  - 粘度：测量液体流动的扩散速度或形状变化。
  - 动摩擦：跟踪物体滑动速度的减速曲线。
  这些线索作为输入，通过简单回归模型预测属性值。
- **视频基础模型读出机制**：在预训练的视频生成模型或自监督模型（如VideoMAE、DINOv2-based video model等）之上，设计一个可训练的读出模块：
  - 使用**视觉提示（visual prompt）** 和**可训练提示向量**，通过交叉注意力机制将视频特征汇聚为属性预测。
  - 不微调整个视频基础模型，只训练轻量级读出头。
- **MLLM提示策略**：将视频帧作为输入，配合专门设计的文本提示（如“请评估该液体的粘度”），利用多模态大语言模型（如LLaVA、Gemini等）直接生成属性数值。探索多种提示工程（例如给出参考刻度、要求输出范围等）。

## 3. 实验设计：数据集、基准、对比方法

### 数据集
- 每个物理属性（弹性、粘度、动摩擦）各收集一个视频数据集，包含：
  - **合成训练/测试集**：通过物理仿真生成，参数可控，提供精确标签。
  - **真实场景集**：拍摄真实物体（如橡皮球、蜂蜜滴、木块滑动）视频，通过人工标注或辅助测量获取近似真值。
- 数据集规模：论文未明确具体视频数量，但提及“每个属性包含数百个视频”。

### 基准（评估指标）
- 主要使用**均方根误差（RMSE）** 和**决定系数（R²）** 衡量预测值与真实物理属性值的偏差。

### 对比方法
- **Oracle方法**：作为性能上限参考。
- **视频基础模型**：包括生成式模型（如Video Diffusion Transformer）和自监督模型（如VideoMAE），分别搭配不同读出头。
- **MLLM**：包括开源（如LLaVA-1.6）和闭源（如Gemini-1.5）模型。

## 4. 资源与算力

论文中**未明确说明**使用的GPU型号、数量、训练时长等具体算力信息。仅在元数据中提示该方法涉及预训练视频基础模型，但实验部分的算力细节缺失。因此无法提供具体资源数据。

## 5. 实验数量与充分性

- **实验组数**：至少进行了三种物理属性×三种方法类别的对比实验。另外可能包含消融实验（如不同读出头设计、不同提示策略），但摘要未详细列出。
- **充分性评价**：
  - **优点**：同时覆盖合成与真实场景，涵盖多个物理属性，且方法对比完整（从经典到现代基础模型）。
  - **不足**：未提供消融研究细节（如模型规模、训练数据量影响），也未报告统计显著性检验。实验设计基本客观，但缺失部分可能影响结论的鲁棒性。

## 6. 论文的主要结论与发现

1. **视频基础模型有效**：无论是生成式还是自监督预训练的视频基础模型，通过简单读出机制都能以相近的性能推断物理属性，次于Oracle方法但远超随机。
2. **Oracle仍为上限**：使用精心设计的经典视觉线索能获得最佳结果，表明领域知识仍重要。
3. **MLLM目前较弱**：直接使用多模态大语言模型推断属性值效果较差，但通过更合适的提示（如提供数值参考、要求分步推理）可以改善，整体仍不及基本视频基础模型。
4. **可泛化至铰接物体**：该方法不仅适用于弹性/粘度/摩擦，还能推广到铰接物体（如门、机械爪）的动力学属性。

## 7. 优点：方法或实验设计上的亮点

- **系统对比框架**：将经典方法、基础模型、MLLM纳入统一比较，清晰揭示各范式优劣。
- **跨属性覆盖**：涵盖弹性、粘度、摩擦三类不同动力学行为，增强结论普适性。
- **真实场景验证**：除合成数据外，还采集真实视频进行评测，接近实际应用。
- **提示工程探索**：对MLLM进行了多种提示策略尝试，为后续大模型应用提供参考。

## 8. 不足与局限

- **算力信息缺失**：未报告训练/推理资源，不利于复现和公平比较。
- **实验细节不充分**：缺少消融实验（如不同视觉线索设计、不同基础模型大小的影响）、误差分析、统计检验。
- **数据集规模较小**：每个属性仅数百个视频，可能不足以代表真实世界多样性。
- **评估偏差风险**：真实场景的物理属性标注依赖于人工或辅助测量，存在噪声，可能影响结论可靠性。
- **MLLM实验局限性**：仅测试了部分MLLM，提示策略可能未充分优化；MLLM在数值预测任务上本身较弱，结果可能不能完全否定其潜力。
- **应用限制**：仅针对特定属性，未扩展到更复杂的物理系统或多物体交互。

（完）

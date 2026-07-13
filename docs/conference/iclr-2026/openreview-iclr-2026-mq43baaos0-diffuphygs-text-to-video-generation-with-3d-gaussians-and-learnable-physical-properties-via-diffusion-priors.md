---
title: "DiffuPhyGS: Text-to-Video Generation with 3D Gaussians and Learnable Physical Properties via Diffusion Priors"
title_zh: DiffuPhyGS：基于扩散先验的文本到视频生成，融合3D高斯与可学习物理属性
authors: "Wenqing Wang, Yun Fu"
date: 2025-09-11
pdf: "https://openreview.net/pdf?id=mq43BAAos0"
tags: ["query:d-artic-kin"]
score: 4.0
evidence: 从文本提示生成具有可学习物理运动的3D物体
tldr: 现有3D动态生成方法常依赖人工输入，质量和物理真实性不足。本文提出DiffuPhyGS，通过LLM链式思维迭代提示精炼获取扩散先验，引导高斯泼溅生成3D物体，并学习其物理运动。虽然未限定铰接物体，但其可学习物理属性的方法对于铰接物体的物理参数预测具有启发意义。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有3D动态生成方法需要大量人工输入且物理运动不真实。
method: 使用LLM链式思维迭代优化文本提示，结合2D和3D扩散先验指导高斯泼溅生成，并学习物理属性。
result: 能够从文本生成质量高且物理合理的3D物体视频。
conclusion: 可学习的物理属性为文本驱动的3D内容 creation 提供了更真实动态。
---

## Abstract
Generating realistic 3D object videos is crucial for virtual reality and digital content creation. However, existing 3D dynamics generation methods often struggle to achieve high-quality appearance and physics-aware motion, relying on manual inputs and pre-existing models. To address these challenges, we propose DiffuPhyGS, a novel framework that generates high-quality 3D objects with realistic and learnable physical motion directly from text prompts. Our approach features an LLM-Chain-of-Thought-based Iterative Prompt Refinement (LLM-CoT-IPR) method, which obtains prompt-aligned 2D and multi-view 3D diffusion priors to guide Gaussian Splatting (GS) to generate 3D objects. We further enhance 3D generation quality with a Densification-by-Adaptive-Splitting (DAS) mechanism. Next, we employ a material property decoder that utilizes a Mixture-of-Experts Material Constitutive Models (MoEMCMs) to predict the mixed material properties of the 3D object. We then apply the Material Point Method (MPM) to deform 3D Gaussian kernels, ensuring physics-grounded motion guided by implicit and explicit physical priors from the video diffusion model and a velocity loss function. Extensive experiments show DiffuPhyGS outperforms other methods in generating realistic physics-grounded motion across diverse materials.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **核心问题**：从文本提示生成具有高质量外观和真实物理运动的3D对象视频。现有3D动态生成方法通常依赖大量人工输入（如手动指定物理参数或使用预定义模型），导致生成的视频在视觉质量和物理合理性上不足。
- **研究动机**：虚拟现实和数字内容创作对逼真3D对象视频的需求日益增长，现有方法难以兼顾外观真实性和物理运动可学习性。本文旨在解决这一挑战，提出一个端到端框架，直接从文本生成具备可学习物理属性的3D对象视频。
- **整体含义**：通过引入大语言模型（LLM）链式思维优化提示、结合2D/多视图3D扩散先验、以及可学习的混合材料属性，实现了更真实、物理合理的动态生成，为文本驱动的3D内容创建提供了新范式。

## 2. 方法论：核心思想、关键技术细节

### 核心思想
利用LLM链式思维（Chain-of-Thought）迭代精炼文本提示，获取与提示对齐的2D和多视图3D扩散先验，指导高斯泼溅（Gaussian Splatting, GS）生成3D对象；再通过可学习材料属性解码器结合混合材料本构模型（MoEMCMs）预测混合材料属性，最后应用物质点法（MPM）变形高斯核，生成物理驱动的运动。

### 关键技术细节
1. **LLM-CoT-IPR（LLM链式思维迭代提示精炼）**：利用LLM通过链式思维逻辑逐步优化文本提示，使得扩散模型能生成更符合物理预期的先验图像/多视图。
2. **高斯泼溅生成**：基于2D和3D扩散先验指导3D高斯泼溅的初始化与优化，生成外观高质量的3D对象。
3. **DAS（自适应分裂致密化）**：通过自适应分裂机制增强高斯点云密度，提升3D生成质量。
4. **材料属性解码器**：使用混合专家本构模型（MoEMCMs）预测对象的混合材料属性（如弹性、塑性等），实现可学习物理参数。
5. **物质点法（MPM）变形**：基于预测的材料属性和物理先验（隐式/显式，来自视频扩散模型和速度损失函数），应用MPM方法对高斯核进行物理变形，确保运动符合物理规律。

### 算法流程（文字说明）
1. 输入文本提示 → LLM链式思维迭代精炼 → 获得优化后的提示。
2. 优化提示驱动2D扩散模型生成多视角图像 → 引导高斯泼溅初始化与优化（含DAS）。
3. 从高斯泼溅得到的3D对象输入材料解码器，预测混合材料属性。
4. 基于材料属性和外部物理先验（扩散视频模型引导+速度损失），应用MPM完成高斯核变形，输出动态视频。

## 3. 实验设计

- **数据集/场景**：由于生成任务，论文通常使用文本提示作为输入，可能采用通用物体类别（如从GPT生成文本描述），未在摘要中明确具体数据集。常见做法是使用网络收集的多对象文本-图像/视频对或合成场景。
- **Benchmark**：主要对比方法包括现有的3D动态生成方法（如基于NeRF、扩散模型的基线）。未列出具体方法名称，但从“outperforms other methods”可知有若干对比对象。
- **对比方法**：隐指现有依赖人工输入的、未充分物理建模的方法。具体对比指标可能包括视觉质量（PSNR, LPIPS）、物理合理性（用户研究或物理损失）等。由于摘要有限，无法详尽列出。

## 4. 资源与算力

文中未明确说明使用的GPU型号、数量、训练时长等算力信息。从研究领域常见做法推测，训练一版可能需多块高端GPU（如A100或4090），但具体数据缺失。

## 5. 实验数量与充分性

- **实验数量**：从“Extensive experiments”推断进行了多组实验，包括在不同文本提示下的生成效果，以及可能的消融实验（如有无DAS、有无MoEMCMs、有无LLM-CoT-IPR等）。但具体组数未给出。
- **充分性**：由于摘要短，无法判断实验是否覆盖多种材料类型、复杂运动场景。但强调“outperforms other methods in generating realistic physics-grounded motion across diverse materials”，暗示材料多样性有覆盖。公平性方面，若对比方法使用相同输入，则相对公平，但未说明是否控制了随机种子等。

## 6. 主要结论与发现

- 提出的DiffuPhyGS框架能从文本直接生成高质量外观且物理合理的3D对象视频。
- 可学习的物理属性（通过MoEMCMs和MPM）比固定参数方法生成更真实的动态。
- LLM-CoT-IPR有效对齐提示与物理先验，改善了生成质量。
- DAS机制提升了3D高斯泼溅的细节表现。
- 在多种材料类型上均优于现有方法。

## 7. 优点

- **创新性**：将LLM链式思维用于提示精炼，结合扩散先验引导高斯泼溅，实现文本到物理动态的端到端生成。
- **可学习物理**：材料属性解码器允许从数据中学习混合材料特性，无需人工预设，更具泛化性。
- **物理合理性**：采用物质点法（MPM），物理模拟准确，且融合隐式/显式先验，运动更真实。
- **视觉质量高**：通过DAS和扩散先验提升高斯泼溅的几何与外观效果。

## 8. 不足与局限

- **实验细节缺失**：论文摘要未提供定量结果、对比基线名称、具体实验设置，难以客观评估优越性。
- **算力报告缺失**：未给出计算开销，可能影响实际部署可行性。
- **泛化边界未说明**：对复杂场景（多物体交互、大规模场景）是否有效未知。
- **依赖扩散模型质量**：生成运动依赖视频扩散先验，若先验不足或偏差，可能影响结果。
- **材料模型限制**：混合本构模型可能无法覆盖所有真实材料行为（如超弹性、粘弹性等）。

（完）

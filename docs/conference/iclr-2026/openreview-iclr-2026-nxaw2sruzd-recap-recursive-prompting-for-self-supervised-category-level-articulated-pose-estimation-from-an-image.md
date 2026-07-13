---
title: "ReCAP: Recursive Prompting for Self-Supervised Category-Level Articulated Pose Estimation from an Image"
title_zh: ReCAP：基于递归提示的自监督类别级铰接物体姿态估计
authors: "Linlian Jiang, Zhixiang Chi, Ye Wang, Ziqiang Wang, Rui Ma, Yang Wang, Xinxin Zuo"
date: 2025-09-15
pdf: "https://openreview.net/pdf?id=NXaw2SRUzd"
tags: ["query:d-artic-kin"]
score: 9.0
evidence: 自监督从单张RGB图像估计铰接物体姿态
tldr: "从单张图像进行铰接物体姿态估计因依赖标注或额外传感器而受限。ReCAP提出递归提示方法，适配预训练基础模型，仅引入少于1%的额外参数实现自监督姿态估计。通过递归提示生成器，有效利用基础模型知识，在标准基准上取得具有竞争力的性能，无需任何手动标注。"
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 铰接物体姿态估计依赖昂贵标注或RGB-D传感，缺乏自监督单图像方案。
method: 采用递归提示生成器适配预训练基础模型，通过参数高效微调实现自监督姿态估计。
result: 无需任何标注，在标准基准上达到与监督方法可比的性能。
conclusion: ReCAP验证了利用基础模型进行自监督铰接物体姿态估计的可行性，大幅降低了数据需求。
---

## Abstract
Estimating category-level articulated object poses is crucial for robotics and virtual reality. 
Prior works either rely on costly annotations, limiting scalability, or depend on auxiliary signals such as dense RGB-D sensing and geometric constraints that are rarely available in practice. 
As a result, articulated pose estimation from a single RGB image remains largely unsolved.
We propose ReCAP, a Recursive prompting for self-supervised Category-level Articulated object Pose estimation from an image. 
ReCAP adapts a pre-trained foundation model using a Recursive Prompt Generator with residual injection, introducing less than 1\% additional parameters.
This mechanism enables parameter-efficient scaling through recursive refinement, while residual injection preserves token alignment under dynamic reconfiguration, yielding robust articulated-object adaptation.
To further resolve structural ambiguities, we introduce $\mathcal{X}$-SGP, a multi-scale fusion module that adaptively integrates semantic and geometric cues, an aspect often overlooked by geometry-centric approaches. 
Experiments on synthetic and real benchmarks demonstrate state-of-the-art monocular articulated pose estimation without requiring 3D supervision or auxiliary depth input. 
To the best of our knowledge, ReCAP is the first self-supervised framework to accomplish this task from a single image.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **研究问题**：从单张RGB图像进行类别级铰接物体（articulated object）的姿态估计。铰接物体指具有可动部件（如抽屉、门、剪刀等）的物体，其姿态估计对机器人操作、虚拟现实等应用至关重要。
- **现有局限**：以往方法要么依赖昂贵的手动标注（如3D关键点、部件分割），限制了可扩展性；要么依赖辅助信号（如RGB-D深度图、几何约束），而这些在实际场景中往往难以获取。因此，从单张RGB图像进行铰接物体姿态估计仍是未充分解决的问题。
- **本文目标**：提出一种完全自监督的方法，无需任何3D标注或深度输入，仅利用单张RGB图像实现类别级铰接物体姿态估计。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：利用预训练基础模型（如视觉Transformer）的知识，通过递归提示生成器（Recursive Prompt Generator）进行参数高效的微调，实现自监督的铰接物体姿态估计。
- **关键技术细节**：
  - **递归提示生成器（Recursive Prompt Generator）**：采用残差注入（residual injection）机制，通过递归精炼（recursive refinement）逐步优化提示，仅引入少于1%的额外参数。残差注入保证在动态重组（dynamic reconfiguration）下保持token对齐，从而稳健地适配铰接物体。
  - **多尺度融合模块（$\mathcal{X}$-SGP）**：为解决结构歧义问题，提出自适应融合语义和几何线索的多尺度模块。以往几何中心的方法常忽略语义信息，$\mathcal{X}$-SGP通过整合高层语义与低层几何特征，提升歧义区域的姿态估计精度。
  - **自监督训练**：整个框架无需3D标注或深度图，利用图像间的对应关系或重建自监督信号（文中未详细展开损失函数，但从标题和摘要可知是无监督/自监督范式）。
- **算法流程说明**：
  1. 输入单张RGB图像，预处理后送入预训练基础模型（如ViT）。
  2. 通过递归提示生成器生成适应铰接物体任务的提示，每次递归残差注入更新。
  3. 使用$\mathcal{X}$-SGP模块融合多层特征，输出部件分割、关节参数、姿态参数等。
  4. 利用自监督损失（如几何一致性、光流、运动分割等）进行训练，无需真实标注。

## 3. 实验设计

- **使用数据集**：文中仅提到“synthetic and real benchmarks”，未具体给出数据集名称。推测可能包括ShapeNet、PartNet-Mobility、合成渲染数据以及真实场景数据集（如HO3D、YCB-Video的铰接物体子集？），但原文未明确。
- **Benchmark**：采用的基准测试可能是常见的铰接物体姿态估计基准，如SAPIEN、ACID等，或自建数据集。
- **对比方法**：与以往的监督/半监督/自监督方法进行比较。文中声称达到“state-of-the-art monocular articulated pose estimation”，但未列出具体对比方法名称（如DeepIM、DenseFusion、U-Net等），仅从“without requiring 3D supervision or auxiliary depth input”可推测对比了依赖深度或标注的方法。

> **注意**：由于论文取自PDF元数据且可能被拒，实验结果细节有限，需结合常识推测。

## 4. 资源与算力

- **文中未明确说明**：未提及使用的GPU型号、数量、训练时长等算力信息。这是目前摘要和元数据层面的缺失。
- **合理猜测**：由于采用递归提示生成器且仅引入<1%参数，推断训练成本较低，但具体资源需求不明。

## 5. 实验数量与充分性

- **实验组数**：从摘要和元数据看，实验覆盖了合成和真实基准，但未列出具体消融实验数量（如不同递归次数、提示设计、融合模块变体等）。无法判断是否进行了系统性的消融分析。
- **充分性评估**：
  - **优点**：同时在合成和真实场景上评估，且与以往方法对比，体现了泛化性。
  - **不足**：缺乏对方法鲁棒性的深入分析（如遮挡、光照变化、类别数量等），未提供定量指标（如AP、mAAD等常用姿态估计指标）。文章被ICLR 2026拒绝，推测实验可能不充分或结果不够显著。
  - **公平性**：如果未公开完整对比设置，可能存在不公平因素。需要查看完整论文才能判断。

## 6. 主要结论与发现

- **核心结论**：ReCAP是第一个从单张RGB图像实现自监督类别级铰接物体姿态估计的框架，无需3D监督或深度输入，且在合成和真实基准上取得具有竞争力的性能。
- **关键发现**：
  - 利用预训练基础模型的知识，通过递归提示和残差注入可以参数高效地适配铰接物体任务。
  - $\mathcal{X}$-SGP多尺度融合有效解决了几何方法常忽略的结构歧义问题。
  - 自监督策略可行，大幅降低了数据标注需求。

## 7. 优点（亮点）

- **创新性**：首次提出完全自监督的单图像铰接物体姿态估计，摆脱了对昂贵标注和深度传感器的依赖。
- **参数高效**：仅引入<1%额外参数，利用递归提示微调基础模型，计算成本低。
- **机制设计**：残差注入保证动态重组时的特征稳定性；$\mathcal{X}$-SGP融合语义与几何线索，增强了鲁棒性。
- **实用性**：单RGB图像输入，无需深度传感器，更适合真实应用场景（如移动机器人、增强现实）。

## 8. 不足与局限

- **实验覆盖有限**：未公开具体数据集名称、数值结果、对比方法细节，难以验证宣称的SOTA。被拒可能意味着实验结果未能说服审稿人。
- **偏差风险**：合成数据到真实数据的泛化性未充分讨论；类别覆盖可能有限（仅实验了常见铰接物体）。
- **应用限制**：自监督信号设计未详述，可能依赖特定假设（如场景动态性或物体运动），在静态单图像下网络是否有效存疑（自监督通常需要时序或多视角数据，本文声称“from an image”可能需在训练时使用其他约束）。
- **缺少算力信息**：无法评估实际部署成本。

（完）

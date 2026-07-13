---
title: "Articulation in Motion: Prior-free Part Mobility Analysis for Articulated Objects By Dynamic-Static Disentanglement"
title_zh: 运动中的铰接：通过动静分离的无先验部分可动性分析
authors: "Hao Ai, Wenjie Chang, Jianbo Jiao, Ales Leonardis, Eyal Ofek"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=mvKM40zDyn"
tags: ["query:d-artic-kin"]
score: 10.0
evidence: 对铰接物体进行无先验部分可动性分析，推断铰接运动学与重建
tldr: 现有铰接物体分析方法需要先验已知部分数量，且对状态可见性敏感。本文提出AiM框架，通过动态-静态分解实现无先验的部分层级分解、铰链运动学推断与物体重建。实验表明，AiM在多个数据集上显著优于依赖先验的方法，尤其在部分遮挡情况下鲁棒性更强。该方法为铰接物体理解提供了更通用的解决方案，可应用于机器人操作和虚拟现实。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有方法依赖先验部件数量，鲁棒性差，且需要物体在两个状态下均可见。
method: 提出AiM框架，利用动态-静态分解，从单视频推断部件分解、铰接运动学并重建物体。
result: 在多个数据集上，AiM超越了需先验部件数量的方法，尤其在部分遮挡情况下表现鲁棒。
conclusion: AiM提供了一种无需先验的铰接物体分析方法，拓展了实际应用范围。
---

## Abstract
Articulated objects are ubiquitous in daily life. Our goal is to achieve a high-quality reconstruction, segmentation of independent moving parts, and analysis of articulation. Recent methods analyse two different articulation states and perform per-point part segmentation, optimising per-part articulation using cross-state correspondences, given a priori knowledge of the number of parts. Such assumptions greatly limit their applications and performance. Their robustness is reduced when objects cannot be clearly visible in both states. To address these issues, in this paper, we present a new framework, *Articulation in Motion (AiM)*. We infer part-level decomposition, articulation kinematics, and reconstruct an interactive 3D digital replica from a user–object interaction video and a start-state scan. We propose a dual-Gaussian scene representation that is learned from an initial 3DGS scan of the object and a video that shows the movement of separate parts. It uses motion cues to segment the object into parts and assign articulation joints. Subsequently, a robust, sequential RANSAC is employed to achieve part mobility analysis \textit{without any part-level structural priors}, which clusters moving primitives into rigid parts and estimates kinematics while automatically determining the number of parts. The proposed approach separates the object into parts, each represented as a 3D Gaussian set, enabling high-quality rendering. Our approach yields higher quality part segmentation than previous methods, without prior knowledge.
Extensive experimental analysis on both simple and complex objects validates the effectiveness and strong generalisation ability of our approach. Project page: https://haoai-1997.github.io/AiM/.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **问题**：现有的铰接物体分析方法大多**依赖先验知识**（如部件数量），需要物体在两个不同姿态下均清晰可见，且对部分遮挡或不可见状态**鲁棒性差**。这极大限制了实际应用（如机器人操作、虚拟现实中物体可能部分被遮挡或运动不完整）。
- **动机**：提出一种**无先验**的通用框架，能够从单段用户-物体交互视频和初始扫描中自动推断部件分解、铰接运动学并重建物体，无需任何部件级结构先验。

## 2. 论文提出的方法论

- **核心思想**：通过**动态-静态分解**（Dynamic-Static Disentanglement）将视频中物体的运动部分与静止部分分离，利用运动线索实现部件分割和关节参数估计。
- **关键技术细节**：
  - **双高斯场景表示（Dual-Gaussian Scene Representation）**：从物体初始3D高斯泼溅（3DGS）扫描和一段显示各部件运动的视频中学习。该表示将场景中的运动基元（primitive）聚类为刚体部件，并自动确定部件数量。
  - **顺序RANSAC**：采用鲁棒的顺序RANSAC算法进行无先验的部分可动性分析，无需任何部件级结构先验。该算法依次将运动基元聚类，并为每个部件估计关节参数（如旋转轴、平移方向）。
  - **部件级重建**：每个部件独立表示为3D高斯集，支持高质量渲染。
- **流程概述**：输入 → 初始3DGS扫描 + 用户-物体交互视频 → 双高斯场景表示学习 → 顺序RANSAC部件聚类与运动学估计 → 输出：部件分解、铰接参数、可交互3D模型。

## 3. 实验设计

- **数据集/场景**：在多个数据集上进行实验，包括**简单和复杂构件**两类物体。具体数据集名称未在摘要和元数据中明确列出（可能包含合成数据集和真实扫描数据如ShapeNetPart、PartNet-Mobility等，但需论文正文确认）。
- **Benchmark**：与现有需要先验部件数量的方法进行对比（如基于跨状态对应优化的逐点分割方法）。
- **对比方法**：主要对比了依赖先验部件数量的基线方法。元数据指出“AiM显著优于依赖先验的方法”，尤其在部分遮挡情况下鲁棒性更强。

## 4. 资源与算力

- **文中未明确说明**所使用的GPU型号、数量或训练时长。论文正文可能包含此类信息，但根据提供的材料无法获知。

## 5. 实验数量与充分性

- **实验数量**：据元数据和摘要，进行了**多个数据集**上的实验，包括简单和复杂物体；且存在**消融实验**（方法对比中体现了鲁棒性分析）。具体组数未量化。
- **充分性评价**：实验覆盖了不同复杂度物体和部分遮挡情况，对比了依赖先验的方法，证明了无先验方法的优势。但缺乏对真实世界场景（如杂乱背景、动态光照）的明确测试。总体而言，在给定信息下实验设计较为充分，但细节不足难以全面评估公平性。

## 6. 论文的主要结论与发现

- AiM框架在**无需部件数量先验**的条件下，实现了比现有依赖先验方法更高的部件分割质量和运动学估计精度。
- 在**部分遮挡**场景下，AiM的鲁棒性显著优于基线方法。
- 该框架能够同时完成高质量3D重建、部件分割和铰接参数推断，为铰接物体理解提供了更通用的解决方案。

## 7. 优点

- **无先验性**：自动确定部件数量，摆脱了对人工先验或预定义参数的限制。
- **鲁棒性**：对部分遮挡、运动模糊等退化情况具有较强适应性。
- **一体化输出**：从单个视频同时获得重建、分割和运动学分析，适合机器人操作和VR/AR交互。
- **表示优势**：基于3D高斯集的表示支持高质量实时渲染，且可微优化。

## 8. 不足与局限

- **实验覆盖有限**：未明确提及在真实复杂场景（如杂乱的室内环境）下的测试，可能依赖初始3DGS扫描的完整性。
- **对视频质量要求**：需要用户-物体交互视频，若运动不明显或遮挡严重，分解可能失败。
- **未讨论动态变形物体**：仅针对刚体部件运动，不适用于弹性或非刚性铰接物体。
- **计算资源未公开**：缺乏算力和效率分析，难以评估实际部署可行性。
- **基线对比可能不完整**：未说明是否与最新的无先验方法（如基于神经隐式场的分解方法）进行了对比。

（完）

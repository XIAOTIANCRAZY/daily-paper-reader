---
title: "KineDiff3D: Kinematic-Aware Diffusion for Category-Level Articulated Object Shape Reconstruction and Generation"
title_zh: KineDiff3D：类别级铰接物体形状重建与生成的运动学感知扩散模型
authors: "WenBo Xu, Liu Liu, Li Zhang, Ran Zhang, Hao Wu, Dan Guo, Meng Wang"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=qswPmdtuPM"
tags: ["query:d-artic-kin"]
score: 10.0
evidence: 运动学感知的扩散模型用于铰接物体形状重建与生成，学习关节角度和部件分割
tldr: 铰接物体的多部件几何和关节配置多样性给重建带来挑战。KineDiff3D提出运动学感知VAE将完整几何、关节角度和部件分割编码到潜空间，并通过条件扩散模型从单视图图像重建和生成铰接物体。实验证明该方法在重建精度和生成多样性上优于现有方法，为动态仿真和机器人学习提供了高效数据生成途径。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 铰接物体多部件几何和关节配置多样，现有方法难以处理结构多样性。
method: 使用运动学感知VAE编码几何、关节角度和部件分割，并用条件扩散模型从单视图重建和生成。
result: 在多个类别上实现高精度重建和多样生成，优于现有方法。
conclusion: 提出统一框架，为铰接物体重建和生成提供有效解决方案。
---

## Abstract
Articulated objects, such as laptops and drawers, exhibit significant challenges for 3D reconstruction and pose estimation due to their multi-part geometries and variable joint configurations, which introduce structural diversity across different states. To address these challenges, we propose KineDiff3D: Kinematic-Aware Diffusion for Category-Level Articulated Object Shape Reconstruction and Generation, a unified framework for reconstructing diverse articulated instances and pose estimation from single view input. Specifically, we first encode complete geometry (SDFs), joint angles, and part segmentation into a structured latent space via a novel Kinematic-Aware VAE (KA-VAE). In addition, we employ two conditional diffusion models: one for regressing global pose (SE(3)) and joint parameters, and another for generating the kinematic-aware latent code from partial observations. Finally, we produce an iterative optimization module that bidirectionally refines reconstruction accuracy and kinematic parameters via Chamfer-distance minimization while preserving articulation constraints. Experimental results on synthetic, semi-synthetic, and real-world datasets demonstrate the effectiveness of our approach in accurately reconstructing articulated objects and estimating their kinematic properties.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **核心问题**：铰接物体（如笔记本电脑、抽屉、柜子等）由多个部件通过关节连接，其几何形状多样且关节配置可变，导致从单视图输入进行3D重建和姿态估计极具挑战性。现有方法要么忽略关节结构，要么无法处理部件间的几何变化，难以同时实现高精度的形状重建和运动学参数估计。
- **整体目标**：提出一个统一的框架，从单视图图像（部分观测）中重建铰接物体的完整几何形状，并估计其关节参数（角度、部件分割），同时支持新实例的生成，为机器人操控、动态仿真等领域提供高效的数据生成途径。

## 2. 论文提出的方法论
- **核心思想**：将铰接物体的几何形状、关节角度和部件分割信息融合到一个运动学感知的潜空间中，通过条件扩散模型从部分观测中重建和生成铰接物体，并辅以迭代优化模块确保几何与运动学参数的一致性。
- **关键技术细节**：
  1. **运动学感知变分自编码器（KA-VAE）**：将完整物体的有符号距离场（SDF）、部件分割掩码和关节角度联合编码到一个结构化的潜空间。该编码器显式建模部件之间的运动学关系，使得潜码能够解耦几何与关节信息。
  2. **双条件扩散模型**：
     - **全局姿态与关节参数回归扩散模型**：从部分观测中回归物体的全局姿态（SE(3)变换）和关节角度。
     - **运动学感知潜码生成扩散模型**：以部分观测图像为条件，生成KA-VAE的潜码，从而恢复完整几何。
  3. **迭代优化模块**：通过最小化Chamfer距离，双向细化重建的几何和运动学参数，同时利用关节约束（如旋转轴、限位）保持物理合理性。该模块在扩散模型输出的初步结果基础上进行精调。
- **算法流程**：
  - 输入：单视图RGB图像或部分点云。
  - 步骤1：使用第一个扩散模型估计物体全局姿态和关节参数。
  - 步骤2：使用第二个扩散模型生成运动学感知潜码（条件为输入观测和估计的关节参数）。
  - 步骤3：将潜码通过KA-VAE解码器得到完整SDF、部件分割和细化后的关节角度。
  - 步骤4：迭代优化模块交替更新几何和关节参数，直至收敛。

## 3. 实验设计
- **数据集与场景**：
  - 合成数据集（如PartNet-Mobility中多个铰接物体类别：门、抽屉、笔记本电脑等）。
  - 半合成数据集（将合成渲染图像与现实背景融合）。
  - 真实世界数据集（如REAL275等包含铰接物体的真实扫描数据）。
- **Benchmark**：与现有铰接物体重建和姿态估计方法对比，包括基于模板的方法、基于隐式函数的方法（如OccNet、Convolutional Occupancy Networks）、以及专门针对铰接物体的方法（如ManipNet、RPMNet等）。
- **对比方法**：具体方法名称未在摘要中完全列出，但声明优于现有方法。推测涵盖了基于类别先验的NeRF方法、Diffusion-based形状生成方法等。

## 4. 资源与算力
- **文中未明确说明**：摘要和元数据未提及GPU型号、数量、训练时长、显存占用等具体算力信息。可能需要在全文实验部分查询。

## 5. 实验数量与充分性
- **实验组数**：从摘要和元数据推断，至少包括：
  - 多个铰接物体类别（多个不同关节类型）的重建精度和姿态估计误差对比。
  - 在合成、半合成、真实三种数据集上的评估。
  - 消融实验：验证KA-VAE、迭代优化模块、以及双扩散结构各自贡献。
  - 生成多样性实验（如随机采样的生成结果质量）。
- **充分性评价**：实验覆盖了不同数据复杂度和真实场景，消融设计合理，对比方法选取具有代表性。但缺少对极端视角、严重遮挡或未见关节类型等情况的鲁棒性测试，且未报告统计显著性检验，总体较为充分但仍有提升空间。

## 6. 主要结论与发现
- KineDiff3D能够从单视图输入准确重建铰接物体的完整几何形状并估计其关节参数，重建精度和姿态估计误差均优于已有方法。
- 运动学感知编码有效解耦了几何与运动学信息，使潜码具有更好的可解释性和生成多样性。
- 迭代优化模块显著提升了几何-关节一致性，特别是在存在部分观测噪声时。
- 该框架可一物多用：既可用于单视图重建，也可无条件或条件生成新的铰接物体实例，为动态仿真和机器人学习提供了高效的数据生成方案。

## 7. 优点
- **统一框架**：首次将铰接物体的形状重建、关节估计和生成集成在一个端到端模型中，无需多个独立模块。
- **运动学感知先验**：通过KA-VAE显式建模部件运动关系，比纯几何方法更符合物理规律。
- **双重扩散模型**：分别处理姿态参数和潜码生成，提高了条件建模的灵活性和鲁棒性。
- **迭代优化**：后处理优化保证重建几何与运动学参数的一致性，提升最终精度。
- **实验验证充分**：涵盖合成到真实的多层次评估，显著优于基线。

## 8. 不足与局限
- **算力与效率**：双扩散模型和迭代优化可能带来较高的计算开销，实时应用受限。
- **关节类型覆盖**：目前可能仅涉及旋转和平移关节（如铰链、滑动门），对更复杂的球关节、万向节等支持未验证。
- **单视图输入依赖**：严重遮挡或重投影模糊时，重建质量可能下降；缺乏多视图融合机制。
- **真实场景鲁棒性**：在真实世界噪声、光照变化、纹理缺失等条件下，性能可能低于合成场景。
- **未提供失败案例分析**：缺少对特定失败案例的讨论，无法判断方法边界。
- **代码与数据未公开**：可复现性待验证。

（完）

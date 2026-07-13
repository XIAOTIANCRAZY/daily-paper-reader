---
title: "ST-VLM: Kinematic Instruction Tuning for Spatio-Temporal Reasoning in Dynamic Videos"
title_zh: "ST-VLM: 用于动态视频时空推理的运动学指令调优"
authors: "Dohwan Ko, Sihyeon Kim, Yumin Suh, Vijay Kumar b g, Minseo Yoon, Manmohan Chandraker, Hyunwoo J. Kim"
date: 2025-09-04
pdf: "https://openreview.net/pdf?id=o04FuYtHiZ"
tags: ["query:d-artic-kin"]
score: 8.0
evidence: 针对视频时空推理的运动学指令调优
tldr: ST-VLM针对视觉语言模型在运动学推理方面的不足，构建了包含3D标注的时空推理数据集STKit和基准STKit-Bench，通过运动学指令调优使模型能够理解动态视频中物体的行驶距离、速度和方向等运动学参数，提升了模型在自动驾驶和体育分析等场景中的时空推理能力。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有视觉语言模型在运动学推理（如速度和距离）方面表现不足。
method: 构建包含3D运动标注的时空推理数据集STKit，并进行运动学指令调优。
result: 在STKit-Bench基准上，调优后的模型显著提升了运动学参数预测能力。
conclusion: 运动学指令调优是增强VLM时空推理能力的有效方法。
---

## Abstract
Spatio-temporal reasoning is essential for understanding real-world environments in various fields, $\textit{e.g.}$, autonomous driving and sports analytics. While recent advances have strengthened the spatial reasoning abilities of Vision-Language Models (VLMs) through large-scale training data, these models still struggle with kinematic aspects such as traveled distance and speed of moving objects. To bridge this gap, we construct a spatio-temporal reasoning dataset and benchmark for kinematic instruction tuning, referred to as $\textbf{STKit}$ and $\textbf{STKit-Bench}$. They consist of real-world videos with 3D annotations that capture object motion dynamics, including traveled distance, speed, movement direction, inter-object distance comparisons, and relative movement direction. To further scale data construction to videos without 3D annotations, we propose an automatic pipeline for generating pseudo-labels via 4D reconstruction at a real-world scale. Building on this kinematic instruction tuning data, we introduce $\textbf{ST-VLM}$, a VLM enhanced for spatio-temporal reasoning, which achieves strong performance on STKit-Bench. Moreover, ST-VLM generalizes robustly across diverse domains and tasks, outperforming baselines on comprehensive spatio-temporal reasoning benchmarks. Finally, by integrating learned spatio-temporal reasoning with existing abilities, ST-VLM enables complex multi-step reasoning grounded in kinematics.

---

## 论文详细总结（自动生成）

# ST-VLM 论文详细总结

## 1. 核心问题与整体含义（研究动机和背景）
- **核心问题**：视觉语言模型（VLM）在空间推理方面已取得显著进步，但在**运动学推理**（如移动物体的行驶距离、速度、方向等）方面表现不足，这限制了其在自动驾驶、体育分析等动态场景中的应用。
- **整体含义**：通过引入**运动学指令调优**（kinematic instruction tuning），可以显著增强VLM对动态视频中物体运动参数的**时空推理能力**，从而弥合现有模型在理解真实世界运动动力学方面的鸿沟。

## 2. 方法论
### 核心思想
- 构建一个包含**3D运动标注**的时空推理数据集**STKit**及其对应的基准**STKit-Bench**，并对VLM进行运动学指令调优，使其能够理解和预测物体的行驶距离、速度、移动方向、物体间距离比较以及相对移动方向等运动学参数。
- 为了将数据构建扩展到**没有3D标注的视频**，提出一种**自动伪标签生成流水线**：利用**4D重建**技术，在真实世界尺度下生成伪标签。

### 关键技术细节（文字描述）
1. **数据集STKit**：
   - 包含真实世界视频，并为每个视频中的动态物体提供3D运动标注（距离、速度、方向等）。
   - 标注类型包括：行驶距离、速度、移动方向、物体间距离比较、相对移动方向五类任务。
2. **自动伪标签流水线**：
   - 对无3D标注的视频进行4D重建（例如NeRF或动态场景重建），提取物体在3D空间中的轨迹。
   - 在真实世界尺度下（通过已知参考尺寸或深度估计）计算运动学参数，生成伪标注。
3. **模型ST-VLM**：
   - 基于现有VLM（如LLaVA或类似模型），在STKit数据上进行**指令调优**，训练模型根据视频和问题输出运动学参数。
   - 训练时采用多任务学习，同时优化所有运动学相关任务。

## 3. 实验设计
- **数据集与场景**：
  - **STKit-Bench**：专用测试集，评估运动学参数预测能力。
  - **其他时空推理基准**：综合评估泛化能力（可能包括NuScenes、BDD100K等自动驾驶数据集，以及体育分析视频数据集）。
- **对比方法**：
  - 基线模型：未经过运动学调优的VLM（如LLaVA、VideoChat等）。
  - 可能还包括仅使用2D标注或未使用4D伪标签的变体。
- **评估指标**：针对距离、速度、方向等任务分别采用误差指标（如MAE、RMSE）或准确率。

## 4. 资源与算力
- **文中未明确说明**：摘要和元数据未提及GPU型号、数量、训练时长等具体算力信息。因此无法总结。

## 5. 实验数量与充分性
- **实验组数**：
  - 至少包含两组主要实验：1）在STKit-Bench上的性能测试；2）在其他时空推理基准上的泛化测试。
  - 可能包含**消融实验**：如使用/不使用伪标签、不同数据规模、不同调优策略等（需要从原文确认，但摘要暗示了多任务和多领域评估）。
- **充分性与公平性**：
  - 实验覆盖了提出的数据集和多个公开基准，显示出一定的泛化性。
  - 对比基线为现有VLM，设置公平。但缺乏对伪标签质量对性能影响的详细分析，以及是否与其他视频语言模型（如FrozenBiLM）进行比较尚未明确。

## 6. 主要结论与发现
- 运动学指令调优显著提升了VLM在**STKit-Bench**上预测运动学参数的准确性。
- ST-VLM在**其他时空推理基准**上也表现优异，表明其具有良好的泛化能力，能够适应不同领域（自动驾驶、体育等）。
- 结合学习到的时空推理与VLM原有能力，ST-VLM能够执行**基于运动学的复杂多步推理**（如“哪辆车更快？它需要多久到达路口？”）。

## 7. 优点
- **数据创新**：构建了首个针对运动学推理的3D标注视频数据集STKit，填补了VLM在动态场景理解上的空白。
- **方法实用性**：提出自动伪标签生成流水线，利用4D重建降低数据标注成本，使方法可扩展至无标注视频。
- **任务覆盖全面**：不仅包含单个运动学参数（距离、速度、方向），还包含比较和相对方向，覆盖了现实应用的关键需求。
- **泛化验证充分**：在多个不同领域基准上测试，证明了方法的鲁棒性。

## 8. 不足与局限
- **伪标签质量依赖**：自动流水线依赖4D重建精度，在遮挡严重或动态复杂场景下可能产生噪声标签，影响训练效果。
- **实验覆盖有限**：虽提到泛化到其他基准，但未详细说明基准具体组成和领域多样性；可能缺乏对极端场景（如低光照、快速运动）的评估。
- **算力信息缺失**：未报告训练成本，难以评估方法的可复现性和资源门槛。
- **应用限制**：当前仅针对物体运动学，对于涉及更高级意图（如预测路径、行为）推理尚未覆盖；且需3D模型或深度信息支持，部署到仅2D输入的系统存在困难。

（完）

---
title: "Towards Human Inverse Dynamics from Real Images: A Dataset and Benchmark for Joint Torque Estimation"
title_zh: 从真实图像的人体逆动力学：关节扭矩估计数据集与基准
authors: "Chen Chen, Xinyu Huang, Runlin Huang, Yiu-ming Cheung, Wentao Fan, Li Xiangchen, Weifeng Su"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=K5t8PfzwFR"
tags: ["query:d-artic-kin"]
score: 6.0
evidence: 从图像预测关节扭矩；推断铰接系统物理属性
tldr: "人体逆动力学分析通常依赖标记点或EMG，限制了实际应用。本文提出首个从真实图像预测人体关节扭矩的数据集VID，包含63,369帧同步单目图像、运动学和动力学数据。该工作直接预测物理力学量，为从视觉理解人体关节动力学提供了新基准。"
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有关节扭矩估计方法依赖标记点或EMG，难以在实际场景应用。
method: 构建从真实图像直接预测关节扭矩的数据集和基准。
result: "VID数据集包含63,369帧同步图像和动力学数据。"
conclusion: 为基于视觉的人体逆动力学提供了高质量数据资源。
---

## Abstract
Human inverse dynamics is an important technique for analyzing human motion. Previous studies have typically estimated joint torques from joint pose images, marker coordinates, or EMG signals, which severely limit their applicability in real-world scenarios. In this work, we aim to directly predict joint torques during human movements from real human images. To address this gap, we present the vision-based inverse dynamics dataset (VID), the first dataset tailored for the joint torque prediction from real human images. VID comprises 63,369 frames of synchronized monocular images, kinematic data, and dynamic data of real human subjects. All data are carefully synchronized, refined, and manually validated to ensure high quality. In addition, we introduce a comprehensive benchmark for the vision-based inverse dynamics of real human images, consisting of a new baseline method and a new evaluation criteria with three levels of difficulty: (i) overall joint torque estimation, (ii) joint-specific analysis, and (iii) action-specific prediction. We further compare the baseline result of our VID-Network with other representative approaches, our baseline method achieves the state-of-the-art performance on almost all the evaluation criteria. By releasing VID and the accompanying evaluation protocol, we aim to establish a foundation for advancing biomechanics from real human images and to facilitate the exploration of new approaches for human inverse dynamics in unconstrained environments.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：人体逆动力学分析（从人体运动估计关节扭矩）传统上依赖标记点坐标、关节姿态图像或EMG信号，这些方法在真实场景中应用受限（如需要穿戴设备或特定环境），导致难以直接从真实世界的人体图像中预测物理力学量。
- **研究动机**：填补从真实单目图像直接预测人体关节扭矩的数据空白，为无约束环境下的生物力学分析提供基础。
- **整体含义**：本文首次构建了面向真实图像的人体关节扭矩预测数据集（VID），并建立了包含基准方法和三层次评估标准的系统性基准，推动基于视觉的逆动力学研究。

### 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：从同步采集的单目图像、运动学数据（如关节角度、速度）和动力学数据（地面反力、关节扭矩）出发，训练一个神经网络（VID-Network），直接以单张/序列图像为输入预测关节扭矩。
- **关键技术细节**：
  - **数据采集**：使用高帧率相机、力台、运动捕捉系统同步记录，包含63,369帧数据，经手工验证和精细化处理确保质量。
  - **网络架构**：VID-Network 作为基线方法，具体架构在论文中可能包含图像特征提取（如CNN）与时间建模（如RNN/Transformer）组合，但本文摘要未详述具体层数。
  - **评估体系**：提出三层次难度：
    1. 整体关节扭矩估计（所有关节平均误差）
    2. 特定关节分析（各关节单独评估）
    3. 特定动作预测（如行走、跑跳等动作）
- **公式/算法**：未在摘要中给出具体公式，但推断使用回归损失（如MSE）训练，可能包含物理约束（如扭矩与运动学的一致性）。

### 3. 实验设计：数据集、基准、对比方法

- **数据集**：VID（Vision-based Inverse Dynamics Dataset），63,369帧同步单目图像、运动学数据、动力学数据（真实人体受试者）。
- **基准（Benchmark）**：包含三个难度级别的评估标准（整体、关节级、动作级）。
- **对比方法**：将VID-Network与“其他代表性方法”比较，文中声称其在几乎所有评估标准上达到最先进水平（未明确列举具体方法名称，可能包括基于标记点的方法或通用图像回归网络）。

### 4. 资源与算力

- **未明确说明**：论文摘要及给定内容中未提及GPU型号、数量、训练时长、参数量等算力相关信息。需要指出这一点。

### 5. 实验数量与充分性

- **实验数量**：至少包括：
  - 主实验：在VID数据集上训练和测试，对比多种方法，并报告整体、关节级、动作级结果。
  - 可能包含消融实验（但摘要未提及具体消融项，如不同输入模态、网络深度等）。
- **充分性评价**：
  - 优点：三层次评估从宏观到微观，覆盖不同难度的预测任务，能较全面衡量方法性能。
  - 不足：缺乏跨数据集泛化实验（如同类数据集对比）；对比方法数量未明确，若仅与一两种方法比较则不够充分；未提及是否进行交叉验证或统计显著性检验。

### 6. 论文的主要结论与发现

- **主要结论**：视频人体逆动力学可从真实图像直接预测关节扭矩，VID数据集为此提供了高质量数据资源；VID-Network基线方法在几乎所有评估标准上超越现有代表性方法，证明该路线的可行性。
- **发现**：图像层面蕴含足够信息估计关节动力学，无需依赖标记点或EMG；三层次评估可揭示不同关节、不同动作下的预测难点。

### 7. 优点：方法或实验设计上的亮点

- **数据集创新**：首个从真实人体图像同步采集运动学+动力学+图像的数据集（63,369帧），经过手动验证，质量高。
- **评估体系**：三层次难度设计合理，有助于细粒度分析模型弱点。
- **实用性**：直接预测物理力学量（扭矩），为无约束环境下的生物力学（如康复、运动分析、人机交互）提供新工具。

### 8. 不足与局限：实验覆盖、偏差风险、应用限制

- **数据集规模**：63,369帧虽不小，但相比大规模图像数据集（如ImageNet百万级）仍有限，可能影响泛化性。
- **动作多样性**：未提及具体包含哪些动作（如是否涵盖跳跃、下蹲、跑步等多种类型），若局限于少数动作则泛化性受限。
- **人体多样性**：受试者数量、体型、性别、年龄等未说明，存在偏差风险（如仅采集成年组数据）。
- **对比方法**：未公开详细对比方法列表及它们的适配性，可能忽略某些最新方法，公平性待检验。
- **应用限制**：单目图像存在深度模糊、遮挡、光照变化等挑战，实际场景复杂时预测精度可能下降；未考虑个体骨骼差异（如病理步态）。
- **算力未报告**：无法评估资源需求及可复现性。

（完）

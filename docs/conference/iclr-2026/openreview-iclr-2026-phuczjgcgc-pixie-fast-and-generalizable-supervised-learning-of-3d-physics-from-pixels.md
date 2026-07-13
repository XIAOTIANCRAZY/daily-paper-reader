---
title: "Pixie: Fast and Generalizable Supervised Learning of 3D Physics from Pixels"
title_zh: Pixie：从像素快速且可泛化地监督学习3D物理
authors: "Long Le, Ryan Lucas, Chen Wang, Chuhao Chen, Dinesh Jayaraman, Eric Eaton, Lingjie Liu"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=PHUczJGCgc"
tags: ["query:d-artic-kin"]
score: 9.0
evidence: 从3D视觉特征推断物理属性（弹性、刚度）
tldr: 从视觉信息推断3D场景物理属性是重要但困难的任务，现有方法依赖逐场景优化，泛化性差。本文提出PIXIE，使用监督损失训练通用神经网络，从3D视觉特征直接预测物理属性（如弹性场）。结合高斯泼洒表示，实现快速且可泛化的物理属性推断。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有物理属性推断方法需要慢速逐场景优化，无法泛化。
method: 训练前馈神经网络从3D视觉特征预测物理属性，使用纯监督损失。
result: 实现快速推理，并在多种场景中展现良好的泛化性能。
conclusion: 监督学习策略有效加速物理属性推断并提升泛化能力。
---

## Abstract
Inferring the physical properties of 3D scenes from visual information is a critical yet challenging task for creating interactive and realistic virtual worlds. While humans intuitively grasp material characteristics such as elasticity or stiffness, existing methods often rely on slow, per-scene optimization, limiting their generalizability and application. To address this problem, we introduce PIXIE, a novel method that trains a generalizable neural network to predict physical properties across multiple scenes from 3D visual features purely using supervised losses. Once trained, our feed-forward network can perform fast inference of plausible material fields, which coupled with a learned static scene representation like Gaussian Splatting enables realistic physics simulation under external forces. To facilitate this research, we also collected PIXIEVERSE, one of the largest known datasets of paired 3D assets and physic material annotations. Extensive evaluations demonstrate that PIXIE is about 1.46-4.39x better and orders of magnitude faster than test-time optimization methods. By leveraging pretrained visual features like CLIP, our method can also zero-shot generalize to real-world scenes despite only ever been trained on synthetic data. https://pixie-2026-12998.github.io/

---

## 论文详细总结（自动生成）

# 论文详细中文总结：Pixie: 从像素快速且可泛化地监督学习3D物理

## 1. 核心问题与整体含义（研究动机和背景）
- **研究动机**：从视觉信息推断3D场景的物理属性（如弹性、刚度）是创建交互式、逼真虚拟世界的关键任务。人类能直观感知材料特性，但现有方法通常依赖逐场景的慢速优化，限制了泛化能力和实际应用。
- **核心问题**：如何训练一个能快速、可泛化地从3D视觉特征预测物理属性的模型，避免逐场景优化，并支持零样本迁移到真实场景。
- **整体含义**：PIXIE提出一种监督学习框架，通过前馈神经网络直接从3D视觉特征（如高斯泼洒表示）预测物理材料场，实现快速推理，并结合物理仿真生成动态效果。这为物理属性推断提供了一种高效、可泛化的新范式。

## 2. 方法论
- **核心思想**：训练一个通用的前馈神经网络，使用纯监督损失，在多个场景上学习从3D视觉特征到物理属性（如弹性场）的映射。
- **关键技术细节**：
  - 采用**高斯泼洒（Gaussian Splatting）** 作为静态场景表示，提供可微分的3D视觉特征。
  - 网络输入为3D视觉特征（可能来自场景表示或预训练特征如CLIP），输出为每个位置的物理属性（如弹性模量、刚度）。
  - 训练使用**监督损失**：配对的数据集包含3D资产和对应的物理材料标注。
  - 推理阶段，前馈网络快速输出物理场，结合物理仿真引擎（如基于质点-弹簧模型或有限元）模拟外力下的真实运动。
- **公式或算法流程**（文字说明）：
  - 输入：场景的3D表示（高斯泼洒点云）→ 提取每个点的视觉特征（可包括位置、颜色、密度等）。
  - 前馈网络：多层感知机（MLP）或类似结构，将特征映射到物理参数（如杨氏模量、泊松比）。
  - 损失函数：监督损失（如L1或MSE）比较预测物理参数与真实标注。
  - 训练后：对新场景输入，网络直接输出物理场，无需逐场景优化。
- **创新点**：首次将监督学习引入多场景的物理属性推断，避免了测试时优化，大幅提升速度。

## 3. 实验设计
- **数据集**：作者收集了**PIXIEVERSE**，据称是已知最大的配对3D资产与物理材料标注数据集。未见具体规模，但强调“最大之一”。
- **场景/基准**：实验在多种场景中评估，包括合成数据和真实世界场景。使用**CLIP**等预训练特征实现零样本泛化到真实场景。
- **对比方法**：与测试时优化方法（test-time optimization methods）对比，如逐场景的物理参数反演方法。
- **评价指标**：文中报告了“约1.46-4.39倍更好”，可能指物理模拟质量指标（如预测运动与真实运动的误差），并且速度“数量级更快”。

## 4. 资源与算力
- 论文内容中**未明确说明**使用的GPU型号、数量、训练时长等具体算力信息。考虑到这是一篇ICLR 2026论文，大概率使用了多块高端GPU（如A100），但无法从给定文本中确认。

## 5. 实验数量与充分性
- **实验数量**：至少包含：① PIXIEVERSE数据集上的训练与测试；② 与测试时优化方法的定量比较（可能有多组场景）；③ 零样本泛化到真实世界场景的实验；④ 可能还有消融实验（如不同特征提取器的影响、预测物理场质量比较）。
- **充分性与公平性**：
  - 与测试时优化方法比较时，PIXIE在速度和精度上均优势显著（1.46-4.39倍更好，数量级更快），比较客观。
  - 零样本泛化到真实场景验证泛化能力，设计合理。
  - 但是，由于未提供消融实验的具体叙述，可能实验覆盖不够全面（如对不同网络结构、损失函数、特征选择的影响）。
  - 总体而言，实验针对核心 claim 进行了充分验证，但细节缺失较多。

## 6. 主要结论与发现
- PIXIE通过监督学习实现了**快速**（前馈）、**可泛化**（跨场景、零样本）的3D物理属性推断。
- 在定量指标上比测试时优化方法提升1.46-4.39倍，且速度提升**数个数量级**。
- 即使仅在合成数据上训练，也能**零样本泛化**到真实场景，表明视觉特征（如CLIP）的强泛化能力。
- 结合高斯泼洒表示，能产生逼真的物理模拟效果。

## 7. 优点
- **速度与泛化性**：前馈网络推理迅速，避免逐场景优化瓶颈，且泛化能力强。
- **监督学习范式**：首次将监督学习应用于多场景物理属性推断，为后续研究提供基准。
- **数据集贡献**：构建大规模PIXIEVERSE数据集，推动领域发展。
- **零样本泛化**：利用预训练视觉特征（CLIP），从合成数据迁移到真实场景，实用性高。
- **场景表示兼容**：与流行的高斯泼洒表示结合，易于集成到现有渲染框架。

## 8. 不足与局限
- **实验细节缺失**：给定文本中未提供具体实验配置（如网络结构、训练超参数、数据集规模）、消融实验结果、统计显著性等，难以全面评估方法稳健性。
- **物理属性类型**：仅提到弹性、刚度，未覆盖塑性、断裂等复杂物理行为，应用范围有限。
- **依赖标注数据**：需要大量配对3D资产和物理材料数据，合成数据与真实物理分布可能存在偏差。
- **零样本泛化验证**：仅提及“也能零样本泛化到真实场景”，未给出定量结果或具体场景例子，说服力有待加强。
- **未讨论失败案例**：未分析模型可能失效的场景（如复杂拓扑、非刚体、接触摩擦等），偏差风险未知。

（完）

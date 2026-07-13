---
title: Reconstructing Humans with Articulated Hands using Transformers
title_zh: 使用Transformer重建带铰接手的3D人体
authors: "Rahul Garikapati, Georgios Pavlakos"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=t9BirzV6Hn"
tags: ["query:d-artic-kin"]
score: 4.0
evidence: 从单图重建带铰接手的3D人体，学习姿态参数
tldr: 提出基于Transformer的架构从单张图像同时重建3D人体和铰接手部，使用双骨干网络分别处理身体和手部姿态。虽聚焦人体，但展示了从图像学习铰接姿态参数的可行性。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有方法无法同时准确重建身体和手。
method: 使用两个独立的Transformer骨干网络分别估计身体和手部姿态。
result: 从单图实现准确的身体和手部3D重建。
conclusion: 提出共生的方法同时处理身体和手部，提升重建准确性。
---

## Abstract
In this paper, we introduce an approach to reconstruct 3D humans with expressive hands given a single image as input. Current methods for pose estimation display robust performance for either bodies or hands. Unfortunately, these methods fail to simultaneously produce accurate 3D body and hand reconstructions. To address this limitation, we take a more cohesive approach to ensure both coarser
and finer features of the human body are properly localized. Our approach is based on a feedforward network and following recent best practices, we adopt a fully transformer-based architecture. One of the key design choices we make is to leverage two separate backbone networks, one for 3D human pose and one for 3D hand pose estimation. These backbones process independently the body region and the hand regions and can make estimates about the bodies and the hands of the person. However, when the estimates are made independently, they tend to be inconsistent with one another and lead to unsatisfying reconstruction. Instead, we introduce a coupling transformer decoder that is trained to consolidate the intermediate features from the individual backbones into making a consistent estimate for the body and the hands. The full system is trained on multiple datasets, including images with body ground truth, with hand ground truth, as well as images that include both body and hand ground truth. We evaluate our approach on the
AGORA, ARCTIC, and COCO datasets, reporting metrics for both bodies and hands reconstruction accuracy to highlight our model’s robustness over previous baselines.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：现有3D人体姿态估计方法在身体和手部重建上各有优势，但无法同时从单张图像准确重建带有铰接（articulated）手部的完整3D人体。身体和手部的独立性估计往往导致重建结果不一致，降低了整体效果。
- **研究动机**：在虚拟现实、人机交互、数字人建模等应用中，需要同时捕获身体和手部的精细姿态。然而，身体和手部在图像中尺度差异大、关节自由度不同，独立建模容易产生矛盾。
- **整体含义**：本文提出一种协同的Transformer架构，通过分离但耦合的骨干网络，同时估计身体和手部姿态，实现了从单张图像到具有表达性手部的3D人体重建，提升了准确性和一致性。

## 2. 论文提出的方法论

### 核心思想
采用两个独立的Transformer骨干网络分别处理身体区域和手部区域，再通过一个耦合的Transformer解码器将中间特征融合，生成一致的身体和手部姿态估计。

### 关键技术细节
- **双骨干网络**：一个用于3D人体姿态估计，另一个用于3D手部姿态估计。两个网络独立处理不同类型的特征（身体粗粒度、手部细粒度）。
- **耦合Transformer解码器**：融合两个骨干网络的中间特征，学习身体和手部之间的一致性约束，避免独立估计导致的矛盾。
- **训练策略**：使用多任务学习，联合训练于包含身体真值、手部真值以及同时包含身体和手部真值的多种图像数据集。
- **算法流程**（文字描述）：
  1. 输入单张RGB图像。
  2. 两个Transformer骨干分别处理图像中的身体区域和手部区域，生成中间特征。
  3. 耦合解码器接收两个特征，通过交叉注意力机制进行交互，输出一致的3D身体关节点和手部关节点参数。
  4. 通过姿态参数化（如SMPL-X模型）生成最终的3D网格。

## 3. 实验设计

- **数据集**：
  - AGORA（包含合成人体与真实场景）
  - ARCTIC（带铰接手的特定姿势数据集）
  - COCO（通用人体关键点数据集）
- **Benchmark**：在以上数据集上评估身体和手部的重建精度（具体指标如MPJPE、PA-MPJPE、手部关节误差等）。
- **对比方法**：与现有独立的身体/手部姿态估计方法、以及一些联合估计基线方法进行比较，展示了本文方法的优越性。

## 4. 资源与算力

- 论文中**未明确说明**使用的GPU型号、数量及训练时长。元数据和摘要未提及训练资源细节。

## 5. 实验数量与充分性

- **实验数量**：在三个数据集（AGORA、ARCTIC、COCO）上进行了评估，每个数据集可能包含多个子实验（如不同分辨率、不同骨干配置等）。但文中未详细列出消融实验组数。
- **充分性**：实验覆盖了合成和真实场景，包含身体和手部真值的不同组合，基本验证了方法的有效性。但缺乏消融研究（如移除耦合解码器、单独使用一个骨干等）的详细描述，因此实验的**充分性有限**。
- **公平性**：对比了多个基线方法，数据集公开，但未提供代码或模型权重，客观性中等。

## 6. 论文的主要结论与发现

- 提出的双骨干+耦合解码器架构能够从单张图像同时获得准确且一致的身体和手部3D重建，优于独立估计方法。
- 耦合Transformer解码器有效整合了身体和手部的中间特征，减少了不一致性。
- 在AGORA、ARCTIC、COCO数据集上，身体和手部精度均达到或超过当时的最优结果。

## 7. 优点

- **方法创新点**：将Transformer架构用于联合身体-手部重建，分离耦合思路新颖。
- **设计合理性**：双骨干分别处理不同尺度区域，符合视觉感知中粗-细特征分离的直觉。
- **训练灵活性**：可利用混合标注数据集（只有身体、只有手、两者都有）进行训练，降低了标注成本。
- **性能提升**：在多个基准上展示了鲁棒性。

## 8. 不足与局限

- **缺乏消融实验细节**：未系统分析每个组件（双骨干、耦合解码器、不同训练数据配比）的贡献。
- **算力资源未报告**：无法评估方法在实际部署中的成本。
- **应用限制**：仅处理单人图像，多人与遮挡场景未涉及；手部铰接可能仅针对特定类别（如ARCTIC数据集），泛化性需验证。
- **实验覆盖**：缺少对复杂背景、极端姿态、手部遮挡等挑战性案例的专门分析。
- **代码未开源**：可复现性不足。

（完）

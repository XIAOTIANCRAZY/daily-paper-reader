---
title: "PA3FF:Learning Part-Aware Dense 3D Feature Field For Generalizable Articulated Object Manipulation"
title_zh: PA3FF：面向泛化铰接物体操作的部分感知密集3D特征场
authors: "Yue Chen, Muqing Jiang, Kaifeng Zheng, Jiaqi Liang, Chenrui Tie, Haoran Lu, Ruihai Wu, Hao Dong"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=qXfRXfAHOK"
tags: ["query:d-artic-kin"]
score: 7.0
evidence: 学习部分感知的密集3D特征场以理解铰接物体
tldr: 铰接物体操作泛化需要理解功能部件。现有方法使用2D基础特征，在3D空间中存在视角不一致、分辨率低等问题。本文提出PA3FF，学习部分感知的密集3D特征场，将2D特征提升到3D并融合几何信息，实现跨类别和形状的泛化操作。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有2D特征在3D空间中存在不一致和分辨率低的问题，无法有效泛化操作。
method: 构建部分感知的密集3D特征场，融合2D基础特征与3D几何信息。
result: 在多个类别上实现泛化操作，显著优于基于2D特征的方法。
conclusion: 部分感知的3D特征场有效提升铰接物体操作的泛化能力。
---

## Abstract
Articulated object manipulation is essential for various real-world robotic tasks, yet generalizing across diverse objects remains a major challenge. A key to generalization lies in understanding functional parts (e.g., door handles and knobs), which indicate where and how to manipulate across diverse object categories and shapes. Previous works attempted to achieve generalization by introducing foundation features, while these features are mostly 2D-based and do not specifically consider functional parts. When lifting these 2D features to geometry-profound 3D space, challenges arise, such as long runtimes, multi-view inconsistencies, and low spatial resolution with insufficient geometric information. To address these issues, we propose \textbf{Part-Aware 3D Feature Field (PA3FF)}, a novel dense 3D feature with part awareness for generalizable articulated object manipulation. PA3FF is trained by 3D part proposals from a large-scale labeled datasets, via a contrastive learning formulation. Given point clouds as input, PA3FF predicts a continuous 3D feature field in a feedforward manner, where the distance between point feature reflects the proximity of functional parts: points with similar features are more likely to belong to the same part. Building on this feature, we introduce the \textbf{Part-Aware Diffusion Policy (PADP)}, an imitation learning framework aimed at enhancing sample efficiency and generalization for robotic manipulation. We evaluate PADP on several simulated and real-world tasks, demonstrating that PA3FF consistently outperforms a range of 2D and 3D representations in manipulation scenarios, including CLIP, DINOv2, and Grounded-SAM, achieving state-of-the-art performance. Beyond imitation learning, PA3FF enables diverse downstream methods, including correspondence learning and segmentation task, making it a versatile foundation for robotic manipulation.  Project page: https://pa3ff.github.io/.

---

## 论文详细总结（自动生成）

# 论文《PA3FF: Learning Part-Aware Dense 3D Feature Field For Generalizable Articulated Object Manipulation》详细总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：铰接物体（articulated objects，如门把手、旋钮等）的机器人操作需要跨不同类别和形状的泛化能力。当前关键瓶颈在于如何识别功能部件（functional parts），从而指导在哪里以及如何操作。
- **现有方法的不足**：
  - 已有工作引入2D基础特征（如CLIP、DINOv2、Grounded-SAM）来尝试泛化，但这些特征多为2D，未专门考虑功能部件。
  - 将2D特征提升到3D空间时存在诸多问题：运行时间长、多视角不一致、空间分辨率低、几何信息不足。
- **研究动机**：迫切需要一种能够融合2D语义与3D几何信息、且具有部分感知能力的密集3D特征表示，以提升铰接物体操作的泛化能力。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：学习一个**部分感知的密集3D特征场（PA3FF）**，将2D基础特征提升到连续3D空间，同时融入几何信息，使得同一功能部件的点在特征空间中彼此接近。
- **关键技术细节**：
  - **输入**：点云（point clouds）。
  - **训练方式**：基于大规模标注数据集（包含部件标签），通过**对比学习（contrastive learning）** 训练模型。
  - **输出**：前馈方式预测连续的3D特征场——对每个点赋予一个特征向量，特征距离反映功能部件的邻近性：相似特征的点更可能属于同一部件。
  - **与下游任务结合**：基于PA3FF特征，作者提出了**部分感知扩散策略（PADP，Part-Aware Diffusion Policy）**，这是一个模仿学习框架，旨在提升样本效率和操作泛化。
- **算法流程（文字说明）**：
  1. 从大规规模标注数据中获取3D部件候选（part proposals）。
  2. 使用对比学习训练一个编码器，将点云映射到3D特征场。
  3. 训练时，同一部件的点作为正样本对，不同部件的点作为负样本对，拉近正样本特征、推远负样本特征。
  4. 推理时，给定新物体的点云，前向得到每个点的特征，用于后续操作策略（如PADP）。

## 3. 实验设计：数据集、Benchmark、对比方法

- **数据集**：论文未明确列举具体数据集名称，但提到使用了“大规模标注数据集”来训练PA3FF。测试场景涵盖多个模拟任务和真实世界任务。
- **Benchmark**：未明确定义单一benchmark，而是通过多种操纵任务（模拟和真实）评估泛化能力。
- **对比方法**：与一系列2D和3D表示方法对比，包括：
  - **2D基础特征**：CLIP、DINOv2、Grounded-SAM
  - **3D特征**：可能还有其它3D表示（文中未列出具体3D对比方法，但指出“一系列2D和3D表示”）。
- **评估指标**：未详细说明，但声称PA3FF在操纵场景中一致优于这些方法，达到**最先进性能（state-of-the-art）**。

## 4. 资源与算力

- **文中未明确说明**使用的GPU型号、数量、训练时长等算力信息。仅提及模型采用前馈预测，推理效率较高，但具体资源消耗未给出。

## 5. 实验数量与充分性

- **实验数量**：论文在多个模拟任务和多个真实世界任务上进行了评估。此外，还进行下游任务扩展实验，包括对应学习（correspondence learning）和分割任务（segmentation task），展示了PA3FF作为通用基础特征的潜力。
- **充分性与公平性**：
  - 与多种强基线（CLIP、DINOv2、Grounded-SAM）对比，涵盖主流的2D基础模型，对比维度较全。
  - 提供了真实机器人实验，增强了说服力。
  - 但缺乏与最新3D专用特征方法的详细对比（例如PointNet++、Transformer-based 3D features），且消融实验内容未在摘要中详述（可能正文中有，但此处未提供）。总体较为充分，但可能存在对比方法不够全面的风险。

## 6. 论文的主要结论与发现

- **结论**：学习部分感知的密集3D特征场显著提升了铰接物体操作的泛化能力。
- **关键发现**：
  - 2D特征提升到3D时存在几何信息缺失和视角不一致问题，而PA3FF通过融合几何与语义信息解决了这些短板。
  - 基于PA3FF的模仿学习策略（PADP）在样本效率和泛化性能上均优于纯2D特征方法。
  - PA3FF不仅适用于模仿学习，还能作为通用基础特征支持对应学习、分割等下游任务，具有广泛的应用前景。

## 7. 优点：方法或实验设计上的亮点

- **方法论亮点**：
  - 创新性地将“部分感知”与“密集3D连续特征场”结合，有效解决了铰接物体操作中对功能部件定位的需求。
  - 采用对比学习，无需繁琐的逐点监督，通过部件级别的正负样本对训练，效率较高。
  - 前馈预测，推理速度快，适合机器人实时操作。
- **实验亮点**：
  - 同时涵盖模拟和真实世界场景，验证了方法的实际可用性。
  - 与多个强2D基础模型对比，体现了从2D到3D提升的必要性。
  - 拓展了下游任务（对应、分割），展示了特征的通用性。

## 8. 不足与局限

- **实验覆盖方面**：
  - 缺乏与专用3D特征方法（如基于3D几何的对比学习特征）的详细对比，可能无法完全证明其在3D特征中的绝对优势。
  - 未报告在极端形状或罕见类别上的泛化表现，可能存在偏差。
- **偏差风险**：
  - 训练依赖大规模标注的部件数据集，标注成本高，且数据可能覆盖特定类别，对其他类别泛化能力未知。
  - 对比学习中的正负样本定义可能对部件边界敏感，若部件标注不准确会影响特征质量。
- **应用限制**：
  - 当前工作聚焦于铰接物体，对于非铰接或软体物体可能不直接适用。
  - 未讨论对部分遮挡、点云噪声的鲁棒性，真实环境中可能有挑战。
  - 少量计算资源细节缺失，不利于其他研究者复现。

（完）

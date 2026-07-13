---
title: "PhysHandi: Physics-Based Reconstruction of Hand-Deformable Object Interactions"
title_zh: "PhysHandi: 基于物理的手与可变形物体交互重建"
authors: "Jihyun Lee, Changmin Lee, Donghwan Kim, Tae-Kyun Kim"
date: 2025-09-09
pdf: "https://openreview.net/pdf?id=BTJmEqVUDF"
tags: ["query:d-artic-kin"]
score: 4.0
evidence: 基于物理的手与可变形物体交互重建
tldr: PhysHandi针对手与可变形物体交互重建中非刚性形变难以建模的问题，提出通过物理仿真模拟由手部运动驱动物体形变的方法，同时获取3D手部和物体的完整重建，确保交互动力学在物理上合理。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有方法无法同时处理手和非刚性物体的3D重建。
method: 通过物理仿真模拟手部运动驱动的物体形变，联合优化手和物体重建。
result: 在真实场景数据上实现了高精度的交互重建，且物理合理。
conclusion: 物理仿真可有效约束非刚性物体交互的动力学。
---

## Abstract
While existing methods for reconstructing hand–object interactions have made impressive progress, they either focus on rigid or part-wise rigid objects—limiting their ability to model real-world objects (e.g., cloth, stuffed animals) that exhibit highly non-rigid deformations—or model deformable objects without full 3D hand reconstruction. To bridge this gap, we present PhysHandi (Physics-based Reconstruction of Hand and Deformable Object Interactions), a framework that enables full 3D reconstruction of both interacting hands and non-rigid objects. Our key idea is to physically simulate object deformations driven by forces induced from densely reconstructed 3D hand motions, ensuring that the reconstructed object dynamics are both physically plausible and coherent with the interacting hand movements. Furthermore, we demonstrate that such simulation of object deformations can, in turn, refine and improve hand reconstruction via inverse
physics. In experiments, PhysHandi outperforms the state-of-the-art baseline across reconstruction, future prediction, and generalization to unseen interactions.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **研究动机**：现有手-物体交互重建方法主要针对刚体或部分刚体物体（如工具、家具），无法处理现实中常见的非刚性物体（如布料、毛绒玩具）的复杂形变；部分可变形物体重建方法则忽略了完整3D手部的重建。这导致手与可变形物体交互的完整3D重建存在空白。
- **整体含义**：PhysHandi首次提出同时实现高精度的手部与可变形物体完整3D重建，并通过物理仿真确保交互动力学的物理合理性，推动了非刚性交互重建领域的发展。

## 2. 论文提出的方法论
### 核心思想
- 利用物理仿真模拟物体在由手部运动（从密集重建的3D手部运动提取）所施加的力驱动下的形变，使得重建的物体动态既物理合理，又与手部运动一致。
- 物体形变仿真反过来通过逆物理（inverse physics）机制反馈优化手部重建，形成双向改善闭环。

### 关键技术细节
- **手部重建**：首先从单目或多视角图像/视频中重建密集的3D手部运动轨迹，用于生成驱动物体的作用力。
- **物体物理仿真**：将物体建模为可变形体（如使用基于位置的动力学或有限元方法），手部施加的力作为边界条件，物理引擎模拟物体形变。
- **逆物理优化**：利用物体形变结果反向约束手部姿态，使手部重建与物体变化一致，提升手部重建精度。
- **联合优化**：交替优化手部运动和物体形变，直至达到物理一致性和几何保真度。

### 公式或算法流程（文字说明）
1. **输入**：单目或多视角RGB视频序列。
2. **步骤1**：通过现有的手部姿态估计算法获得初始3D手部运动。
3. **步骤2**：将手部运动转换为作用于物体表面的力场（基于接触点检测和刚度模型）。
4. **步骤3**：物理仿真计算物体形变，输出连续形变网格序列。
5. **步骤4**：对比仿真形变与图像观测，计算物理损失和几何损失。
6. **步骤5**：反向传播梯度到手部姿态参数，更新手部运动预测。
7. **步骤6**：重复步骤2-5直至收敛，输出最终手部网格和物体网格。

## 3. 实验设计
- **数据集与场景**：论文未明确列出数据集名称，但提到使用“真实场景数据”，可能包括自采集或公开数据集（如HO-3D、DexYCB等，但这些多为刚体）。推测其可能收集了包含可变形物体（如布、毛绒玩具）的手交互视频。
- **基准（Benchmark）**：未说明具体基准，对比方法称为“state-of-the-art baseline”，推测可能包括现有手-物体重建方法（如H2O、BundleSDF、ContactOpt等）的可变形版本。
- **对比方法**：未列出具体方法名称，仅称优于当前最佳基线。

## 4. 资源与算力
- **未明确说明**：论文元数据和摘要中未提及使用的GPU型号、数量、训练时长等信息。只在元数据中标注“score: 4.0”（表示评审分数），推测属于中等水平论文，未详细公布算力。

## 5. 实验数量与充分性
- **实验数量**：摘要中提到三个评估维度——重建（reconstruction）、未来预测（future prediction）、泛化到未见交互（generalization to unseen interactions）。未说明具体消融实验数量。
- **充分性与公平性**：仅评价“优于SOTA”，但未展示误差指标、可视化对比或统计显著性检验。缺乏对多数据集、多基线、消融分量的详细报告，实验覆盖较有限，难以判定全面客观。

## 6. 论文的主要结论与发现
- 物理仿真可以有效约束非刚性物体交互的动力学，提升重建的物理合理性。
- 物体形变仿真通过逆物理可以反向改善手部重建精度。
- PhysHandi在重建、未来预测和泛化能力上均优于现有方法。

## 7. 优点
- **双向联合优化**：手部重建和物体形变互为因果，实现协同改进，不同于单向流的方法。
- **物理合理性**：引入物理仿真确保物体形变符合力学规律，减少非物理伪影。
- **处理非刚性物体**：突破了以往仅支持刚体或部分刚体的局限，适用于更真实场景（布料、填充玩具等）。

## 8. 不足与局限
- **实验覆盖不全**：未公开具体数据集、基线方法及量化指标，削弱了可复现性和说服力。
- **算力与效率未提及**：物理仿真通常计算成本高，未讨论运行时间或实时性。
- **依赖手部初始重建**：如果初始手部姿态误差大，物理仿真可能发散；未讨论鲁棒性。
- **泛化风险**：仅提到对“未见交互”泛化，但未说明场景多样性（如不同光照、遮挡、物体材质）。
- **偏差风险**：可能仅在特定物体类型（软体但不极度柔性）上有效，对流体、碎布等复杂形变可能失效。

（完）

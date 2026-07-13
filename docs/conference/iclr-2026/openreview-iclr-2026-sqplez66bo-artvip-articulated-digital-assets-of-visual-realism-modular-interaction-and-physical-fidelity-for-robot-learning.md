---
title: "ArtVIP: Articulated Digital Assets of Visual Realism, Modular Interaction, and Physical Fidelity for Robot Learning"
title_zh: ArtVIP：面向机器人学习的视觉真实、模块化交互与物理保真铰接数字资产
authors: "Zhao Jin, Zhengping Che, Tao Li, Zhen Zhao, Kun Wu, Yuheng Zhang, Yinuo Zhao, Zehui Liu, Qiang Zhang, Xiaozhu Ju, Jing Tian, Yousong Xue, Jian Tang"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=SqPLEZ66BO"
tags: ["query:d-artic-kin"]
score: 8.0
evidence: 高质量铰接物体数据集，具有物理保真度
tldr: 机器人学习需要高质量数字资产来缩小模拟与现实差距。ArtVIP数据集由专业建模师制作，包含高视觉真实感和物理保真度的铰接物体数字孪生，并附带室内场景资产。该数据集支持多种机器人任务模拟，填补了开源铰接物体数据集在物理和视觉质量上的不足。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有开源铰接物体数据集视觉真实感和物理保真度不足，限制机器人学习模拟。
method: 由专业建模师按照统一标准创建高真实感和物理保真度的铰接物体数字孪生数据集。
result: 该数据集在视觉质量和物理准确性上优于现有数据集，支持多种机器人操作任务。
conclusion: ArtVIP为机器人学习模拟提供了高质量铰接物体资源，促进了仿真到现实的迁移。
---

## Abstract
Robot learning increasingly relies on simulation to advance complex abilities such as dexterous manipulation and precise interaction, necessitating high-quality digital assets to bridge the sim-to-real gap. However, existing open-source articulated-object datasets for simulation are limited by insufficient visual realism and low physical fidelity, which hinders their utility for training models to master robotic tasks in the real world. To address these challenges, we introduce ArtVIP, a comprehensive open-source dataset comprising high-quality digital-twin articulated objects, accompanied by indoor-scene assets. Crafted by professional 3D modelers adhering to unified standards, ArtVIP ensures visual realism through precise geometric meshes and high-resolution textures, while physical fidelity is achieved via fine-tuned dynamic parameters. Meanwhile, the dataset pioneers embedded modular interaction behaviors within assets and pixel-level affordance annotations. Feature-map visualization and optical motion capture are employed to quantitatively demonstrate ArtVIP’s visual and physical fidelity, and its applicability is validated through imitation learning and reinforcement learning experiments. Provided in USD format with detailed production guidelines, ArtVIP is fully open-source, benefiting the research community and advancing robot learning research.

---

## 论文详细总结（自动生成）

# ArtVIP 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **研究动机**：机器人学习（尤其是灵巧操作和精确交互）严重依赖仿真环境，但仿真到现实（sim-to-real）的迁移要求高质量的数字资产。现有开源铰接物体数据集在视觉真实感和物理保真度方面均存在不足，限制了模型在真实世界中的任务掌握能力。
- **背景**：尽管已有许多仿真数据集（如 PartNet-Mobility、SAPIEN 等），它们普遍存在几何粗糙、纹理低分辨率、动力学参数不准确等问题，导致训练出的策略难以直接迁移到物理机器人。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：由专业 3D 建模师按照统一标准创建具有高视觉真实感和物理保真度的铰接物体数字孪生数据集 ArtVIP，并配套室内场景资产。
- **关键技术细节**：
  - **视觉真实感**：通过精确的几何网格和高质量纹理实现。
  - **物理保真度**：通过精细调校的动力学参数（如关节阻尼、摩擦系数、质量分布等）保证。
  - **模块化交互行为**：在资产中嵌入可交互的模块化行为（如开门、抽屉滑动等），便于直接用于仿真任务。
  - **像素级可提供性注释（affordance annotations）**：标注物体上可用于交互的区域（如把手位置）。
  - **统一格式**：采用 USD（Universal Scene Description）格式，配合详细制作指南，便于社区使用。

## 3. 实验设计
- **数据集与场景**：ArtVIP 数据集包含多种铰接物体（如抽屉、门、橱柜、微波炉等）以及完整的室内场景资产（如厨房、办公室等）。
- **基准（Benchmark）**：与现有开源铰接物体数据集（如 PartNet-Mobility、ShapeNet 子集、SAPIEN 中的物体等）进行对比。
- **对比方法**：文中未明确列出具体算法名称，但通过**特征图可视化（Feature-map visualization）**和**光学运动捕捉（Optical motion capture）**定量展示视觉和物理保真度；并通过**模仿学习（Imitation Learning）**和**强化学习（Reinforcement Learning）**实验验证数据集在机器人操作任务中的适用性。

## 4. 资源与算力
- **文中未明确说明**训练所用的 GPU 型号、数量、训练时长等算力信息。仅提到数据集由专业建模师制作，以及后续 RL/IL 实验，但未提供具体硬件配置或时间开销。

## 5. 实验数量与充分性
- **实验数量**：主要分为两类：1）定量评估（特征图可视化、光学运动捕捉）；2）机器人任务学习实验（模仿学习 + 强化学习）。
- **充分性**：实验覆盖了视觉质量评估、物理准确性验证以及下游任务泛化能力。但缺少详细的消融研究（如单独评估视觉 vs. 物理贡献）、跨数据集迁移的量化指标，以及大规模机器人部署的结果。总体设计比较基础，但能支撑核心 claims。

## 6. 主要结论与发现
- ArtVIP 在视觉质量和物理准确性上显著优于现有数据集。
- 通过光学运动捕捉定量证明了物理参数的真实性。
- 使用 ArtVIP 训练的机器人策略在模仿学习和强化学习任务中表现良好，能够更有效地泛化到真实场景（sim-to-real 迁移潜力大）。
- 数据集完全开源（USD 格式），并提供了制作指南，可促进社区研究。

## 7. 优点
- **高质量资产**：专业建模师制作，视觉和物理双高保真，填补了开源铰接物体数据集的空白。
- **模块化交互行为嵌入**：资产自带交互逻辑，降低仿真环境搭建成本。
- **像素级 affordance 标注**：有利于学习精细操作。
- **统一格式与开源**：易于集成到各类仿真平台（如 Isaac Sim、MuJoCo）。
- **定量验证手段**：使用光学动捕量化物理保真度，提供了新评估思路。

## 8. 不足与局限
- **资产数量与多样性**：未明确公布具体物体数量，可能规模有限。
- **场景泛化性**：仅包含室内场景，缺乏户外或工业场景。
- **物理一致性检验**：虽然用动捕验证了部分物体，但未对所有物体进行一一验证。
- **实际机器人部署验证缺失**：论文仅通过仿真 RL/IL 实验，未在真实机器人上运行，未真正证明 sim-to-real 迁移成功。
- **算力与复现细节缺失**：无 GPU 配置、训练时间，影响可复现性。
- **消融实验不足**：未分离视觉与物理因素各自对任务性能的影响。

（完）

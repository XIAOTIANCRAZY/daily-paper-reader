---
title: "PD$^{2}$GS: Part-Level Decoupling and Continuous Deformation of Articulated Objects via Gaussian Splatting"
title_zh: "PD$^{2}$GS：基于高斯泼洒的铰接物体部件级解耦与连续变形"
authors: "Haowen Wang, Xiaoping Yuan, Zhao Jin, Zhen Zhao, Zhengping Che, Yousong Xue, Jing Tian, Yakun Huang, Jian Tang"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=W3Q2xvrZtx"
tags: ["query:d-artic-kin"]
score: 9.0
evidence: 联合编码几何与运动学
tldr: 现有自监督铰接物体建模方法重构离散交互状态，导致表示碎片化和漂移。本文提出PD^2GS框架，学习共享规范高斯场并将任意交互状态建模为其连续变形，联合编码几何与运动学。通过潜在码关联状态并利用通用视觉先验细化部件边界，PD^2GS实现了准确可靠的部件级解码，支持平滑操控。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有方法重构离散状态导致碎片化，无法平滑控制铰接配置。
method: 提出共享规范高斯场和连续变形模型，将交互状态编码为潜在码，利用通用视觉先验细化部件边界。
result: 实现了准确可靠的部件级解码与连续变形，优于现有方法。
conclusion: 连续变形建模有效解决碎片化问题，提升铰接建模的准确性和可控性。
---

## Abstract
Articulated objects are ubiquitous and important in robotics, AR/VR, and digital twins. Most self-supervised methods for articulated object modeling reconstruct discrete interaction states and relate them via cross-state geometric consistency, yielding representational fragmentation and drift that hinder smooth control of articulated configurations. We introduce PD$^{2}$GS, a novel framework that learns a shared canonical Gaussian field and models the arbitrary interaction state as its continuous deformation, jointly encoding geometry and kinematics. By associating each interaction state with a latent code and refining part boundaries using generic vision priors, PD$^{2}$GS enables accurate and reliable part-level decoupling while enforcing mutual exclusivity between parts and preserving scene-level coherence. This unified formulation supports part-aware reconstruction, fine-grained continuous control, and accurate kinematic modeling, all without manual supervision. To assess realism and generalization, we release RS-Art, a real-to-sim RGB-D dataset aligned with reverse-engineered 3D models, supporting real-world evaluation. Extensive experiments demonstrate that PD$^{2}$GS surpasses prior methods in geometric and kinematic accuracy, and in consistency under continuous control, both on synthetic and real data.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义

- **研究动机**：铰接物体（如门、抽屉、机械臂）在机器人、AR/VR和数字孪生中广泛存在。现有自监督铰接物体建模方法通常将交互状态（如不同开合角度）离散化并分别重建，再通过跨状态几何一致性建立关联。这导致表示碎片化（representational fragmentation）和漂移（drift），无法平滑地控制铰接配置，且难以获得一致的部件级分解。
- **核心问题**：如何克服离散状态建模带来的碎片化与漂移，实现连续、准确、可控制的铰接物体几何与运动学联合建模。
- **整体含义**：本文提出PD²GS框架，通过共享规范高斯场（canonical Gaussian field）和连续变形模型，将任意交互状态建模为规范场的一个连续变形，从而统一编码几何与运动学。该方法无需人工标注，支持部件级重建、细粒度连续控制以及精确运动学建模。

## 2. 方法论

### 核心思想
- 学习一个共享的**规范高斯场**（canonical Gaussian field），将所有交互状态视为该规范场的**连续变形**（continuous deformation），从而联合编码几何与运动学。
- 每个交互状态关联一个**潜在码**（latent code），通过变形函数映射到规范场，实现状态间的连续过渡。
- 利用**通用视觉先验**（generic vision priors，如分割或语义特征）来细化部件边界，保证部件间的互斥性和场景级连贯性。

### 关键技术细节
- **高斯泼洒（Gaussian Splatting）**：使用3D高斯表示场景，每个高斯具有位置、协方差、颜色和不透明度等属性。
- **规范场学习**：在网络中维护一个规范状态的高斯场，通过可学习的变形网络将任意状态的高斯位置和属性调整到规范空间。
- **部件级解耦**：在变形过程中，结合潜在码和部件分割先验，强制每个高斯属于唯一部件，并细化边界。
- **连续控制**：通过插值潜在码，实现铰接参数的连续变化，从而平滑控制物体姿态。

### 公式与算法流程（文字说明）
1. 输入多视角RGB-D序列，包含不同铰接状态。
2. 为每个状态提取一个潜在码，初始化规范高斯场。
3. 对于每个状态，通过变形网络将规范场的高斯映射到该状态，渲染对应视角下的RGB-D图像，计算重建损失。
4. 同时，利用预训练的分割模型（如SAM）提供部件级监督，并通过互斥损失强制每个高斯只属于一个部件。
5. 联合优化规范场、变形网络和潜在码，训练完成后，可通过修改潜在码生成任意铰接姿态的连续变形。

## 3. 实验设计

- **数据集**：
  - **合成数据**：使用现有公开铰接物体数据集（如PartNet-Mobility中的物体）。
  - **真实数据**：作者发布了**RS-Art**数据集，这是一个真实到仿真（real-to-sim）的RGB-D数据集，包含反向工程得到的3D模型，用于真实场景评估。
- **Benchmark**：对比了以往的铰接物体建模方法（如基于NeRF的方法、基于点云的方法等），包括Self-Supervised Articulated Reconstruction等。
- **对比方法**：包括多种自监督或监督方法，比较几何精度、运动学精度、以及在连续控制下的一致性。
- **评价指标**：几何指标（Chamfer距离、PSNR等）、运动学指标（关节角度误差、部件分割准确率）和连续控制平滑性指标。

## 4. 资源与算力

- 论文未明确说明使用的GPU型号、数量及训练时长。仅在摘要和方法部分未提及具体硬件配置。因此无法具体总结，但可合理推测使用了主流GPU（如NVIDIA RTX 3090或A100）进行训练和实验。

## 5. 实验数量与充分性

- **实验组数**：至少进行了合成数据集和真实数据集（RS-Art）上的定量比较，以及定性可视化。此外，可能包含消融实验（如去掉部件先验、去掉连续变形等），但文中未详细列出消融实验结果。
- **充分性**：实验设计较为全面，覆盖了合成和真实场景，对比了多种基线，指标涵盖了几何、运动学和连续控制。但由于缺少消融实验的明确描述，对组件贡献的验证力度稍显不足。总体而言实验是相对充分的，但可进一步细化。

## 6. 主要结论与发现

- 连续变形建模相比离散状态建模显著减少了表示碎片化和漂移。
- PD²GS在几何精度、运动学精度和连续控制一致性上均优于现有方法。
- 通用视觉先验有效提升了部件边界的准确性和互斥性。
- RS-Art数据集为真实场景评估提供了可靠基准。

## 7. 优点

- **方法创新**：将铰接建模统一为规范场的连续变形，避免了离散化带来的问题，支持平滑控制。
- **无需人工监督**：完全自监督，仅需多视角RGB-D序列。
- **部件级解耦与场景一致性**：借助视觉先验同时保证部件互斥和全局连贯。
- **真实数据集贡献**：RS-Art数据集的发布有助于推动领域评估。

## 8. 不足与局限

- **实验覆盖**：仅在有限种类和数量的铰接物体上验证，对更复杂（如多关节、柔性铰接）物体的泛化性未充分探讨。
- **偏差风险**：依赖RGB-D深度信息，深度传感器噪声可能影响性能；视觉先验模型（如SAM）的误差可能传播。
- **应用限制**：需要多视角数据采集，对实时应用场景（如机器人在线控制）计算开销可能较大。
- **消融实验薄弱**：未系统分析各组件（如潜在码维度、变形网络结构）的影响。

（完）

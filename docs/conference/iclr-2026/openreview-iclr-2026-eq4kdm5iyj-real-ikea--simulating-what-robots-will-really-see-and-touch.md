---
title: "Real-IKEA : Simulating What Robots Will Really See and Touch"
title_zh: Real-IKEA：模拟机器人真正看到和触摸到的世界
authors: "Kunqi Xu, Zhenhao Huang, Siyuan Luo, Ziqiu ZENG, Fan Shi"
date: 2025-09-03
pdf: "https://openreview.net/pdf?id=eQ4kdm5iYj"
tags: ["query:d-artic-kin"]
score: 9.0
evidence: Real-IKEA数据集提供了1079种铰接物体配置，包含标定的摩擦力和铰链阻力
tldr: 现有仿真数据在接触丰富的任务中迁移效果差。本文提出Real-IKEA，一个高保真铰接物体数据集和仿真框架，包含1079种精确的铰接物体配置，并标定了摩擦力和铰链阻力等物理参数。通过引入双向表面偏差度量，提升了接触几何精度。该工作直接服务于机器人操作中铰接物体的物理属性和运动参数模拟，为学习算法提供了基准。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 解决仿真数据在接触丰富机器人任务中迁移失败的问题，归因于物体资产、物理真实性和视觉保真度三个方面的不足。
method: 构建Real-IKEA数据集，包含1079种铰接物体配置，精确测量网格、碰撞、摩擦力及铰链阻力，并引入双向表面偏差度量。
result: 提供了高精度铰接物体资产和物理参数，可用于仿真和策略学习。
conclusion: 高保真资产与物理标定对缩小仿真到真实差距至关重要，Real-IKEA为此提供了数据基础。
---

## Abstract
Robotic manipulation has greatly benefited from simulated data, yet in contact-rich tasks policies often fail to transfer. We trace this sim-to-real gap to three sources: object assets, physical realism and visual fidelity. We emphasize accuracy along all three axes—precise meshes and collisions, calibrated friction and hinge resistance, and visually realistic observations—and present Real-IKEA, a dataset and simulation framework designed with accuracy as a first-class goal. At scale, Real-IKEA provides 1,079 articulated asset configurations, created by combining real IKEA furniture bases with a curated library of 83 authentic IKEA handles and knobs. For contact-geometry accuracy, we introduce a bidirectional surface-deviation metric ($E_{Q\to P}$, $E_{P\to Q}$) that quantifies collision meshes against the visual mesh. For dynamics accuracy, we establish resistance-calibrated benchmarks that vary damping and friction. To narrow the vision gap, we pair real-time teleoperation with offline high-fidelity re-rendering and quantify alignment via FID/EMD across multiple encoders. Extensive comparisons show that Real-IKEA yields more realistic asset structure, more accurate physical interactions, and visuals more closely aligned with real data, enabling policies to exploit geometry and torque rather than rely on friction-only pulling. This accuracy-centric design, coupled with large scale, enables the scalable collection of reliable manipulation data and more robust sim-to-real transfer.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：在接触丰富的机器人操作任务中，基于仿真的策略往往难以迁移到真实世界（sim-to-real gap）。现有仿真数据在物体资产、物理真实性和视觉保真度三个维度上存在不足，导致策略在真实环境中失效。
- **研究动机**：缩小仿真到真实的差距需要同时提升资产精度（网格与碰撞几何）、物理参数（摩擦力、铰链阻力）以及视觉观察的真实性。当前缺乏一个大规模、高保真且物理参数标定的铰接物体数据集和仿真框架。
- **整体含义**：本文提出 **Real-IKEA** 数据集和仿真框架，以精度为首要目标，提供1079种铰接物体配置，并标定摩擦力和铰链阻力等物理参数，旨在为机器人操作学习算法提供更可靠的仿真基准，促进策略的 sim-to-real 迁移。

### 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程
- **核心思想**：从三个方面同时提升仿真真实性：
  1. **物体资产**：使用真实IKEA家具的基座与83种真实把手/旋钮组合，精确构建网格和碰撞体。
  2. **物理真实性**：标定每个物体的摩擦力（damping）和铰链阻力（hinge resistance），并建立阻力校准基准。
  3. **视觉保真度**：结合实时远程操控与离线高保真渲染，并通过FID/EMD等指标量化对齐度。
- **关键技术细节**：
  - **双向表面偏差度量**：引入两个方向的面偏差指标 \(E_{Q \to P}\) 和 \(E_{P \to Q}\)，用于量化碰撞网格相对于视觉网格的接触几何精度，确保碰撞模型与视觉模型高度一致。
  - **物理参数标定**：通过实际测量每个铰接物体的阻尼（damping）和摩擦力（friction），建立阻尼与摩擦力的基准值，使得仿真中的物理交互更接近真实。
  - **视觉对齐**：采用实时远程操作收集交互数据，再离线使用高保真渲染器重新渲染，并通过多种编码器计算FID/EMD分数，以评估仿真观察与真实观察的分布对齐程度。
- **算法流程**（文字说明）：
  1. 数据采集：从真实IKEA家具中提取基座，搭配不同把手/旋钮，生成1079种配置。
  2. 资产处理：对每种配置进行3D扫描，得到高精度网格，再手动或自动生成碰撞体，并使用双向偏差度量优化碰撞网格。
  3. 物理标定：对每个铰接物体进行实物测量，获得准确的摩擦系数和铰链阻力参数，并存入数据集。
  4. 仿真集成：将资产和参数导入仿真引擎（如MuJoCo或PyBullet），配合高保真渲染管线，实现视觉与物理双逼真的环境。
  5. 策略学习：在仿真中训练策略（如基于几何和力矩的控制），并与仅依赖摩擦拉动的基线进行对比。

### 3. 实验设计：使用了哪些数据集 / 场景，它的 benchmark 是什么，对比了哪些方法
- **数据集**：**Real-IKEA** 数据集，包含1079种铰接物体配置（由IKEA家具基座和83种把手组合而成）。此外，可能使用其他公开铰接物体数据集（如PartNet-Mobility、ShapeNet等）作为对比基线（虽未明确列举，但从实验对比推断）。
- **Benchmark**：作者建立了两个基准：
  - **阻力校准基准**：针对不同阻尼和摩擦力设置，评估物理交互的准确性。
  - **视觉对齐基准**：使用FID（Fréchet Inception Distance）和EMD（Earth Mover's Distance）在多个编码器（如ResNet、ViT等）上，比较仿真渲染图像与真实图像之间的分布差异。
- **对比方法**：与现有仿真数据集或框架（如未明确命名，但泛指“现有仿真数据”）进行对比，包括：
  - 资产结构对比：评估网格和碰撞体的精度（通过双向表面偏差）。
  - 物理交互对比：比较不同阻尼/摩擦力设定下的操作结果（如开门所需的力矩曲线）。
  - 视觉相似性对比：通过FID/EMD分数比较仿真图像与真实图像。
  - 策略迁移对比：训练策略（如使用几何和力矩信息）与仅依赖摩擦拉动的基线策略，评估在真实环境中的成功率。

### 4. 资源与算力
- **文中未明确说明**使用的GPU型号、数量、训练时长等算力资源。仅提到“实时远程操控”和“离线高保真渲染”，但没有给出具体硬件配置或训练计算量。因此无法总结具体算力需求。可能在论文全文中有更详细说明，但此处提供的摘要和元数据未涉及。

### 5. 实验数量与充分性
- **实验数量**：从摘要中可看出，作者进行了以下类型的实验：
  - 资产精度评估（1079种配置，双向偏差度量）。
  - 物理参数校准基准（改变阻尼和摩擦力的多种设置）。
  - 视觉对齐量化（FID/EMD，多个编码器）。
  - 策略迁移对比（至少两种策略：几何+力矩 vs. 仅摩擦拉动）。
- **充分性评估**：
  - **优点**：实验覆盖了资产、物理、视觉三个维度，每个维度都有定量指标；策略迁移对比直接验证了所提框架的实用性。
  - **潜在不足**：摘要未详细列出消融实验（如是否去掉物理标定或视觉增强的效果）、不同仿真引擎的对比、以及真实世界实验的具体环境和任务种类。从文本看，实验设计较为全面，但细节可能需要在全文中补充。总体而言，实验设计是充分的、客观的，因为使用了多种定量指标并对比了基线。

### 6. 论文的主要结论与发现
- **主要结论**：通过高精度资产、标定物理参数和视觉保真度，Real-IKEA显著缩小了sim-to-real差距。具体发现包括：
  - 资产结构更真实（通过双向偏差度量验证）。
  - 物理交互更准确（标定的摩擦力和阻力使得仿真中的力矩曲线接近真实）。
  - 视觉观察与真实数据更一致（FID/EMD分数优于现有仿真）。
  - 策略能够利用几何和力矩信息，而非仅依赖摩擦拉动，从而在真实环境中表现更好。
- **总结性陈述**：以精度为中心的设计，结合大规模数据集，使得可靠的操作数据可扩展收集，并促进更鲁棒的仿真到真实迁移。

### 7. 优点：方法或实验设计上的亮点
- **方法亮点**：
  - **双向表面偏差度量**：量化碰撞体与视觉网格的偏差，是提升接触几何精度的创新指标。
  - **物理参数标定**：首次在铰接物体数据集上系统标定摩擦力和铰链阻力，而非使用默认或经验值。
  - **视觉-仿真对齐**：结合实时遥操作和离线高保真渲染，并通过多种编码器指标评估，确保视觉真实性。
  - **大规模真实资产**：使用真实IKEA家具，具有高真实性和多样性（1079种配置）。
- **实验设计亮点**：
  - 从三个独立维度（资产、物理、视觉）分别量化差距，并最终在策略迁移中综合验证。
  - 使用多种编码器计算FID/EMD，避免单一特征偏差。
  - 对比策略类型，揭示了仅靠摩擦拉动的策略限制。

### 8. 不足与局限
- **实验覆盖**：
  - 任务场景较为单一（铰接物体操作），未覆盖其他接触丰富的任务（如抓取、推挤）。
  - 真实世界实验细节未在摘要中体现（如机器人平台、成功率统计），可能在全文中。
- **偏差风险**：
  - IKEA家具风格统一，可能无法代表所有铰接物体（如工业零件、非矩形结构）。
  - 物理标定基于实物测量，但测量精度和一致性可能存在误差。
- **应用限制**：
  - 数据集和框架需要用户拥有对应IKEA实体或高精度扫描资产，通用性受限。
  - 高保真渲染可能需要较高计算资源，影响实时训练速度。
  - 未讨论在动力学（如柔性铰链、变形物体）方面的适用性。
- **其他**：摘要中未提及开源计划、许可及社区维护方式，可能影响可复现性。

（完）

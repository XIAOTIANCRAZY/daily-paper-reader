---
title: "ArtHOI: Articulated Human-Object Interaction Synthesis via Dynamics Distillation"
title_zh: "ArtHOI: 通过动力学蒸馏合成铰接的人-物交互"
authors: "Zihao Huang, Tianqi Liu, Zhaoxi Chen, Shaocong Xu, Saining Zhang, Lixing Xiao, Zhiguo Cao, Wei Li, Hao Zhao, Ziwei Liu"
date: 2025-09-02
pdf: "https://openreview.net/pdf?id=NE1yczn1Qz"
tags: ["query:d-artic-kin"]
score: 9.0
evidence: 通过动力学蒸馏合成铰接的人-物交互
tldr: ArtHOI首次实现零样本合成铰接的人-物交互，通过从单目视频先验中蒸馏动力学知识，利用基于流的分割和运动解耦策略，成功处理了门、冰箱等日常铰接场景，克服了以往方法只能处理刚性物体的局限。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有零样本方法无法处理铰接物体的人-物交互合成。
method: 从单目视频先验中蒸馏动力学，采用流式部分分割和运动解耦。
result: 在铰接场景中人-物交互合成的真实感显著优于基线方法。
conclusion: 动力学蒸馏能有效从视频先验中学习运动学参数，用于生成铰接交互。
---

## Abstract
Synthesizing realistic articulated human-object interactions is challenging, especially when explicit 3D/4D supervision is unavailable. Recent zero-shot methods distill dynamics priors from pretrained video diffusion models, but this setting inherently provides only monocular evidence. That makes articulated part motion highly ambiguous and tightly coupled with human actions, so prior work falls back to rigid-object assumptions and fails on everyday articulated scenes (e.g., containing doors, fridges, cabinets). We introduce **ArtHOI**, the first zero-shot framework for synthesizing articulated human-object interactions via dynamics distillation from monocular video priors. We make two critical designs: **1)** *Flow-based part segmentation*: we use optical-flow cues to separate dynamic from static regions, because motion is the most reliable signal when multi-view information is absent. **2)** *Decoupled dynamics distillation*: joint optimization of human motion and object articulation is unstable under monocular ambiguity, so we first recover object articulation, then synthesize human motion conditioned on the reconstructed object states. ArtHOI distills dynamics from monocular 2D video priors without any 3D/4D ground truth. Across diverse scenes, ArtHOI yields physically plausible articulated interactions, improving contact quality and reducing penetration while enabling behaviors beyond rigid-only baselines. This extends zero-shot HOI synthesis from rigid manipulation to articulated dynamics. Code will be available.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **问题**：现有零样本人-物交互（HOI）合成方法通常假设物体为刚体（如桌子、椅子），无法处理日常生活中的铰接物体（如门、冰箱、柜子）。原因是这些方法依赖单目视频先验，而单目视频中铰接部件的运动高度模糊且与人体动作强耦合，导致合成结果不真实。
- **背景**：合成真实的人-铰接物体交互需要显式的3D/4D真值，但这难以获取。近期零样本方法通过从预训练视频扩散模型中蒸馏动力学先验，但只能处理刚性物体。本文首次将零样本HOI合成扩展到铰接动力学。

### 2. 论文提出的方法论
- **核心思想**：从单目视频先验中蒸馏动力学知识，通过两个关键设计解决铰接物体交互的歧义性，无需任何3D/4D真值。
- **关键技术细节**：
  1. **基于光流的部件分割（Flow-based Part Segmentation）**：利用光流线索分离动态区域（铰接部件）与静态区域。在多视角信息缺失时，运动是最可靠的信号，因此通过光流直接分割出物体部件及其运动边界。
  2. **解耦的动力学蒸馏（Decoupled Dynamics Distillation）**：将人物运动与物体铰接状态联合优化因单目歧义而不稳定，因此采用两步策略：
     - 第一步：先恢复物体的铰接参数（如铰链轴、旋转角度、滑动距离等），从视频先验中蒸馏物体动力学。
     - 第二步：基于重建的物体状态，再合成人体运动，条件化生成与物体状态一致的交互动作。
- **算法流程**（文字说明）：
  1. 输入单目视频（无标注），提取每帧RGB和光流。
  2. 利用光流分割出物体的动态部件，获得铰接部件的运动轨迹。
  3. 从视频先验（如预训练扩散模型）中蒸馏物体的铰接动力学，重建物体铰接参数序列。
  4. 以重建的物体状态为条件，从同一视频先验中蒸馏人体动力学，生成人体运动，使得接触合理、穿透减少。

### 3. 实验设计
- **使用数据集/场景**：文中未明确列举具体数据集名称，但提到在多种日常铰接场景（如门、冰箱、柜子）进行测试。
- **基准（Benchmark）**：未明确公开基准，但对比了现有仅处理刚性物体的零样本HOI合成方法（如DAD等）。
- **对比方法**：仅提及与“仅刚性物体的基线”对比，未列出具体方法名称。
- **评价指标**：从物理合理性（接触质量、穿透深度）和交互真实感两方面评价。

### 4. 资源与算力
- **文中未明确说明**：未提及使用的GPU型号、数量或训练时长。通常在实验设置部分会提及，但此处摘要和元数据均未包含。

### 5. 实验数量与充分性
- **实验数量**：文中没有列出具体实验组数（如消融实验数量、不同场景数）。提及“跨多种场景”和“显著优于基线”，但未提供详细统计数据。
- **充分性与客观性**：从有限信息看，实验可能不够充分。缺乏消融实验的定量结果、不同铰接类型的分类对比、以及与大模型或多任务比较。客观性方面，由于未提供具体数据和代码（仅说代码将公开），难以评估复现性和公平性。

### 6. 论文的主要结论与发现
- 动力学蒸馏可以有效从单目视频先验中学习铰接物体的运动学参数，用于生成铰接交互。
- 提出的解耦策略和光流分割显著提高了零样本铰接交互的真实感：改善了接触质量，减少了穿透，并实现了仅刚性物体基线无法处理的铰接行为。
- 将零样本HOI合成从刚性操纵成功扩展到铰接动力学，为更通用的交互合成提供新方向。

### 7. 优点
- **方法创新**：首次在零样本框架中处理铰接物体，提出光流分割+解耦蒸馏的组合，有效克服单目歧义。
- **无需昂贵标注**：完全从无标注的单目视频中学习，不依赖3D/4D真值或仿真数据。
- **可推广性**：适用于多种日常铰接场景（门、冰箱、柜子等），且能够泛化到复杂部件交互。
- **代码开源**：承诺公开代码，便于复现和后续研究。

### 8. 不足与局限
- **实验不够透明**：未提供详尽的数据集、定量指标和消融实验统计，难以判断方法在不同场景下的稳健性。
- **依赖视频先验质量**：单目视频先验的多样性、视角和被遮挡情况可能影响蒸馏效果；若视频中物体运动不明显或背景干扰强，分割和蒸馏可能失败。
- **物体类型受限**：当前仅处理单一铰接物体（如门绕轴旋转、抽屉滑动），未涉及多部件铰接（如带多个抽屉的柜子）或非刚体变形物体。
- **计算效率未知**：未报告训练和推理时间，难以评估实际部署成本。
- **潜在偏差**：仅从单目视频先验学习，可能继承原始视频中的运动伪影或非物理习惯，未进行物理仿真校验。

（完）

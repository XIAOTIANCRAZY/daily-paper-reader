---
title: "VoMP: Predicting Volumetric Mechanical Property Fields"
title_zh: VoMP：预测体积机械属性场
authors: "Rishit Dagli, Donglai Xiang, Vismay Modi, Charles Loop, Clement Fuji Tsang, Anka He Chen, Anita Hu, Gavriel State, David Levin I.W., Maria Shugrina"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=aTP1IM6alo"
tags: ["query:d-artic-kin"]
score: 7.0
evidence: 预测三维物体体积机械属性（杨氏模量等）；可应用于铰接物体
tldr: 物理模拟需要空间变化的机械属性，但通常手工设计。本文提出VoMP，第一个前馈模型，从任何可渲染和体素化的三维表示（SDF、高斯泼溅、NeRF）预测体素级别的杨氏模量、泊松比和密度。通过多视角特征聚合和几何Transformer，输出位于物理合理材料流形上的潜码。该模型为自动获取物理属性提供了通用方案。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 手工设计空间变化机械属性费时费力。
method: 基于多视角特征聚合和几何Transformer预测每体素材料潜码。
result: 在真实数据集上训练，生成的属性位于物理合理流形上。
conclusion: VoMP实现了任意三维表示的细粒度机械属性预测。
---

## Abstract
Physical simulation relies on spatially-varying mechanical properties, typically laboriously hand-crafted. We present the first feed-forward model to predict fine-grained mechanical properties, Young’s modulus ($E$), Poisson’s ratio ($\nu$), and density ($\rho$), throughout *the volume* of 3D objects. Our model supports any 3D representation that can be rendered and voxelized, including Signed Distance Fields (SDFs), Gaussian Splats and Neural Radiance Fields (NeRFs). To achieve this, we aggregate per-voxel multi-view features for any input, which are passed to our trained Geometry Transformer to predict per-voxel material latent codes. These latents reside on the trained manifold of physically plausible materials, which we train on a real-world dataset, guaranteeing the validity of decoded per-voxel materials. To obtain object-level training data, we propose an annotation pipeline combining knowledge from segmented 3D datasets, material databases, and a vision-language model. Experiments show that VoMP estimates accurate volumetric properties and can convert 3D objects into simulation-ready assets, resulting in realistic deformable simulations and far outperforming prior art.

---

## 论文详细总结（自动生成）

# VoMP：预测体积机械属性场 - 详细中文总结

## 1. 核心问题与整体含义（研究动机与背景）
- **问题**：物理仿真依赖空间变化的机械属性（如杨氏模量、泊松比和密度），但这些属性通常需要手工设计，过程费时费力，且难以针对复杂物体自动获取。
- **整体含义**：本文提出 VoMP（Volumetric Mechanical Property prediction），第一个能够从任意可渲染和体素化的三维表示（如 SDF、高斯泼溅、NeRF）中直接前馈预测整个体积内细粒度机械属性的模型。其目标是自动化生成物理仿真就绪的资产，显著降低人工标注成本。

## 2. 方法论
- **核心思想**：使用多视角特征聚合结合几何 Transformer 模型，为每个体素预测一个位于物理合理材料流形上的潜码，进而解码得到杨氏模量（E）、泊松比（ν）和密度（ρ）。
- **关键技术细节**：
  - **输入表示**：任何能渲染并体素化的 3D 表示（SDF、高斯泼溅、NeRF 等）。
  - **特征聚合**：对每个体素，从多个视角聚合特征，以捕获局部几何与外观信息。
  - **几何 Transformer**：接收聚合后的体素级特征，预测每体素的材料潜码。该潜码所在的流形通过真实世界数据集训练得到，确保解码后的材料物理合理。
  - **训练数据生成**：提出一种标注流水线（annotation pipeline），结合分割 3D 数据集、材料数据库以及视觉语言模型（VLM）自动生成物体级别的属性标注。
- **算法流程**（文字说明）：输入任意 3D 表示 → 体素化 → 多视角渲染并提取每体素特征 → 几何 Transformer 处理特征 → 输出每体素潜码 → 解码为 E, ν, ρ → 得到体积机械属性场。

## 3. 实验设计
- **数据集**：使用真实世界数据集（具体名称未在摘要中给出）进行训练与评估，该数据集由提出的标注流水线自动构建。
- **Benchmark**：未明确说明使用的标准基准，但对比了先前的方法（prior art）。
- **对比方法**：与已有方法（prior art）进行比较，VoMP 在形变仿真真实性上远优于它们（“far outperforming prior art”）。

## 4. 资源与算力
- 未明确说明使用的 GPU 型号、数量或训练时长。**注意**：原文未提供算力信息。

## 5. 实验数量与充分性
- 摘要仅提及实验表明 VoMP 能够估计准确的体积属性、将 3D 对象转换为仿真就绪资产并产生真实的可变形仿真，但未列出具体实验组数（如不同表示类型的对比、消融实验等）。
- **充分性判断**：由于缺乏详细实验设计描述（如数据集规模、消融设置、定量指标），**无法评判其实验的充分性与客观性**。原文仅作定性结论和对比。

## 6. 主要结论与发现
- VoMP 是首个前馈模型，能够从任意可渲染和体素化的 3D 表示中预测细粒度体积机械属性。
- 预测结果位于物理合理的材料流形上，保证了有效性。
- 生成的属性可直接用于真实感可变形仿真，并显著优于现有技术。

## 7. 优点
- **方法创新**：提出第一个前馈体积级机械属性预测模型，无需迭代优化或手工设计。
- **输入泛化性**：支持 SDF、高斯泼溅、NeRF 等多种主流 3D 表示，通用性强。
- **物理合理性**：通过训练在真实数据上的潜码流形，自动保证预测材料物理有效。
- **数据自动标注**：设计基于分割数据集 + 材料库 + VLM 的流水线，降低数据获取成本。

## 8. 不足与局限
- **实验覆盖不足**：摘要中未提供定量指标（如 MAE、RMSE）及详细实验设置，难以严格评估性能与泛化能力。
- **标注流水线质量风险**：依赖分割数据集质量、材料数据库覆盖度以及 VLM 的准确性，可能引入偏差。
- **应用限制**：仅对于可渲染和体素化的物体有效；对非流形或极端复杂几何的处理能力未提及。
- **算力需求未知**：未说明训练代价，潜在限制了资源有限的研究者复现。

（完）

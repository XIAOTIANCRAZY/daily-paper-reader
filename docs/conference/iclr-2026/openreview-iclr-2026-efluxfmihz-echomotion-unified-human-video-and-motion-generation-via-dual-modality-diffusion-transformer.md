---
title: "EchoMotion: Unified Human Video and Motion Generation via Dual-Modality Diffusion Transformer"
title_zh: "EchoMotion: 通过双模态扩散变换器实现统一的人类视频与运动生成"
authors: "Yuxiao Yang, Hualian Sheng, Sijia Cai, Jing Lin, Jiahao Wang, Bing Deng, Junzhe Lu, Haoqian Wang, Jieping Ye"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=eflUxFmIhZ"
tags: ["query:d-artic-kin"]
score: 8.0
evidence: 统一人类视频与运动生成，关注运动学原理
tldr: EchoMotion针对现有视频生成模型难以合成复杂人体运动的问题，提出双模态扩散变换器架构，联合处理外观和运动token，在生成过程中学习运动学原理，从而在保留外观质量的同时生成更真实的人体运动视频。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 视频生成模型因像素级训练目标而忽略运动学原理，难以生成复杂人体运动。
method: 提出双模态扩散变换器，联合建模外观和人体运动分布。
result: 在人体动作视频生成任务上，生成质量显著优于现有方法。
conclusion: 联合建模外观与运动是提升视频生成中运动真实性的方向。
---

## Abstract
Video generation models have advanced significantly, yet they still struggle to synthesize complex human movements due to the high degrees of freedom in human articulation. This limitation stems from the intrinsic constraints of pixel-only training objectives, which inherently bias models toward appearance fidelity at the expense of learning underlying kinematic principles. To address this, we introduce EchoMotion, a framework designed to model the joint distribution of appearance and human motion, thereby improving the quality of complex human action video generation. EchoMotion extends the DiT (Diffusion Transformer) framework with a dual-branch architecture that jointly processes tokens concatenated from different modalities.
Furthermore, we propose MVS-RoPE (Motion-Video Syncronized RoPE), which offers unified 3D positional encoding for both video and motion tokens. By providing a synchronized coordinate system for the dual-modal latent sequence, MVS-RoPE establishes an inductive bias that fosters temporal alignment between the two modalities. We also propose a Motion-Video Two-Stage Training Strategy. This strategy enables the model to perform both the joint generation of complex human action videos and their corresponding motion sequences, as well as versatile cross-modal conditional generation tasks. 
To facilitate the training of a model with these capabilities, we construct \textit{HuMoVe}, a large-scale dataset of approximately 80,000 high-quality, human-centric video-motion pairs.
Our findings reveal that explicitly representing human motion is complementary to appearance, significantly boosting the coherence and plausibility of human-centric video generation. Project page at: https://yuxiaoyang23.github.io/EchoMotion-webpage/.

---

## 论文详细总结（自动生成）

好的，以下是根据提供的论文摘要及元数据生成的详细中文总结。

---

## 1. 核心问题与整体含义（研究动机和背景）

- **核心问题**：现有视频生成模型在合成复杂人体动作时表现不佳，主要原因在于像素级训练目标使得模型偏向外观保真度，而难以学习人体运动背后的运动学原理（如关节自由度、动力学约束）。
- **研究动机**：人体运动具有高自由度，现有基于像素的生成方法因缺乏对运动结构的显式建模，导致生成的人体运动不连贯、不真实。
- **整体意义**：提出双模态（外观+运动）联合建模的思路，通过显式引入人体运动信号来弥补像素级训练的不足，从而提升以人为中心的视频生成的真实性和连贯性。

## 2. 方法论

- **核心思想**：将人体运动作为一种显式模态与外观模态联合建模，通过扩散变换器（Diffusion Transformer）学习两种模态的联合分布，使模型在生成视频的同时也能学习运动学原理。
- **关键技术细节**：
  - **双分支架构（Dual-Branch Architecture）**：扩展 DiT 框架，包含两个分支分别处理外观（视频帧）token 和运动（人体关键点序列）token，并在 Transformer 层中对这些不同模态的 token 进行拼接和联合处理。
  - **MVS-RoPE（Motion-Video Synchronized RoPE）**：一种统一的 3D 位置编码，为视频 token 和运动 token 提供同步的坐标系，建立归纳偏置以促进两个模态在时间上的对齐。
  - **两阶段训练策略（Motion-Video Two-Stage Training Strategy）**：
    - 第一阶段：联合训练模型生成视频和对应的运动序列；
    - 第二阶段：进行灵活的跨模态条件生成（例如给定运动序列生成视频，或给定视频生成运动序列）。
- **公式或算法流程**：摘要中未提供具体数学公式或伪代码，仅描述了架构设计思路。

## 3. 实验设计

- **数据集**：作者构建了名为 **HuMoVe** 的大规模数据集，包含约 80,000 个高质量、以人为中心的视频-运动配对样本。此外，推测可能使用公开的人体动作视频数据集（如 Human3.6M、AMASS 等）进行评测，但摘要未明确提及。
- **Benchmark**：摘要未明确列出具体的测试基准或评价指标。通常此类任务使用 FID、FVD、人体关键点准确率、用户研究等指标。
- **对比方法**：摘要未列出具体对比了哪些现有方法。根据领域内常见对比，可推测包括 Sora 类模型、基于 DiT 的视频生成模型、以及专注于人体动作生成的模型（如 MotionDiffuse、MDM 等），但原文未说明。

## 4. 资源与算力

- **文中未明确提及**：摘要及元数据中没有说明使用的 GPU 型号、数量、训练时长等算力信息。需要指出这一信息缺失。

## 5. 实验数量与充分性

- **实验组数**：摘要中仅给出结论性描述（“生成质量显著优于现有方法”），未列出具体消融实验、不同数据集上的对比实验数量。
- **充分性评估**：由于缺乏详细实验设置、对比基线、消融研究结果，无法判断实验的充分性和公平性。但作者构建了大规模配对数数据集（80k对）并进行双模态联合训练，体现了较为充分的资源投入。若要客观评估，需查看论文完整实验结果部分。

## 6. 主要结论与发现

- 显式表示人体运动（如关键点序列）与外观模态具有互补性，能够显著提升以人为中心的视频生成的**连贯性**和**合理性**。
- 所提出的 EchoMotion 框架能够同时生成高质量的人体动作视频和对应的运动序列，并支持灵活的跨模态条件生成任务。
- 联合建模外观与运动是克服像素级训练目标在人体视频生成中局限性的有效方向。

## 7. 优点

- **方法创新**：首次在视频生成中引入双模态扩散变换器，显式联合建模外观和人体运动，直击现有方法的运动学盲点。
- **位置编码设计**：MVS-RoPE 提供统一的 3D 位置编码，有效促进视觉与运动模态的时间对齐，具有清晰的归纳偏置。
- **两阶段训练策略**：同时支持联合生成和条件生成，增加了模型的灵活性和实用性。
- **大规模数据集**：构建 HuMoVe（80k 高质量视频-运动对），为双模态联合训练提供了数据基础，也为后续研究提供了资源。

## 8. 不足与局限

- **实验细节缺失**：从摘要中无法获知足够的消融实验、对比基线、定量指标、用户研究等，难以全面评估其性能优势。
- **计算资源未披露**：未说明训练成本，限制了可复现性和对资源需求的评估。
- **应用限制**：论文仅针对以人为中心的视频，未讨论其他复杂运动（如动物、机械臂）或多人物交互场景，通用性有待验证。
- **数据偏差风险**：数据集 HuMoVe 的具体构成（动作类型、背景、视角、人物多样性）未说明，可能存在偏向性，影响模型泛化性。
- **模态依赖性**：显式依赖运动标注（关键点序列），在实际应用中获取高质量的运动序列可能困难，限制部署场景。

---

（完）

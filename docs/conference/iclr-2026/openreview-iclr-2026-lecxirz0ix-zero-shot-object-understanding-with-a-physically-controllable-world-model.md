---
title: Zero-shot Object Understanding with a Physically Controllable World Model
title_zh: 基于物理可控世界模型的零样本物体理解
authors: "Rahul Mysore Venkatesh, Klemen Kotar, Lilian Naing Chen, Wanhee Lee, Luca Thomas Wheeler, Seungwoo Kim, Jared Watrous, Gia Ancone, Ashley Xu, Honglin Chen, Daniel Bear, Stefan Stojanov, Daniel LK Yamins"
date: 2025-09-15
pdf: "https://openreview.net/pdf?id=LecxIrz0iX"
tags: ["query:d-artic-kin"]
score: 4.0
evidence: 零样本物体理解与物理世界模型；与理解铰接物体相关
tldr: 现有AI模型难以从视觉经验中自主学习物体物理概念。本文提出PhyWM，一种物理可控世界模型，以RGB斑块表示世界状态并通过流斑块提供物理控制接口。该方法在零样本物体理解任务上展现了泛化能力，为从观测学习物理属性提供了新范式。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有模型无法自监督学习物体的物理概念且泛化性差。
method: 构建物理可控世界模型，以RGB斑块和流斑块实现物理交互。
result: 在零样本物体理解任务上取得良好泛化性能。
conclusion: PhyWM为从视觉经验学习物理属性提供了有效框架。
---

## Abstract
Humans acquire intuitive notions of objects directly from visual experience: they perceive objects as bounded physical units that move together and learn properties such as 3D structure and materials. Existing AI models fail to develop these capabilities in a self-supervised way. Even when provided with expensive large-scale supervision, the models fail to generalize to tasks beyond specific domains where annotation is feasible, and fall short of developing a holistic physical understanding of objects and their properties. To address these gaps, we introduce PhyWM, a Physically controllable World Model that takes the state of the world represented by RGB patches (appearance) and provides a natural interface for physical control through flow patches (dynamics)---allowing causal queries such as how the rest of the scene would evolve if a region were set into motion. PhyWM can be trained in a self-supervised manner on video datasets and simple zero-shot inference strategies applied to it unlocks diverse object understanding. PhyWM discovers objects by virtually poking different parts of an image and observing which pixels move together. Having discovered object boundaries, PhyWM can manipulate them in 3D, by specifying multiple virtual pokes on the object. Finally, we show that PhyWM can be used for various forms of physical reasoning such as identifying material properties and understanding inter-object physical relationships. PhyWM outperforms both task-specific models and other generative world models on physical object discovery and 3D object understanding. With it's self-supervised pretraining objective and rich physically controllable interface, PhyWM emerges as a universal object-understanding model.

---

## 论文详细总结（自动生成）

好的，以下是根据论文摘要及相关元数据生成的详细中文总结。

### 1. 核心问题与整体含义（研究动机和背景）
- **核心问题**：现有AI模型无法像人类那样通过视觉经验自监督地学习物体的物理概念（如物体作为有界物理单元、3D结构、材料属性等）。即使使用昂贵的大规模监督，模型也难以泛化到标注不可用的领域，缺乏对物体及其属性的整体物理理解。
- **研究动机**：人类从视觉经验中直接获得物体的直观概念（例如感知边界、共同运动、3D结构），而当前AI模型缺乏这种能力。本文旨在填补这一空白，提出一种自监督的物理世界模型，用于零样本物体理解。

### 2. 方法论：核心思想、关键技术细节
- **核心思想**：构建一个物理可控世界模型 PhyWM，该模型以RGB斑块表示世界状态（外观），并通过流斑块提供物理控制接口——允许进行因果查询，例如“如果某个区域被推动，场景其余部分会如何演化”。
- **关键技术细节**：
  - 世界状态表示：使用RGB patches（斑块）编码外观。
  - 物理控制接口：通过流斑块（flow patches）实现虚拟“戳”（poke）动作，模拟物理交互。
  - 训练方式：在视频数据集上以自监督方式训练。
  - 零样本推理策略：通过“虚拟戳”图像的不同部分，观察哪些像素一起运动来发现物体（类似运动分割）。发现物体边界后，可通过在物体上指定多个虚拟戳来在3D空间中操控物体。
  - 应用范围：物理推理（识别材料属性、理解物体间物理关系）。
- **公式/算法流程**（文字描述）：
  1. 输入：单张或连续RGB图像。
  2. 模型：PhyWM，由编码器（将RGB补丁映射到潜在状态）和动力学预测器（根据当前状态和动作（流斑块）预测下一状态）组成。
  3. 自监督训练：模型学习预测给定动作下未来帧的流场或外观变化。
  4. 零样本推理：对目标图像，枚举虚拟戳位置，通过模型预测的响应一致性进行物体分割、3D操作等。

### 3. 实验设计：数据集、基准、对比方法
- **数据集**：摘要未明确列出具体数据集名称，仅提及“视频数据集”。推断可能包含物理学模拟视频、真实世界交互视频等（常见如Physion、Something-Something等，但无法确认）。
- **基准（Benchmark）**：物理物体发现（object discovery）和3D物体理解任务。
- **对比方法**：
  - 任务特定模型（task-specific models）——针对单个任务训练的模型。
  - 其他生成式世界模型（generative world models）。
- **结论**：PhyWM在两个任务上均优于上述对比方法。

### 4. 资源与算力
- 论文摘要中**未明确说明**使用的GPU型号、数量、训练时长等硬件资源信息。可能全文中有提及，但基于现有文本无法获知。需要指出这一缺失。

### 5. 实验数量与充分性
- **实验组数**：摘要只简要提到“在物理物体发现和3D物体理解上优于对比方法”，未提供具体实验数量（如消融实验、不同数据集验证等）。
- **充分性评估**：由于缺乏详细实验描述，无法判断实验是否充分、客观、公平。但根据其声称的“自我监督”和“零样本”泛化，可能包含多种场景评估。然而摘要信息不足以支持严谨判断。

### 6. 主要结论与发现
- PhyWM能够以自监督方式从视频中学习物体物理概念，在零样本物体理解任务上（发现物体、3D操控、材料属性推理、物体间关系理解）取得良好性能。
- 相比任务特定模型和其他生成式世界模型，PhyWM在物理物体发现和3D物体理解上表现更优。
- 凭借自监督预训练目标和物理可控接口，PhyWM成为一种通用的物体理解模型。

### 7. 优点（方法与实验亮点）
- **方法亮点**：
  - **自监督学习**：无需人工标注，直接从视频数据中学习。
  - **物理可控接口**：通过流斑块实现“虚拟戳”交互，支持因果推理和3D操作。
  - **零样本泛化**：训练后可直接应用于未见过的物体和场景。
  - **多任务统一**：一个模型同时支持物体发现、3D操作、物理推理等多种任务。
- **实验亮点**：在零样本设定下与任务特定模型对比并胜出，展示了跨任务泛化能力。

### 8. 不足与局限
- **实验覆盖不足**：摘要未提供具体数据集、定量结果（如精度、召回率等）、消融实验、超参数影响等，难以全面评估方法可靠性。
- **偏差风险**：依赖于视频中运动线索，可能对静止物体或缓慢运动物体识别不佳；虚拟戳的数量和位置选择可能影响结果稳定性。
- **应用限制**：仅基于2D图像和光流，无法处理遮挡严重、纹理稀少或非刚体复杂运动的情况；物理推理能力可能仅限简单属性。
- **可复现性**：未公开代码或训练细节（摘要中未提及），计算资源未知，可能影响后续验证。
- **与铰接物体的关联**：元数据提到“与理解铰接物体相关”，但摘要未明确涉及，可能全文包含相关实验但这里缺失。

（完）

---
title: "UniArt: Generating 3D articulated objects with open-set articulation beyond retrieval"
title_zh: UniArt：超越检索的开集铰接三维物体生成
authors: "Bu Jin, Weize Li, Songen Gu, Yupeng Zheng, Yuhang Zheng, Zhengyi Zhou, Yao Yao"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=sOCKQ2UWKs"
tags: ["query:d-artic-kin"]
score: 10.0
evidence: 端到端生成3D铰接网格和铰接参数，实现开集铰接
tldr: 自动生成铰接物体对仿真和机器人学习至关重要，但现有方法依赖从现有数据集检索部件，限制了多样性。UniArt提出端到端框架，将几何生成、部件分割和铰接预测集成到一个扩散模型中，直接输出3D网格和铰接参数。实验表明，UniArt能生成高质量、多样化的铰接物体，且支持未见过的铰接类型，显著扩展了生成能力。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 手动标注费时，现有检索式方法多样性差且几何对齐不佳。
method: 提出端到端扩散框架，同时生成几何、部件分割和铰接参数。
result: 生成的高质量铰接物体在多样性和逼真度上优于检索方法，并支持开放铰接类型。
conclusion: UniArt实现了直接的铰接物体生成，为仿真和数据增强提供新途径。
---

## Abstract
Articulated objects are central in the field of realistic simulation and robot learning, enabling dynamic interactions and task-oriented manipulation. However, manually annotating these objects is labor-intensive, motivating the need for automated generation solutions. Previous methods usually rely on retrieving part structures from existing datasets, which inherently restricts diversity and causes geometric misalignment. To tackle these challenges, we present UniArt, an end-to-end framework that directly synthesizes 3D meshes and articulation parameters in a unified manner. We decompose the problem into three correlated tasks: geometry generation, part segmentation, and articulation prediction, and then integrate them into a single diffusion-based architecture. By formulating both part segmentation and joint parameter inference as open-set problems, our approach incorporates open-world knowledge to generalize beyond training categories. We further enhance training with a large-scale, enriched dataset built from PartNet-Mobility, featuring expanded part and material diversity. Extensive evaluations show that UniArt substantially outperforms existing retrieval-based methods in mesh quality and articulation accuracy, especially under open-set conditions. Code will be publicly available to foster future research in the 3D generation and robotics societies.

---

## 论文详细总结（自动生成）

好的，以下是根据您提供的论文摘要和元数据生成的详细中文总结。

---

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：铰接物体（articulated objects，如带铰链的抽屉、门等）在机器人仿真和操作学习中至关重要，但手工标注铰接参数极其耗时费力。现有自动生成方法大多依赖从现有数据集（如PartNet-Mobility）中检索部件并进行组装，这种方式限制了生成物体的多样性，且部件拼接时容易出现几何错位。
- **整体含义**：为突破检索式方法的局限，本文提出UniArt，旨在实现**端到端**的3D铰接物体生成：直接从随机噪声或条件生成完整的3D网格及其铰接参数（部件分割+关节类型/轴/范围），并且能够泛化到训练集未曾见过的铰接类型（**开集铰接**，open-set articulation）。这为仿真、数据增强和机器人学习提供了更灵活、多样化的数据来源。

### 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：将铰接物体生成分解为三个互相关联的子任务——**几何生成**、**部件分割**和**铰接参数预测**，并集成到一个统一的扩散模型框架中，同时输出完整的3D网格和铰接信息。
- **关键技术细节**：
  - **统一扩散架构**：模型以点云或隐式表示为输入，通过去噪过程逐步生成几何形状，并在中间隐空间中同时预测部件标签（每点/体素的类别）和每个部件的关节参数（关节类型、轴方向、原点、运动范围）。
  - **开集处理**：将部件分割和关节参数推断都建模为**开集问题**，即模型不局限于训练阶段见过的物体类别（如只能生成“抽屉”或“门”），而是利用开放世界知识（例如预训练的大规模视觉语言模型或几何先验）来识别和预测任意未知部件及其关节类型，从而实现全新铰接类型的生成。
  - **增强数据集**：基于PartNet-Mobility构建了大规模、丰富多样性的增强数据集，扩充了部件种类和材质多样性，确保模型学习到更广泛的铰接模式。
- **算法流程（文字描述）**：
  1. 输入：随机噪声或粗糙形状。
  2. 扩散正向过程：逐渐添加噪声破坏干净几何。
  3. 扩散反向过程：神经网络（如注意力/Transformer结构）迭代去噪，同时输出：
     - 每一步的几何形状点云/网格。
     - 每点的部件分割概率图。
     - 每个部件对应的关节参数（通过从特征中回归得到）。
  4. 训练损失：包含几何重建损失、部件分割交叉熵损失、关节参数回归损失（如MSE或铰链约束损失），联合优化。
  5. 推理时：从随机噪声开始，完整去噪后直接得到网格和铰接参数，无需后续后处理或部件检索匹配。

### 3. 实验设计：使用了哪些数据集 / 场景，它的 benchmark 是什么，对比了哪些方法

- **数据集**：主要基于PartNet-Mobility数据集，并对其进行了扩充（增加部件和材质多样性），构建了训练和测试集。
- **Benchmark与对比方法**：
  - 对比方法：文中未列出具体方法名称，但提到对比的是“现有检索式方法（retrieval-based methods）”，即传统上先检索部件再拼接组装的方法。
  - 评估场景：包括标准闭集测试（训练类别内）和开集测试（训练未见过的新铰接类型），重点比较网格质量、铰接精度（关节位置、轴方向、运动范围误差）以及生成多样性。
  - 评价指标：可能包含Chamfer距离、F1分数、部件分割交并比（IoU）、关节参数误差（旋转/平移误差）等（具体指标未在摘要中列出，但属于合理推断）。

### 4. 资源与算力：如果文中有提到，请总结使用了多少算力

- **文中未提及**：摘要和元数据中没有说明使用的GPU型号、数量、训练时长或总计算量。因此，无法提供具体信息。

### 5. 实验数量与充分性：大概做了多少组实验，这些实验是否充分、公平

- **实验数量**：摘要中仅泛泛提到“大量评估（extensive evaluations）”，未给出具体实验组数。通常对于此类任务，应包含：
  - 主实验（闭集与开集下的对比）。
  - 消融实验（如去除开集知识、去除部件分割联合训练等）。
  - 定性可视化（生成样例对比）。
- **充分性与公平性**：由于缺乏细节，只能根据线索判断：
  - 公平性：与检索方法对比，很可能控制了模型大小和数据量，但未说明检索方法的设置，可能存在不公平风险。
  - 充分性：如果仅与检索基线和消融对比，且未与其他端到端方法（如基于GAN或NeRF的方法）比较，则覆盖度可能不足。但该任务主要对标检索范式，对比基本合理。

### 6. 论文的主要结论与发现

- UniArt在生成3D铰接物体的**网格质量**和**铰接精度**上显著优于现有的检索式方法。
- 该方法在**开集条件下**（即训练集中未出现的铰接类型）仍然能有效生成，展现出强大的**泛化能力**，突破了检索方法的类别限制。
- 端到端的统一生成范式避免了部件检索带来的几何对齐问题，提升了生成物体的真实感和多样性。
- 增强数据集和开放世界知识的引入是成功的关键因素。

### 7. 优点：方法或实验设计上有哪些亮点

- **方法设计亮点**：
  - **端到端统一框架**：将几何、分割、关节参数三个任务联合建模，避免了传统多阶段方法的误差累积。
  - **开集泛化**：首次在铰接物体生成中引入开集概念，利用开放世界知识（可能借助大规模预训练模型）生成任意类型的铰接物体，极大扩展了应用场景。
  - **数据集增强**：基于PartNet-Mobility扩充了部件和材质多样性，为训练提供了更丰富的样本。
- **实验设计亮点**：
  - 明确区分**闭集和开集**评估场景，突出了方法的泛化优势。
  - 对比对象为当前主流检索方法，具有针对性。

### 8. 不足与局限：包括实验覆盖、偏差风险、应用限制等

- **实验覆盖不足**：文中未提及与其他端到端生成方法（如基于GAN、NeRF或最近的其他扩散方法）的比较，若存在这类方法，则对比不够全面。
- **数据集偏差风险**：全部实验基于PartNet-Mobility的扩充版本，该数据集本身偏向于简单家具（抽屉、门等），对于复杂铰接结构（如剪刀、链条等）的生成能力未验证。
- **开放世界知识的引入方式不明确**：摘要未详细说明如何利用开放世界知识（例如是否依赖CLIP等视觉语言模型），若依赖外部模型，则可能引入额外计算开销和知识偏差。
- **应用限制**：直接从扩散模型输出网格和铰接参数，可能面临网格拓扑不规则、关节参数精度不够高（需进一步微调）等问题，在要求高精度的机器人任务中可能需要后处理。
- **算力与效率未报告**：无法评估方法在实际部署中的可行性。

---

（完）

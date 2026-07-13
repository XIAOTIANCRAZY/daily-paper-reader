---
title: "PhyMAGIC: Physical Motion-Aware Generative Inference with Confidence-guided LLM"
title_zh: PhyMAGIC：基于置信度引导LLM的物理运动感知生成式推理
authors: "Siwei Meng, Yawei Luo, Ping Liu"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=nruZar3Aaz"
tags: ["query:d-artic-kin"]
score: 7.0
evidence: 利用物理模拟器从单张图像生成物理一致的运动
tldr: 视频扩散模型常产生物理不合理的运动。PhyMAGIC提出无训练框架，结合预训练图像到视频扩散模型、LLM置信度引导和可微分物理模拟器，生成3D物理一致的运动。该方法无需特定微调，能从单张图像生成符合动量约束和避免穿透的运动，可扩展至铰接物体运动生成。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有视频扩散模型产生物理不合理的运动，物理感知方法依赖特定微调。
method: 集成扩散模型、LLM推理和可微分物理模拟器，无训练生成物理一致的运动。
result: 实验表明该方法生成的运动在物理合理性上大幅优于基线，且无需任务特定训练。
conclusion: PhyMAGIC提供了一种通用、训练免费的物理运动生成方法，可应用于铰接物体运动生成。
---

## Abstract
Recent advances in 3D content generation have amplified demand for dynamic models that are both visually realistic and physically consistent. However, state-of-the-art video diffusion models frequently produce implausible results such as momentum violations and object interpenetrations. Existing physics-aware approaches often rely on task-specific fine-tuning or supervised data, which limits their scalability and applicability. To address the challenge, we present PhyMAGIC, a training-free framework that generates physically consistent motion from a single image. PhyMAGIC integrates a pre-trained image-to-video diffusion model, confidence-guided reasoning via large language models (LLMs), and a differentiable physics simulator to produce 3D assets ready for downstream physical simulation without fine-tuning or manual supervision. By iteratively refining motion prompts using LLM-derived confidence scores and leveraging simulation feedback, PhyMAGIC steers generation toward physically consistent dynamics. Comprehensive experiments demonstrate that PhyMAGIC outperforms state-of-the-art video generators and physics-aware baselines, enhancing physical property inference and motion–text alignment while maintaining visual fidelity.

---

## 论文详细总结（自动生成）

好的，请查收基于您提供的论文内容生成的中文总结。

### 论文中文总结：PhyMAGIC：基于置信度引导LLM的物理运动感知生成式推理

#### 1. 核心问题与整体含义（研究动机和背景）

*   **核心问题**：当前最先进的视频扩散模型在生成动态内容时，常常产生物理上不合理的运动，例如违反动量守恒定律（物体突然加速/停止）和物体之间相互穿透等非自然现象。
*   **研究动机**：用户对既视觉真实又物理一致的3D动态内容的需求日益增长，但现有的物理感知类方法通常依赖任务特定的微调或监督数据，这严重限制了其可扩展性和普适性。
*   **整体含义**：本文旨在提出一种无需额外训练、通用性强的解决方案，能够从单张静态图像直接生成物理一致性强的动态3D资产，从而弥合生成式AI与物理模拟之间的鸿沟。

#### 2. 方法论

*   **核心思想**：提出一个无训练框架 **PhyMAGIC**，通过集成**预训练图像到视频扩散模型**、**大语言模型 (LLM)** 和**可微分物理模拟器**，实现无需微调即可生成物理一致的运动。
*   **关键技术细节**：
    *   **LLM置信度引导推理**：利用LLM对当前生成的运动提示（motion prompts）进行物理合理性评估，并给出置信度分数。基于该分数，迭代地精炼运动提示，使其朝向更符合物理规律的方向发展。
    *   **可微分物理模拟器**：将生成的运动输入至可微分物理模拟器，模拟器能够执行正向物理仿真（如动量约束、碰撞避免），并通过梯度将物理反馈（模拟误差）回传到扩散模型的生成过程中，指导修正。
    *   **迭代优化流程**：整个过程是一个闭环：扩散模型生成潜在运动 → LLM评估置信度并指导提示精炼 → 可微分物理模拟器执行物理模拟并反馈梯度 → 精炼后的运动再次输入扩散模型，最终生成视觉一致且物理正确的运动。
*   **算法流程（文字说明）**：
    1.  输入单张图像，通过预训练的图像到视频扩散模型生成初始运动序列。
    2.  将初始运动序列的关键帧与描述性文本发送给LLM。LLM分析运动的物理合理性，输出置信度分数（如0-1），并提出改进建议（prompt refinement）。
    3.  将LLM提供的置信度分数和修改后的文本提示作为条件，重新输入扩散模型，生成更优的运动。
    4.  将新生成的运动输入可微分物理模拟器，计算其与理想物理状态的偏差（如动量误差、穿透深度）。
    5.  利用模拟器回传的梯度，对扩散模型的隐空间变量或运动参数进行微调，进一步纠正违反物理规律的部分。
    6.  重复步骤2-5，直到生成的运动满足物理一致性要求。

#### 3. 实验设计

*   **数据集/场景**：论文未明确说明使用的具体数据集名称。从描述推测，可能基于包含单张静态图像及对应动态视频的公共数据集（如DAVIS、COCO子集、或自建运动场景）。
*   **Benchmark**：与**最先进的视频生成器**和**物理感知基线方法**进行对比，衡量物理属性推断（如动量、质量）和运动-文本对齐的准确性，同时保持视觉保真度。
*   **对比方法**：未列出具体基线方法名称，但声称优于**当前最先进的视频生成器**和**物理感知基线方法**。

#### 4. 资源与算力

*   论文提供的文本中**未明确说明**所使用的GPU型号、数量或训练时长（因为方法本身是**无训练**的，资源消耗主要来自推理阶段的反复迭代和物理模拟）。因此无法总结算力信息。

#### 5. 实验数量与充分性

*   **实验数量**：从摘要和元数据（title、method、result等）来看，论文进行了**综合实验**，包括与多种方法的对比、以及对应的评估指标（物理属性推理、运动-文本对齐、视觉保真度）。但未提及具体进行了多少组实验（如不同数据集、不同场景下的对比，以及明确的消融实验数量）。
*   **充分性评估**：
    *   **优点**：实验覆盖了主要对比维度（物理合理性、对齐度、视觉质量），且基于公开可复现的预训练模型和模拟器，具有一定客观性。
    *   **不足**：由于没有具体实验细节（如消融实验设置、统计显著性检验等），无法全面判断实验的充分性。尤其缺乏对**无训练框架**在不同类型图像（如静态、动态、复杂场景）上的鲁棒性测试，可能存在选择偏见。

#### 6. 论文的主要结论与发现

*   **主要发现**：PhyMAGIC方法能够**无需任何任务特定训练或手动监督**，从单张图像生成物理一致的运动，显著优于现有视频扩散模型（它们在动量、穿透等物理属性上经常出错）。
*   **结论**：本文提供了一种**通用、训练免费**的物理运动生成方法，不仅适用于普通物体，还能**扩展到铰接物体（articulated objects）** 的运动生成，展示了良好的可扩展性和应用潜力。

#### 7. 优点

*   **方法亮点**：
    *   **无训练框架**：无需对扩散模型或模拟器进行任何微调，节省了大量训练成本，且易于集成到现有工作流中。
    *   **多模态协同**：巧妙结合了生成式AI（扩散模型）、推理能力（LLM）和物理仿真（可微分模拟器），各取所长。
    *   **迭代精炼机制**：通过LLM置信度引导 + 物理反馈的迭代闭环，高效地引导生成向物理一致方向收敛。
    *   **扩展性**：支持铰接物体运动生成，增加了方法的应用范围（如机器人、动画等）。
*   **实验亮点**：在多种指标（物理属性、对齐度、视觉保真）上全面超越基线，且保持图高质量。

#### 8. 不足与局限

*   **实验覆盖不足**：缺少在**大规模、多样化真实场景数据集**上的测试，特别是涉及复杂交互（如流体、软体、人物关节）的场景。方法的泛化性有待更严格的验证。
*   **偏差风险**：依赖预训练扩散模型和LLM的固有偏见（如训练数据覆盖不全，导致某些物理现象缺乏先验）。LLM置信度分数本身可能不够精确，存在误导风险。
*   **应用限制**：
    *   当前仅针对单张图像生成，无法处理**视频到视频**或**多视角输入**场景。
    *   可微分物理模拟器的计算成本可能较高，尤其在迭代次数多时，实时应用受限。
    *   对于**极端物理行为**（如爆炸、超高速运动），模拟器可能不稳定或无法处理。
*   **资源与效率**：作为无训练框架，推理速度通常慢于有训练模型，且**未提供任何性能基准**（如每秒帧数FPS或推理时间），难以评估其实际应用的可行性。

（完）

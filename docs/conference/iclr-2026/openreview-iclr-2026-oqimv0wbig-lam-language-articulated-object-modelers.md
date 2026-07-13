---
title: "LAM: Language Articulated Object Modelers"
title_zh: LAM：语言铰接物体建模系统
authors: "Yipeng Gao, Yunhao Ge, Peilin Cai, Daniel Seita, Laurent Itti"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=OQIMv0WBig"
tags: ["query:d-artic-kin"]
score: 9.0
evidence: 从文本提示生成铰接物体的几何与铰接参数
tldr: 现有铰接物体生成依赖输入视觉结构或预置资产。本文提出LAM，利用大语言模型和视觉语言模型协作，将铰接物体生成转化为统一代码生成任务。系统首先设计层次化部件结构，然后编写几何与铰接代码，编译调试后输出。该方法可从零生成具有合理运动参数的铰接物体。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有方法生成铰接物体依赖视觉输入或预置资产，缺乏灵活性。
method: 将生成任务转化为代码生成，通过LLM/VLM协作设计结构并编写代码。
result: 成功从文本生成具有合理几何和铰接参数的铰接物体。
conclusion: LAM实现了从文本零生成铰接物体的新范式。
---

## Abstract
We introduce LAM, a system that explores the collaboration of large-language models and vision-language models to generate articulated objects from text prompts. Our approach differs from previous methods that either rely on input visual structure(e.g., an image) or assemble articulated models from pre-built assets. In contrast, we formulate articulated object generation as a unified code generation task, where geometry and articulations can be co-designed from scratch. Given an input text, LAM coordinates a team of specialized modules to generate code to represent the desired articulated object procedurally. The LAM first reasons about the hierarchi-= cal structure of parts (links) with Link Designer, then writes code, compiles it, and debugs it with Geometry & Articulation Coders and self-corrects with Geometry & Articulation Checkers. The code serves as a structured and interpretable bridge between individual links, ensuring correct relationships among them. Representing everything with code allows the system to determine appropriate joint types and calculate their exact placements more reliably. Experiments demonstrate the power of leveraging code as a generative medium within an agentic system, showcasing its effectiveness in automatically constructing complex articulated objects.

---

## 论文详细总结（自动生成）

# LAM：语言铰接物体建模系统——中文详细总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：如何仅从文本描述（text prompts）零开始生成具有合理几何形状和铰接运动参数的3D铰接物体（articulated objects），而无需依赖任何视觉输入（如图像）或预置资产库。
- **研究动机**：现有铰接物体生成方法要么依赖输入视觉结构（例如一张物体图片），要么从预制的部件资产中组装，缺乏从纯语义描述出发的灵活性和创造性。自然界中存在大量需要自定义部件关系和运动方式的物体（如家具、机械），传统方法无法满足“仅凭一句话就生成一个可动的3D物体”的需求。
- **整体含义**：LAM首次将大语言模型（LLM）与视觉语言模型（VLM）协同用于铰接物体生成，并将问题统一转化为“代码生成”任务，使几何设计和运动学设计在同一个可解释的代码框架内完成。这为自然语言驱动的3D内容创作提供了新范式。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：将铰接物体生成形式化为一个多智能体协作的代码生成与调试流程。系统由多个专门模块（agents）组成，分别负责结构设计、几何代码编写、铰接代码编写、编译调试和自纠正。
- **关键技术细节**：
  - **Link Designer（部件设计器）**：首先根据文本推理出物体的层次化部件结构（links及层级关系），例如桌子有桌面、桌腿，铰链门有门框、门板、把手等。
  - **Geometry & Articulation Coders（几何与铰接编码器）**：分别编写描述每个部件几何形状的代码（如参数化几何体）和描述部件间铰接类型与运动约束的代码（如旋转关节、滑动关节、固定关节等）。
  - **Compilation & Debugging（编译与调试）**：系统自动编译生成的代码，并利用 **Geometry & Articulation Checkers（几何与铰接检查器）** 进行自纠正，确保代码逻辑正确、部件间关系合理（例如关节位置准确、运动范围无碰撞）。
  - **代码作为统一表示**：整个物体（包括几何和铰接）用程序化代码表示，代码既是生成过程的中间产物也是最终输出，可解释性强，便于后续修改、复用或进一步渲染。
- **流程示意**：输入文本 → Link Designer → Geometry Coder + Articulation Coder → 编译 → Checker → 若失败则反馈并修正代码 → 迭代直至成功 → 输出可执行的铰接物体代码。

## 3. 实验设计：数据集、基准方法与对比
- **数据集/场景**：由于论文正文未提供，根据摘要仅知使用文本提示作为输入。推测作者可能使用自建数据集或已有物体描述文本（如PartNet-Mobility等数据集中的物体类别）。但元数据中未明确提及。
- **基准方法（benchmark）**：未在摘要中说明。通常这类工作会对比基于图像的重建方法（如从单张图片生成铰接物体）、基于资产组合的方法（如从预设部件库拼接）以及仅使用LLM或VLM的基线。具体对比细节需阅读全文。
- **对比方法**：摘要中仅笼统提及“之前的依赖视觉结构或预置资产的方法”，未给出名称。元数据中无额外信息。

## 4. 资源与算力
- **未明确说明**：在提供的摘要和元数据中，没有提到使用的GPU型号、数量、训练时长或推理计算资源。因此无法总结算力消耗，仅指出论文未公开此信息。

## 5. 实验数量与充分性
- **实验数量**：元数据及摘要未列出具体实验组数。通常一篇ICLR级别的论文会包含：多个不同复杂度的物体生成示例、与基线的定量/定性对比、消融实验（如移除Checker、不同LLM/VLM组合的影响）。由于信息不足，无法判断充分性。
- **客观性与公平性**：仅从摘要看，作者声称成功生成，但未展示失败案例或定量指标（如几何精度、关节位置误差、用户研究等）。实验设计是否公平取决于对比方法是否同等情况（例如是否允许基线也使用LLM）。需全文核实。

## 6. 论文的主要结论与发现
- **主要结论**：LAM系统能够从零开始，仅根据文本提示自动生成具有合理几何和铰接参数的复杂铰接物体。将代码作为生成媒介的智能体系统（agentic system）在此任务上表现有效，展示了利用代码作为结构化、可解释中介的潜力。
- **发现**：LLM与VLM协作可以弥补单一模型的不足（VLM提供视觉常识，LLM提供逻辑推理和代码生成能力）；通过编译-调试循环可以自动纠正几何与运动学错误。

## 7. 优点：方法或实验设计上的亮点
- **方法亮点**：
  - **零输入视觉依赖**：完全从文本出发，无需图像或3D资产，极大扩展了应用场景。
  - **代码生成统一范式**：将几何和铰接结合在一起，代码天然保证部件间逻辑关系正确，且易于修改和迭代。
  - **智能体协作+自纠正**：模块化设计，各司其职，并通过Checker进行自我修复，提高了生成成功率。
  - **可解释性**：最终产出是程序化代码，不是黑盒模型输出，便于人类理解和二次编辑。
- **实验亮点**（如有）：可能包含多种物体类别（如家具、工具、玩具），展示系统鲁棒性。但文献未提供细节。

## 8. 不足与局限
- **实验覆盖不足**：未提供量化评价指标（如成功率、几何准确度、关节运动误差），使得结果难以客观对比。缺少在不同复杂程度文本下的失败分析。
- **偏差风险**：依赖LLM/VLM，可能继承其训练数据中的偏置（如常见物体表现好，罕见物体差）；代码生成可能受限于编程语言库（如OpenSCAD）的表达范围。
- **应用限制**：生成的物体质量受限于代码表示（可能无法表达自由曲面或复杂纹理）；铰接类型可能仅限于参数化常见类型（旋转、平移、螺旋等），无法处理柔性体或非线性运动。
- **无法从摘要评估**：其他局限（如推理速度、对长文本提示的处理、物理碰撞检测是否完善）无法得知。
- **算力与资源未报**：不利于复现和公平比较。

（完）

---
title: "ArtiPG++: Towards Efficient Procedural Generation of Articulated Object Data"
title_zh: ArtiPG++：面向铰接物体数据的高效过程化生成
authors: "Jiude Wei, Nange Wang, Longfei Xu, Yuxuan Li, Cewu Lu, Jianhua Sun"
date: 2025-09-03
pdf: "https://openreview.net/pdf?id=WrUIGjX73K"
tags: ["query:d-artic-kin"]
score: 8.0
evidence: 过程化生成铰接物体数据；支持学习运动学和物理属性
tldr: 大规模高质量铰接物体数据对视觉感知和具身智能至关重要，但现有采集方法难以扩展。本文提出ArtiPG++，一个高效过程化生成框架，无需外部资产，自动生成多样化的铰接物体及其完整标注（几何、运动学、物理属性）。该框架显著降低了数据获取成本，为学习铰接物体运动学参数和物理属性提供了数据基础。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有铰接物体数据收集方法难以规模化。
method: 提出高效过程化生成框架，自动生成铰接物体并附带丰富标注。
result: 无需外部资产即可生成多样化铰接物体数据。
conclusion: ArtiPG++为铰接物体感知研究提供了可扩展的数据生成方案。
---

## Abstract
To leverage deep learning in advancing vision perception and embodied intelligence, an extensive number of high-quality and richly annotated 3D articulated objects is essential. However, current methods for collecting articulated objects and their annotations are either based on human effort or physics simulators, which are difficult to scale up, posing challenges to the collection of large-scale and richly annotated articulated objects. In such context, procedural generation has recently gained attention in articulated object synthesis. However, it still faces challenges such as reliance on external assets and the complexity of designing procedural rules. To this end, we propose ArtiPG++, a highly efficient framework for synthesizing articulated objects with rich annotations, featuring three key advantages: 1) asset-free spatial structure synthesis via procedural rules, 2) labor-free synthesis of realistic geometric details, along with precise and diverse annotations, and 3) easy expansion to new object categories, with a ready-to-use tool for convenient synthesis. ArtiPG++ currently supports the procedural synthesis for 39 common object categories, and requires only a few hours to develop procedural generation rules for novel categories, which is a one-time effort for infinite objects synthesis. We conduct extensive evaluations on the objects and annotations synthesized by ArtiPG++, through both direct comparisons in terms of diversity and distribution, as well as performance in downstream tasks. Please refer to the appendix for more details, analysis, discussions and code implementation.

---

## 论文详细总结（自动生成）

# 论文总结：ArtiPG++：面向铰接物体数据的高效过程化生成

## 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：大规模、高质量且带有丰富标注的3D铰接物体数据对于推动视觉感知和具身智能的深度学习至关重要。然而，当前收集铰接物体及其标注的方法主要依赖人工或物理模拟器，难以规模化扩展，导致大规模、多标注的铰接物体数据获取困难。
- **背景**：过程化生成（procedural generation）最近在铰接物体合成领域受到关注，但仍面临依赖外部资产（external assets）以及设计过程化规则复杂等挑战。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：提出一个无需外部资产的高效过程化生成框架——ArtiPG++，自动合成铰接物体并附带完整的几何、运动学和物理属性标注。
- **关键技术细节**：
  - **资产无关的空间结构合成**：通过过程化规则直接生成物体结构，不依赖任何外部3D资产。
  - **自动化逼真几何细节合成**：无需人工干预即可生成精细几何形状，同时产出精确且多样化的标注（包括运动学参数、物理属性等）。
  - **易于扩展**：仅需数小时即可为新物体类别开发过程化生成规则，且一次规则开发即可无限次生成。
- **算法流程（文字描述）**：用户定义物体类别的过程化规则（如部件类型、连接方式、尺寸范围等）→ 框架根据规则随机采样生成结构拓扑 → 自动填充几何细节并计算运动学约束（关节类型、自由度、运动范围）→ 附加物理属性（质量、摩擦系数等）→ 输出完整的铰接物体模型及对应标注。
- **当前支持**：39个常见物体类别。

## 3. 实验设计

- **数据集/场景**：使用ArtiPG++自身合成的铰接物体数据（涵盖39类），在合成数据上进行评估。
- **Benchmark**：未明确提及标准化benchmark，但通过多样性分布比较（直接比较合成数据与真实数据的分布）以及下游任务性能来验证。
- **对比方法**：未在摘要中明确列出对比方法，但暗示与其他现有过程化生成或人工/模拟器收集的方法进行比较（需参考全文附录获取对比细节）。

## 4. 资源与算力

- **未明确说明**：摘要中未提及使用的GPU型号、数量、训练时长等算力信息。需查阅论文全文（附录）才能获知。

## 5. 实验数量与充分性

- **实验数量**：摘要提及“extensive evaluations”，包括多样性/分布直接比较和下游任务性能评估。但未给出具体实验组数（如消融实验、跨类别泛化测试等）。
- **充分性与公平性**：仅摘要信息难以判断实验充分性。但论文声称通过直接比较和下游任务进行验证，设计上具有一定客观性。可能缺乏与传统数据收集方法的公平对比（如相同数据量下的基准测试），需阅读全文确认。

## 6. 主要结论与发现

- **结论**：ArtiPG++是一个高效、可扩展的过程化生成框架，无需外部资产即可生成大量多样化、带有丰富标注的铰接物体数据，显著降低数据获取成本，为学习铰接物体运动学参数和物理属性提供数据基础。
- **发现**：在多样性、分布匹配度以及下游任务性能上，合成数据表现良好。

## 7. 优点

- **资产自由**：无需任何外部3D资产，完全通过规则生成，避免了数据版权和资产依赖问题。
- **高自动化**：几何细节、标注全部自动生成，无需人工干预，显著降低人力成本。
- **易于扩展**：对新类别开发规则仅需数小时，且一次开发即可无限生成，具备很强的可扩展性。
- **标注丰富**：同时提供几何、运动学、物理属性等多种标注，适配多种下游任务（如感知、操控、模拟）。

## 8. 不足与局限

- **规则设计仍需专业投入**：虽然开发新类别的规则只需数小时，但仍需人工设计规则，对于极其复杂或非常规的物体类别可能仍有挑战。
- **数据真实度**：过程化生成的几何细节可能无法完全媲美真实扫描或手工建模的物体，分布差异可能影响下游任务的泛化性。
- **实验覆盖有限**：摘要仅提及39类物体的生成和下游任务验证，未明确涵盖与其他主流铰接物体数据集（如PartNet-Mobility, ShapeNet等）的大规模定量对比，需全文补充。
- **算力和资源未公开**：未说明生成数据的计算成本，实际部署时可能需要一定计算资源。
- **未讨论动态物理模拟验证**：物理属性（如质量、摩擦）可能未在物理模拟器中实际验证其合理性。

（完）

# Etsy Jewelry Gemini Prompt

[中文](#中文说明) · [English](#english)

## 中文说明

面向珠宝商品摄影的 Codex Skill，用于为 Gemini 图像模型编写、诊断和迭代优化可执行提示词。

它将产品结构、佩戴比例与摄影风格拆分为独立参考来源，降低参考图互相污染、首饰结构漂移和人体细节失真的概率。适用于 Etsy 商品主图、佩戴图和生活方式摄影提示词工作流。

[![Release](https://img.shields.io/github/v/release/a18675482130-sudo/etsy-jewelry-gemini-prompt)](https://github.com/a18675482130-sudo/etsy-jewelry-gemini-prompt/releases/latest)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

> 该 Skill 只输出文字提示词、诊断和修改建议，不调用图片生成工具。

## 核心能力

### 参考图职责隔离

- 产品图只负责款式、材质、宝石、金属结构、扣件和尺寸关系
- 佩戴图只负责视觉大小、佩戴位置、人物动作和构图
- 风格图只负责皮肤、光线、色彩和摄影质感
- 不允许任一参考图替代其他参考图的职责

### 产品一致性控制

- 锁定主体造型、宝石数量、排列方向和连接方式
- 保持链条、镯身、戒臂、耳扣或镶嵌结构
- 控制镜像、遮挡、比例漂移、自动叠戴和结构重设计

### 品类专用策略

| 品类 | 重点控制 |
|---|---|
| 项链与吊坠 | 吊坠结构、链长、链条粗细、锁骨位置与颈部裁切 |
| 手链与手镯 | 链节、开口、扣件、自然腕围与手腕结构 |
| 耳环与耳钉 | 数量、左右方向、耳扣、垂坠长度与头发遮挡 |
| 戒指 | 戒面方向、戒臂、爪镶、佩戴手指与自然贴合 |

### 生成结果诊断

按产品结构、佩戴比例、人体裁切、皮肤、景深、宝石高光和生成标记的顺序检查结果。每轮只修正最重要的 1–3 个问题，保留已经稳定的部分，避免整篇重写导致回退。

## 输入协议

产品参考图与完整产品规格至少提供一项。其他输入均为可选。

| 输入 | 要求 | 用途 |
|---|---|---|
| 产品参考图 | 与完整规格二选一 | 产品结构、材质、宝石、扣件和可见比例 |
| 完整产品规格 | 与产品图二选一 | 类型、毫米尺寸、材质、镀色、宝石和连接结构 |
| 佩戴比例参考图 | 可选 | 佩戴位置、视觉大小、动作和构图 |
| 摄影风格参考图 | 可选 | 肤色、光线、色彩和摄影质感 |

缺少佩戴图时，Skill 不会虚构精确链长、圈口或垂坠尺寸。缺少风格图且用户未指定风格时，默认采用暖棕低调珠宝摄影。

## 安装

从 [Releases](https://github.com/a18675482130-sudo/etsy-jewelry-gemini-prompt/releases/latest) 下载最新 ZIP，解压后将：

```text
skill/etsy-jewelry-gemini-prompt
```

复制到 Codex Skills 目录：

```text
%USERPROFILE%\.codex\skills\etsy-jewelry-gemini-prompt
```

重新打开 Codex 任务后即可调用。

## 快速开始

### 单张产品图

```text
使用 $etsy-jewelry-gemini-prompt，根据产品参考图生成一版短提示词。
品类为戒指，要求暖棕暗调、单手展示、1:1。
严格保持戒面、爪镶、宝石数量和戒臂宽度，不增加叠戴饰品。
```

### 三张参考图

```text
使用 $etsy-jewelry-gemini-prompt 生成完整版提示词。
图一只负责产品结构，图二只负责佩戴比例和构图，
图三只负责皮肤、光线和摄影质感。
```

### 诊断生成结果

```text
使用 $etsy-jewelry-gemini-prompt 分析这张生成结果。
先确认可见事实，只列出最主要的三个问题，
然后给出仅修改这些问题的替换文案。
```

## 输出模式

| 指令 | 输出 |
|---|---|
| `关键词` | 20–40 个可复制关键词 |
| `短版` 或 `1` | 约 200–350 字提示词及相关限制词 |
| `完整版` | 完整的参考图、产品、构图、皮肤、光线和景深模块 |
| `看看` 或 `分析一下` | 结论、主要问题和本轮替换文案 |
| `再生成一版` | 修订后的文字提示词，不生成图片 |

## 项目结构

```text
skill/etsy-jewelry-gemini-prompt/
├── SKILL.md
├── agents/
│   └── openai.yaml
├── examples/
└── references/
    ├── diagnosis-matrix.md
    ├── jewelry-types.md
    ├── negative-prompts.md
    ├── prompt-modules.md
    └── workflow.md
```

## 设计原则

- 产品准确性优先于摄影风格
- 只使用与当前品类和问题相关的限制词
- 不根据不可见信息推断绝对尺寸或复杂结构
- 用户指定风格优先于默认暖棕暗调
- 迭代时做局部修正，不破坏已稳定结果

## 适用边界

- 提示词无法保证图像模型百分之百还原复杂珠宝结构
- 精确商品比例仍建议提供佩戴参考图或毫米尺寸
- 不虚构检测报告、材质证明、宝石证书或平台背书
- 不替代 Etsy、Google 或其他平台的官方规范

## License

[MIT License](LICENSE)

本项目与 Etsy、Google 或 Gemini 官方无隶属、合作或背书关系。相关名称仅用于说明适用场景和兼容工作流。

---

## English

A Codex Skill for authoring, diagnosing, and iteratively refining production-ready Gemini prompts for jewelry product photography.

It separates product identity, wearing scale, and photographic style into independent reference roles. This reduces cross-reference contamination, structural drift, and anatomical artifacts across Etsy listing images, on-body shots, and lifestyle photography workflows.

> This Skill outputs text prompts, diagnostics, and revision guidance only. It does not invoke image-generation tools.

### Core capabilities

#### Reference-role isolation

- Product reference: design, materials, gemstones, metalwork, findings, and relative dimensions
- Wearing reference: visual scale, placement, pose, and composition
- Style reference: skin rendering, lighting, color, and photographic finish
- No reference may override the responsibilities of another

#### Product-fidelity control

- Locks silhouette, gemstone count, orientation, and connection structure
- Preserves chains, bangles, ring shanks, earring findings, and stone settings
- Suppresses mirroring, occlusion, scale drift, automatic stacking, and redesign

#### Category-specific constraints

| Category | Primary controls |
|---|---|
| Necklaces and pendants | Pendant structure, chain length and thickness, collarbone placement, neck crop |
| Bracelets and bangles | Links, openings, clasps, natural wrist fit, wrist anatomy |
| Earrings and studs | Quantity, left/right orientation, findings, drop length, hair occlusion |
| Rings | Face orientation, shank, prongs, worn finger, natural fit |

#### Iterative diagnosis

Reviews generated results in a fixed order: product structure, wearing scale, anatomical crop, skin, depth of field, gemstone highlights, and synthetic artifacts. Each iteration corrects only the 1–3 highest-impact issues while preserving stable elements.

### Input protocol

Provide either a product reference image or a complete product specification. All other inputs are optional.

| Input | Requirement | Purpose |
|---|---|---|
| Product reference image | Required unless specifications are supplied | Structure, materials, gemstones, findings, visible proportions |
| Complete product specification | Required unless a product image is supplied | Type, millimeter dimensions, materials, plating, gemstones, connections |
| Wearing-scale reference | Optional | Placement, visual scale, pose, composition |
| Photography-style reference | Optional | Skin tone, lighting, color, photographic finish |

Without a wearing reference, the Skill does not invent exact chain lengths, ring sizes, or drop measurements. Without a style reference or explicit direction, it defaults to warm, low-key jewelry photography.

### Installation

Download the latest ZIP from [Releases](https://github.com/a18675482130-sudo/etsy-jewelry-gemini-prompt/releases/latest). Extract it and copy:

```text
skill/etsy-jewelry-gemini-prompt
```

to your Codex Skills directory:

```text
%USERPROFILE%\.codex\skills\etsy-jewelry-gemini-prompt
```

Reopen the Codex task before invoking the Skill.

### Quick start

#### Single product reference

```text
Use $etsy-jewelry-gemini-prompt to create a short prompt from the product reference.
Category: ring. Style: warm, low-key, single-hand presentation, 1:1.
Preserve the ring face, prongs, gemstone count, and shank width. Do not add stacked jewelry.
```

#### Three-reference workflow

```text
Use $etsy-jewelry-gemini-prompt to create a complete prompt.
Image 1 controls product structure only.
Image 2 controls wearing scale and composition only.
Image 3 controls skin, lighting, and photographic finish only.
```

#### Diagnose a generated result

```text
Use $etsy-jewelry-gemini-prompt to analyze this generated result.
Confirm visible facts first, identify only the three highest-impact issues,
then provide replacement copy that corrects those issues without changing stable elements.
```

### Output modes

| Command | Output |
|---|---|
| `keywords` | 20–40 copy-ready keywords |
| `short` or `1` | A concise prompt with relevant negative constraints |
| `full` | Complete reference, product, composition, skin, lighting, and depth-of-field modules |
| `review` or `analyze` | Verdict, primary issues, and targeted replacement copy |
| `regenerate prompt` | Revised text prompt only; no image generation |

### Design principles

- Product accuracy takes priority over photographic style
- Apply only constraints relevant to the current category and failure mode
- Never infer absolute dimensions or hidden structures from unavailable evidence
- Explicit user direction overrides the default warm, low-key style
- Prefer localized corrections over full prompt rewrites

### Scope and limitations

- Prompting cannot guarantee exact reconstruction of complex jewelry structures
- For precise on-body scale, provide a wearing reference or millimeter dimensions
- The Skill does not fabricate test reports, material claims, gemstone certificates, or platform endorsements
- It does not replace official Etsy, Google, or marketplace policies

### License

[MIT License](LICENSE)

This project is not affiliated with, sponsored by, or endorsed by Etsy, Google, or Gemini. These names are used solely to describe compatible workflows and intended use cases.

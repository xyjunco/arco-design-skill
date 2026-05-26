# Arco Design Skill

中文 | [English](#english)

## 中文

`arco-design` 是一个面向 Figma 原型设计的可复用 Skill，可作为 Codex skill 安装使用。它用于在使用 **Arco Design System (New)** 时保持组件选择、variant 语义、状态表达、布局质量和页面结构的一致性。

它不绑定具体业务场景，沉淀的是可复用的中后台设计规则：如何选择 Arco 组件，如何理解组件属性，如何避免手绘控件和错误状态映射，以及如何在 Figma 中输出高度组件化、可复核的原型。

## 适用场景

- 使用 Arco Design System (New) 绘制中后台产品原型
- 从 PRD 或说明生成 Figma 页面、表单、列表、弹窗、详情页
- 需要保持原型高度组件化，优先复用 Arco assets
- 需要把复杂输入拆成低出错成本的结构化控件
- 需要在生成后做布局、分页、表格、Tag、弹窗和文字溢出的质量检查

## 核心能力

- 约束 Arco Design System (New) 的组件来源和 library key
- 总结 Button、Input、Select、Tag、Table、Pagination、Alert、Modal、Drawer 等组件的属性语义
- 区分交互态和业务态，避免用禁用、悬停、聚焦等状态表达业务结果
- 规定表单 label/control 栅格、列表页 content rail、表格列宽、分页吸底、Tag 内容 hug 等布局规则
- 要求范围、过滤、DSL、表达式等高出错成本输入优先拆成条件构建器
- 要求表达式和变量插值编辑区提供可复制变量清单
- 提供最终交付前的设计检查清单

## 安装

将仓库放到 Codex skills 目录中：

```bash
mkdir -p ~/.codex/skills
git clone https://github.com/xyjunco/arco-design-skill.git ~/.codex/skills/arco-design
```

如果已经存在本地 skill，可以直接更新 `SKILL.md`：

```bash
cd ~/.codex/skills/arco-design
git pull --ff-only
```

## 使用方式

在任务中明确提到 `arco-design`，或让 Codex 使用 Arco Design System (New) 生成 Figma 原型：

```text
请加载 arco-design skill，基于这份 PRD 重新绘制 Figma 原型。
```

或：

```text
请按照 Arco Design System (New) 规范，修复这个中后台页面的表格、分页和表单布局。
```

## 设计原则摘要

- 先复用 Arco New 组件，再做业务布局。
- 先理解组件属性语义，再修改 variant。
- 组件状态只表达交互过程，不表达业务生命周期。
- 业务状态应通过 Tag、badge/status、Alert、结果面板或文案表达。
- 表单、表格、分页、弹窗、变量输入等复杂区域必须经过截图级 QA。

## 文件结构

```text
arco-design-skill/
├── SKILL.md
└── README.md
```

`SKILL.md` 是 Codex 实际加载的技能文件。`README.md` 仅用于 GitHub 说明，不是运行时必需内容。

---

## English

`arco-design` is a reusable skill for Figma prototyping with **Arco Design System (New)**. It can be installed as a Codex skill, and it keeps component selection, variant semantics, state communication, layout quality, and page structure consistent when generating or refining admin-style product prototypes.

The skill is business-agnostic. It captures reusable design rules for choosing Arco components, interpreting component properties, avoiding hand-drawn controls, separating interaction states from business states, and producing highly componentized Figma prototypes.

## Use Cases

- Design admin or internal-tool prototypes with Arco Design System (New)
- Generate Figma pages, forms, tables, dialogs, drawers, and detail views from PRDs
- Keep prototypes componentized and aligned with Arco assets
- Replace error-prone free-text inputs with structured controls
- Run QA for layout, pagination, table columns, tags, dialogs, and text overflow

## Key Capabilities

- Defines the canonical Arco Design System (New) library source and library key
- Documents component semantics for Button, Input, Select, Tag, Table, Pagination, Alert, Modal, Drawer, and more
- Separates interaction states from business states
- Defines layout rules for form label/control grids, list content rails, table widths, sticky-bottom pagination, content-hugging tags, and modal spacing
- Requires structured condition builders for range, filter, DSL, and expression inputs when free text would be costly or error-prone
- Requires copyable variable references near expression and template editors
- Provides a delivery checklist for design QA

## Installation

Clone this repository into the Codex skills directory:

```bash
mkdir -p ~/.codex/skills
git clone https://github.com/xyjunco/arco-design-skill.git ~/.codex/skills/arco-design
```

If the skill already exists locally, update it with:

```bash
cd ~/.codex/skills/arco-design
git pull --ff-only
```

## Usage

Mention `arco-design` explicitly in a task, or ask Codex to generate a Figma prototype with Arco Design System (New):

```text
Load the arco-design skill and redraw this PRD as a Figma prototype.
```

Or:

```text
Fix this admin page according to Arco Design System (New), especially tables, pagination, and form layout.
```

## Design Principles

- Reuse Arco New components before creating business-specific layouts.
- Understand component property semantics before changing variants.
- Use component states for interaction states only, not business lifecycle states.
- Express business states with Tag, badge/status, Alert, result panels, or copy.
- Run screenshot-level QA for complex areas such as forms, tables, pagination, dialogs, and expression inputs.

## Repository Layout

```text
arco-design-skill/
├── SKILL.md
└── README.md
```

`SKILL.md` is the skill file loaded by Codex. `README.md` is repository documentation for GitHub and is not required at runtime.

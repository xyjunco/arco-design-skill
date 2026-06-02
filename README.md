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
- 约束团队图表库的组件来源和使用场景
- 总结 Button、Input、Select、Tag、Table、Pagination、Alert、Modal、Drawer 等组件的属性语义
- 区分交互态和业务态，避免用禁用、悬停、聚焦等状态表达业务结果
- 规定表单 label/control 栅格、列表页 content rail、表格列宽、分页吸底、Tag 内容 hug 等布局规则
- 要求范围、过滤、DSL、表达式等高出错成本输入优先拆成条件构建器
- 要求表达式和变量插值编辑区提供可复制变量清单
- 要求 Select、Input、Button 等组件的内容写入实例本身，禁止用 cover/overlay 伪装文本或样式
- 要求页面 frame、Card、Modal、Drawer、Table、Chart 容器必须包住可见内容，不允许裁切或内容浮出容器
- 提供最终交付前的设计检查清单

## 必需依赖

使用本 skill 生成或修复 Figma 原型前，执行者必须通过 Figma connector 自动检查并加载以下团队 library。用户不需要手工下载组件库，也不需要本地安装 Figma Desktop。

| Library | 用途 | Library Key |
|---|---|---|
| Arco Design System (New) | 页面框架、表单、表格、按钮、导航、状态、浮层、反馈组件 | `lk-1fdff400a519972aa8ca99185d4b6a6105f88f62ea6ba8ee2ef0938635f787d47223212bf8899cf7d82847743cca58a67cbae1d9ebc9f7ba26f35df5c323e3b9` |
| 源力设计体系-图表库 | 折线、面积、柱状、Toplist、饼图、环形图等数据可视化组件 | `lk-a3da2a4c46bf632b0087bf59bbf692c935855e5e43ccdc92b9bc4d9211e6480d468df1801feddcc717033d4eb400c04580f8d7c6f817fd9db44f597ec0727931` |

这两个依赖是团队 Figma library，不应复制进 git。仓库只保存依赖说明、library key、组件使用规则和预检流程。

## Figma 使用流程

写入 Figma 时必须先加载 `figma-use`，再执行以下流程：

1. 通过 `get_libraries`、`search_design_system` 或等价能力确认两个团队 library 可访问。
2. 搜索 Arco 组件时限定 `includeLibraryKeys` 为 Arco library key；搜索图表组件时限定为图表库 library key。
3. 按页面需要懒加载真实组件实例，不一次性导入整库。
4. 修改组件属性或实例内部文本层来写入内容。
5. 如果 library 不可访问、组件 key 失效或当前文件只读，应停止相关绘制并说明权限或依赖缺口，不要手绘替代。

不需要本地 Figma Desktop。只要当前会话有 Figma connector、目标文件编辑权限，并且账号能访问团队 library，即可在云端 Figma 文件中生成原型。

## 组件实例规则

禁止使用以下方式伪装组件：

- 在 `Arco / select` 上覆盖 `Select cover` 来显示选项文本。
- 在 `Arco / input` 上覆盖 `Input cover` 来显示输入值。
- 在 `Arco / button` 上覆盖 `Button text cover` 来显示按钮文案。
- 使用外部文本、矩形、假箭头或手绘菜单伪装 Select、Dropdown、Input、Button、Table、Pagination、Tag、Alert、Modal、Drawer。

正确方式：

- 先使用 Arco 公开父组件实例。
- 通过组件 property 写入可配置文本、尺寸、状态、类型和显隐项。
- 当组件没有暴露对应 property 时，修改实例内部真实文本层；不要在实例外覆盖文本。
- 下拉展开态应使用真实 dropdown/selection 组件或对应弹层结构。
- 图表必须使用 `源力设计体系-图表库`，不手绘折线、柱状、饼图、坐标轴、图例和 tooltip。

## 布局 QA 规则

交付前必须检查：

- 页面 frame 高度随内容收口或增高，不能把内容留在 frame 外。
- Card、Modal、Drawer、Popover、Alert、Table 和图表容器必须包住全部可见内容。
- 表格、提示、分页、按钮、图表、图例、坐标和 tab 不得浮在容器外。
- 不用 clipping mask 掩盖布局错误；图表必须完整展示主体、图例、坐标、标签和数值。
- 表单 label/control、筛选栏、表格列、分页与内容 rail 要对齐。
- Tag 应 hug content，不能前后留大量空白。
- 状态颜色必须和业务语义一致，失败/危险不可使用成功色。
- 截图 QA 里发现裁切、重叠、错位、按钮异常整片色或文本溢出时，必须回到结构和组件属性修复，而不是叠 cover。

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
- 内容必须写入真实组件实例，不用 cover/overlay 伪装组件文本和样式。
- 图表必须使用团队图表库组件，不手绘数据可视化。
- 容器高度和 auto layout 必须服务内容完整展示，不靠裁切遮错。
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
- Defines the required team chart library source and usage scenarios
- Documents component semantics for Button, Input, Select, Tag, Table, Pagination, Alert, Modal, Drawer, and more
- Separates interaction states from business states
- Defines layout rules for form label/control grids, list content rails, table widths, sticky-bottom pagination, content-hugging tags, and modal spacing
- Requires structured condition builders for range, filter, DSL, and expression inputs when free text would be costly or error-prone
- Requires copyable variable references near expression and template editors
- Requires Select, Input, Button, and similar component content to be written into the real instance instead of cover or overlay layers
- Requires page frames and containers such as Card, Modal, Drawer, Table, and Chart to enclose all visible content without clipping
- Provides a delivery checklist for design QA

## Required Dependencies

Before generating or repairing a Figma prototype with this skill, the agent must use the Figma connector to check and load these team libraries automatically. Users do not need to download libraries manually or install Figma Desktop locally.

| Library | Purpose | Library Key |
|---|---|---|
| Arco Design System (New) | Page structure, forms, tables, buttons, navigation, states, overlays, and feedback components | `lk-1fdff400a519972aa8ca99185d4b6a6105f88f62ea6ba8ee2ef0938635f787d47223212bf8899cf7d82847743cca58a67cbae1d9ebc9f7ba26f35df5c323e3b9` |
| 源力设计体系-图表库 | Line, area, bar, toplist, pie, donut, and other data visualization components | `lk-a3da2a4c46bf632b0087bf59bbf692c935855e5e43ccdc92b9bc4d9211e6480d468df1801feddcc717033d4eb400c04580f8d7c6f817fd9db44f597ec0727931` |

These dependencies are team Figma libraries and should not be copied into git. The repository stores only dependency documentation, library keys, component usage rules, and the preflight workflow.

## Figma Workflow

When writing to Figma, load `figma-use` first, then follow this workflow:

1. Use `get_libraries`, `search_design_system`, or an equivalent capability to confirm that both team libraries are available.
2. Search Arco components with `includeLibraryKeys` restricted to the Arco library key; search chart components with the chart library key.
3. Lazily import real component instances as needed for the page. Do not import the whole library.
4. Write content through component properties or real text layers inside the instance.
5. If the library is unavailable, a component key is stale, or the target file is read-only, stop the relevant drawing work and report the missing permission or dependency. Do not hand-draw substitutes.

Figma Desktop is not required. Cloud Figma files can be generated through the Figma connector as long as the session is authenticated, the target file is editable, and the account has access to the team libraries.

## Component Instance Rules

Do not fake components with these patterns:

- Adding a `Select cover` over `Arco / select` to show selected text.
- Adding an `Input cover` over `Arco / input` to show input content.
- Adding a `Button text cover` over `Arco / button` to show button copy.
- Using external text, rectangles, fake arrows, or hand-drawn menus to imitate Select, Dropdown, Input, Button, Table, Pagination, Tag, Alert, Modal, or Drawer.

Correct approach:

- Use the public Arco parent component instance first.
- Set text, size, state, type, and visibility through component properties where available.
- If the component does not expose the needed property, edit the real text layer inside the instance instead of placing text above it.
- Use real dropdown/selection components or the corresponding overlay structure for expanded select states.
- Use the team chart library for charts. Do not hand-draw lines, bars, pies, axes, legends, or tooltips.

## Layout QA Rules

Before delivery, verify:

- Page frames shrink or grow to contain content; content must not sit outside the frame.
- Card, Modal, Drawer, Popover, Alert, Table, and chart containers enclose all visible content.
- Tables, alerts, pagination, buttons, charts, legends, axes, and tabs do not float outside their containers.
- Do not hide layout issues with clipping masks. Charts must show the plot, legends, axes, labels, and values completely.
- Form label/control grids, filter bars, table columns, pagination, and content rails are aligned.
- Tags hug their content and do not include large empty padding.
- Status colors match business semantics. Failure and destructive states must not use success colors.
- If screenshot QA finds clipping, overlap, misalignment, abnormal full-blue buttons, or text overflow, fix the structure and component properties instead of adding cover layers.

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
- Write content into real component instances. Do not fake component text or styles with cover or overlay layers.
- Use team chart library components for data visualization.
- Container height and auto layout must make content fully visible; do not use clipping to hide layout errors.
- Run screenshot-level QA for complex areas such as forms, tables, pagination, dialogs, and expression inputs.

## Repository Layout

```text
arco-design-skill/
├── SKILL.md
└── README.md
```

`SKILL.md` is the skill file loaded by Codex. `README.md` is repository documentation for GitHub and is not required at runtime.

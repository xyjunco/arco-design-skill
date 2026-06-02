---
name: arco-design
description: 使用 Arco Design System (New) 进行 Figma 原型设计时必须遵守的组件资产选择、variant 语义、状态信息传达、组合边界、数据可视化图表选择和高组件化输出规范。
memory_kind: figma_design_system_usage
library_name: Arco Design System (New)
library_key: lk-1fdff400a519972aa8ca99185d4b6a6105f88f62ea6ba8ee2ef0938635f787d47223212bf8899cf7d82847743cca58a67cbae1d9ebc9f7ba26f35df5c323e3b9
library_preload_required: true
library_preload_method: figma-use
required_figma_libraries:
  - name: Arco Design System (New)
    role: ui_components
    library_key: lk-1fdff400a519972aa8ca99185d4b6a6105f88f62ea6ba8ee2ef0938635f787d47223212bf8899cf7d82847743cca58a67cbae1d9ebc9f7ba26f35df5c323e3b9
    scope: team_library
    required: true
  - name: 源力设计体系-图表库
    role: data_visualization_components
    library_key: lk-a3da2a4c46bf632b0087bf59bbf692c935855e5e43ccdc92b9bc4d9211e6480d468df1801feddcc717033d4eb400c04580f8d7c6f817fd9db44f597ec0727931
    scope: team_library
    required: true
source_file_key: I37eNx2fIvGS0sl4fVNHmH
generated_at: 2026-05-26
updated_at: 2026-06-02
language: zh-CN
purpose: "长期复用 Arco Design System (New) 在 Figma 原型中的组件选择、variant 语义、信息表达、数据可视化图表选择和组件化实施规则"
---

# Arco Design System (New) 抽象使用规范

## 0. 使用原则

本技能沉淀 Arco Design System (New) 的组件资产、属性语义和原型组件化规则，并补充已订阅的源力数据可视化图表库使用规则，不绑定任何具体业务。后续使用 Arco 生成 Figma 原型时，先读取本文件，再按目标产品场景选择组件。

### 0.1 必选依赖与自动加载顺序

使用本技能进行 Figma 原型设计前，执行者必须通过 Figma 工具自动确认并加载两个团队 library 依赖。不要把这个动作转嫁给用户手工完成；用户使用技能时不应该卡在“先去加载组件库”这一步。

| 依赖 | library key | 用途 | 缺失时处理 |
|---|---|---|---|
| `Arco Design System (New)` | `lk-1fdff400a519972aa8ca99185d4b6a6105f88f62ea6ba8ee2ef0938635f787d47223212bf8899cf7d82847743cca58a67cbae1d9ebc9f7ba26f35df5c323e3b9` | 团队 UI 组件库：页面框架、表单、表格、按钮、导航、状态、浮层、反馈组件 | 停止绘制并说明团队 Arco library 不可访问，不用手绘替代 |
| `源力设计体系-图表库` | `lk-a3da2a4c46bf632b0087bf59bbf692c935855e5e43ccdc92b9bc4d9211e6480d468df1801feddcc717033d4eb400c04580f8d7c6f817fd9db44f597ec0727931` | 团队数据可视化组件库：折线、面积、柱状、Toplist、饼图、环形图等 | 若页面需要图表则停止绘制并说明团队图表 library 不可访问，不手绘图表 |

自动加载协议：

1. 若需要写入或检查 Figma 文件，先加载并遵守 `figma-use` 技能；调用 `use_figma` 时必须传 `skillNames: "figma-use"`。
2. 在绘制任何页面前，先用当前 Figma 工具的 `get_libraries`、`search_design_system` 或等价能力确认两个 library 对当前文件可用。
3. 搜索 Arco 组件时必须限定 `includeLibraryKeys` 为 Arco library key；搜索图表组件时必须限定为图表库 library key。
4. 不要一次性导入整个组件库。按页面需要懒加载组件：先搜索目标组件，再用返回的 component/component set key 导入实例。
5. 对高频基础组件，可在首个 `use_figma` 写入调用前做探测性预加载：例如 Arco 的 `Button`、`input`、`select`、`table`，图表库的 `折线图`、`Toplist 条形图`。探测只用于确认依赖可用，不在画布上留下测试节点。
6. 若搜索结果没有返回目标 `libraryName`、无法导入组件，或当前账号/文件没有团队 library 访问权限，不要退化为矩形、文本、手绘图标或 cover 方案；应停止相关绘制并报告团队 library 权限或组件缺口。
7. 只有在依赖已确认可用且目标组件可导入后，才开始创建页面、导入组件、修改实例属性和做截图 QA。

`figma-use` 能自动完成的是“检查团队可用库、搜索组件、按 key 导入组件实例”；不能绕过 Figma 权限、library 未对当前文件启用或 library 已被删除的情况。遇到团队 library 权限或启用缺口时，正确行为是报告缺口，而不是手绘替代。

### 0.2 依赖打包边界

这两个依赖已经是团队 Figma library，应作为正式运行依赖使用，并通过 library key 自动预检和按需导入。不要把 Figma library 误当成本地 npm 包或静态资源直接打进 git。Figma library 的组件身份由 Figma 云端文件、发布状态、组件 key、团队权限共同决定；把截图、导出的 `.fig` 文件或组件描述放进仓库，并不能让目标用户自动拥有同一个 library，也不能保持原有 component key。

不可行或不推荐：

- 不要把团队 Figma library 复制进 git，声称用户可以直接本地加载；团队 library 应通过 Figma 权限和 library key 加载。
- 不要用导出的 `.fig` 文件替代 library 依赖；导入后会生成新的文件和新的 component key，原 skill 中记录的 key 不会自动匹配。
- 不要把组件截图、SVG、JSON 描述当作 Arco 组件来源；这样会退化成不可维护的手绘资产。
- 不要为了“自包含”把 Arco 控件重画成 repository 内的私有组件，除非明确要建设一套新的设计系统。

推荐做法：

1. 在 skill 中保存依赖 manifest：library 名称、library key、用途、是否必需、缺失时处理方式。
2. 每次绘制前通过 `figma-use`/Figma 工具自动 preflight：确认 library 可见、按 key 搜索组件、按需导入实例。
3. 如果组织要离线或跨团队复用，复制并维护一个新的受控 Figma design system 文件，发布为目标团队 library，再把新 library key 和组件 key 更新到 skill。
4. 如果要把依赖随 git 分发，只能分发 manifest、组件 key map、preflight 脚本、用法文档和少量自有 fallback 资产；不能分发 Figma 云端 library 的真实身份。
5. fallback 资产只用于明确标注的低保真兜底或缺口说明，不得在正式 Arco 原型交付中替代 required library。

核心目标：

1. 先复用 Arco New 组件，再做业务布局。
2. 先理解组件属性语义，再修改 variant。
3. 用组件传递明确的信息层级、操作优先级、反馈强度和状态语义。
4. 保持原型高度组件化：关键控件、表格、反馈、导航、表单、浮层、状态标识都应来自 Arco assets。
5. 图表必须使用已订阅的图表库组件表达趋势、占比、排行和维度分布，不手绘折线、柱、饼、图例、坐标轴或 tooltip。
6. 不把业务状态硬塞进交互态、结构层或图标层。

## 1. Figma 资产边界

### 1.1 资产来源

- 目标 library：`Arco Design System (New)`。
- Figma library key：`lk-1fdff400a519972aa8ca99185d4b6a6105f88f62ea6ba8ee2ef0938635f787d47223212bf8899cf7d82847743cca58a67cbae1d9ebc9f7ba26f35df5c323e3b9`。
- 搜索组件时必须用 `search_design_system`，并优先限定 `includeLibraryKeys` 到上述 library key。
- 搜索结果可能混入其他系统 library；只有 `libraryName = "Arco Design System (New)"` 的结果可作为本技能依据。
- 数据可视化补充 library：`源力设计体系-图表库`。
- 图表库 library key：`lk-a3da2a4c46bf632b0087bf59bbf692c935855e5e43ccdc92b9bc4d9211e6480d468df1801feddcc717033d4eb400c04580f8d7c6f817fd9db44f597ec0727931`。
- 需要折线、面积、柱状、Toplist、饼图、环形图时，必须优先搜索并导入 `源力设计体系-图表库` 里的组件，而不是在 Arco 卡片里手绘图形。
- 图表库是 Arco 中后台页面的可视化补充资产：页面框架、表单、表格、按钮、状态、浮层继续使用 Arco；数据图形本体使用图表库。

### 1.2 变量与样式事实

- 当前文件可见的 `Arco Design System (New)` library variable collection 为 `Collection 1`，但该 collection 返回变量数量为 0。
- 与 Arco 命名相近的 token collection 例如 `IT-arco-token`、`IT-arco-style` 属于其他 library，不等同于 `Arco Design System (New)`。只有在任务明确允许使用这些 style/token library 时，才可引用。
- 不要臆造 Figma variables。若要绑定变量，必须先通过 `getAvailableLibraryVariableCollectionsAsync`、`getVariablesInLibraryCollectionAsync`、`search_design_system` 或现有节点绑定信息复核。
- 如果原型只需要视觉准确，可使用组件自带 styles；如果任务要求 token 化，必须先确认变量来源、变量名、mode 和 scope。

### 1.3 常见视觉语义

这些色彩语义可用于理解组件传达的信息，但写入原型时优先依赖组件实例和 style，而不是硬编码：

| 语义 | 常见命名 | 常见值 | 传递信息 |
|---|---|---:|---|
| 主品牌/主要行动 | `primary-6` | `#165DFF` | 主行动、当前选中、可点击焦点 |
| 正向/成功 | `success-6` | `#00B42A` | 成功、已完成、健康、允许 |
| 警告/提醒 | `warning-6` | `#FF7D00` | 风险、待关注、需要确认 |
| 错误/危险 | `danger-6` | `#F53F3F` | 错误、失败、破坏性操作 |
| 主文本 | `color-text-1` | `#1D2129` | 标题、主要内容 |
| 次文本 | `color-text-2` | `#4E5969` | 描述、辅助内容 |
| 弱文本 | `color-text-3` | `#86909C` | placeholder、低优先级信息 |
| 边界 | `color-border-2` | `#E5E6EB` | 分割、容器边界 |
| 填充 | `color-fill-2` | `#F2F3F5` | 输入底色、轻背景 |
| 页面底色 | `color-bg-1` | `#F7F8FA` | 中后台页面背景 |
| 白底 | `White` | `#FFFFFF` | 卡片、弹层、表格容器 |

## 2. 属性语义总则

### 2.1 Component property 类型

| 类型 | 在 Figma 中的作用 | 使用规则 |
|---|---|---|
| `VARIANT` | 决定组件形态、语义、尺寸、交互态、数量、布局 | 只能表达组件自身设计轴，不要拿来表达业务字段 |
| `BOOLEAN` | 显隐某个子结构，如图标、标题、描述、按钮、辅助文本 | 只控制结构是否出现，不表达业务真假 |
| `TEXT` | 文本槽位，如按钮文案、placeholder、标题、状态文字 | 改文本前确认写入的是内容层，不是 icon、dot、marker |
| `INSTANCE_SWAP` | 替换图标或 slot 子组件 | 用同类图标替换；不要把复杂业务组件塞进 icon slot |

### 2.2 通用 variant 轴

| 轴 | 常见值 | 含义 | 设计信息 |
|---|---|---|---|
| `尺寸` | 大 / 中 / 小 / 迷你 | 控件密度与层级 | 大更显著，中为默认，小/迷你适合密集区域 |
| `状态` | 默认 / 悬停 / 聚焦 / 激活 / 禁用 | 交互态 | 表达用户操作过程，不表达业务状态 |
| `类型` | 主要、次要、文本、信息、成功、警告、错误等 | 组件类别或语义 | 表达优先级或反馈性质 |
| `种类` | 标准 / 危险 / 警告 / 成功 | 动作风险或结果倾向 | 危险=破坏性，警告=需谨慎，成功=正向 |
| `形状` | 长方形 / 全圆角 / 方形 / 圆形 | 轮廓和图标化程度 | 方形/圆形通常用于 icon-only |
| `布局` | 水平 / 垂直 | 信息排列方向 | 水平适合短集合，垂直适合长内容或窄容器 |
| `数量` | 2-N | 子项数量 | 仅表达组件结构数量，不表达数据总量 |
| `填充` | true / false | 输入或标签背景形态 | true 更轻、更弱；false 边框更明确 |
| `禁用` | true / false | 是否不可操作 | 禁用表示不可交互，不能表示未完成、失败或无权限原因 |
| `悬停/聚焦/激活` | true / false | 分离式交互态 | 用于演示状态，不作为默认静态页面主状态 |
| `选中/开启/多选/范围` | true / false | 选择或控件模式 | 代表用户选择结构，不代表业务结果 |

### 2.3 信息强度

从弱到强的信息表达应按以下顺序选择：

1. 普通文本：解释、辅助说明、低风险内容。
2. `Tag`：类别、短状态、属性标记、可移除筛选项。
3. `badge/status`：流程或列表中的状态语义，适合密集读取。
4. `Alert`：页面内持续可见的提示、规则、风险、摘要。
5. `Message`：短暂操作反馈。
6. `modal-confirm`：需要用户明确确认的风险或不可逆动作。
7. `Modal` / `Drawer`：承载完整任务或复杂内容的浮层。

### 2.4 交互态和业务态分离

- `状态=禁用` 只表示控件不可操作，不表示数据失败、草稿、跳过、未开始。
- `状态=聚焦/悬停/激活` 只用于表达用户当前交互，不作为业务生命周期。
- 业务态应通过 `badge/status`、`Tag`、`Alert`、表格 status cell、结果面板或文案表达。
- 不要通过改变按钮类型、输入框状态或 tab 交互态来暗示业务态。

## 3. 组件选择总览

| 信息/任务类型 | 首选组件 | 传递的信息 |
|---|---|---|
| 主要动作 | `Button` 的主要按钮 | 当前页面最重要、可立即执行 |
| 次要动作 | `Button` 的次要/线框/文本按钮 | 辅助、取消、返回、低优先级 |
| 图标动作 | `interactive-button/*` + `Tooltip` | 紧凑工具动作，需要解释 |
| 文本输入 | `input` / `textarea` / `password` / `InputNumber` | 用户录入数据 |
| 搜索 | `search-box` | 筛选或检索 |
| 枚举选择 | `select` / `radio-group` / `radio-button-group` | 从有限集合选择 |
| 多选 | `checkbox` / `checkbox-group` / `select` 多选 | 可叠加选择 |
| 二元即时控制 | `switch` | 开/关立即生效 |
| 时间选择 | `datepicker` / `timepicker` | 日期、时间、范围 |
| 层级选择 | `cascader` / `tree` / `treeselect` | 父子层级或目录 |
| 大量对象迁移 | `transfer` / `transfer-tree` | 候选和已选集合移动 |
| 数据列表 | `table` + table-cell 系列 + `pagination` | 可扫描、可排序、可操作 |
| 时序趋势 | 图表库 `折线图` / `折线组合/dataLinesCombo` | 指标随时间变化、峰谷、趋势方向 |
| 趋势量感 | 图表库 `AreaChart 面积图` | 趋势背后的规模、累计感、吞吐量变化 |
| 分类/周期对比 | 图表库 `BarChart 柱状图` | 离散类别、时间段、系列之间的数值比较 |
| TopN 排名/维度分布 | 图表库 `Toplist 条形图` | 对象排行、维度分布、高优对象定位 |
| 少量构成占比 | 图表库 `Component/PieChart` / `Mini_pie_chart` | 2-5 类构成、紧凑占比提示 |
| 完整占比卡片 | 图表库 `Card/PieChart` | 带标题、图例、统计信息的占比模块 |
| 短标签 | `Tag` | 分类、属性、短状态 |
| 状态点/状态文字 | `badge/status` | 状态语义 |
| 页面内提示 | `Alert` | 持续说明或风险提示 |
| 短暂反馈 | `Message` | 操作结果提示 |
| 风险确认 | `modal-confirm` | 阻断式确认 |
| 短任务浮层 | `Modal` | 局部任务、少量信息 |
| 长任务浮层 | `drawer` | 保留上下文的复杂编辑或详情 |
| 导航切换 | `tabs/*` / `menu` / `Anchor` / `steps` | 同级视图、层级导航、锚点、流程 |
| 表单结构 | `form` / `form-item` | 字段、必填、说明、校验 |
| 空结果 | `empty` | 无数据、无匹配、无权限等空态 |

## 4. 组件资产详解

### 4.1 Button

资产：`Button`，component set，key `3990ea6d08b3b4b9e95ae360ed73eadf9493a0b3`。

已验证属性：

| 轴/属性 | 值 |
|---|---|
| `类型` | 主要按钮 / 次要按钮 / 虚框按钮 / 线框按钮 / 文本按钮 |
| `种类` | 标准 / 危险 / 警告 / 成功 |
| `形状` | 长方形 / 全圆角 / 方形 / 圆形 |
| `尺寸` | 大 / 中 / 小 / 迷你 |
| `状态` | 默认 / 悬停 / 聚焦 / 激活 / 禁用 |
| `显示图标` | true / false |
| `替换图标`、`更换图标` | instance swap |

抽象规则：

- `主要按钮` 只用于最高优先级提交或创建动作；一个操作区通常只保留一个。
- `次要按钮` 用于取消、返回、辅助动作。
- `文本按钮` 用于弱动作、表格行内动作、链接式动作。
- `虚框按钮` 用于轻量新增、上传入口、空态引导等低压操作。
- `线框按钮` 适合中等优先级操作，不应压过主按钮。
- `种类=危险` 必须用于删除、撤销、清空、覆盖等破坏性动作，并配合确认反馈。
- `种类=警告` 用于高风险但可恢复动作。
- `种类=成功` 用于明确正向动作，应少用，避免把页面做成多色按钮集合。
- 方形/圆形形状多用于纯图标按钮；必须有 Tooltip 或可访问名称。

用户感知：

- 主按钮告诉用户“这是下一步”。
- 危险按钮告诉用户“这会造成损失或不可逆后果”。
- 禁用按钮告诉用户“此刻不能操作”，但不会解释原因；原因需要旁侧文字或 Tooltip。

### 4.2 Button Group

资产：`Button-group`，component set，key `35728118b20e3595bb65e4219b8cbf52ad5ab053`。

已知属性：

| 轴 | 值 |
|---|---|
| 数字结构轴 | 2 / 3 / 4 / 5 / 6 / 7 / 8 |

抽象规则：

- 用于一组紧密相关且可并列展示的按钮。
- 如果是互斥视图切换，优先考虑 `radio-button-group` 或 `tabs/rounded`。
- 如果是页面级导航，不使用 Button Group，改用 Tabs 或 Menu。

### 4.3 Link

资产：`Link`，key `60253fc5ea2d82720305959a58e8629863575398`。

已验证属性：

| 轴/属性 | 值 |
|---|---|
| `类型` | 默认 / 错误 / 警告 / 成功 |
| `状态` | 默认 / 悬停 / 激活 / 禁用 |
| `显示图标` | true / false |
| `显示下拉` | true / false |
| `替换文本` | text |
| `替换图标` | instance swap |

抽象规则：

- 用于文本中的跳转、表格行内轻动作、详情入口。
- `类型=错误` 只用于危险或负向动作，不用于普通强调。
- 多个 Link 并列时要控制数量；超过 3 个操作应收纳到 dropdown。

### 4.4 Interactive Button

资产：`interactive-button/*`，多数为单组件 icon action。

已检索资产包括：

| 组件 | key | 含义 |
|---|---|---|
| `interactive-button/search` | `b3bd8f8f2928e8a283d4678903bbdac77ec68356` | 搜索 |
| `interactive-button/settings` | `960ee35bed2d81c9dc62a3ac6b4978ebb8e91b7f` | 设置 |
| `interactive-button/refresh` | `8963492605d46cc498c9227a2948096522cc4d66` | 刷新 |
| `interactive-button/history` | `d57a5ac05df76504951889d1abf128552d76916f` | 历史 |
| `interactive-button/save` | `2fe5a24d4d9c5b318f0f8f1d8d3da4d668fefca4` | 保存 |
| `interactive-button/download` | `e1708c64228e98bfd3140ff970b84592939c7055` | 下载 |
| `interactive-button/upload` | `bf89a1a664517abaf082e299edf5c52f43bccf04` | 上传 |
| `interactive-button/more` | `1271f972facbac860b578f9b291ad130d9876fb4` | 更多 |
| `interactive-button/more-vertical` | `4e7d11360a969f254fe1c60ff1ebab1fc70ecb0f` | 纵向更多 |
| `interactive-button/eye` | `9cbde0ff6805e26997ca700cc320dfb7884c81df` | 查看/预览 |
| `interactive-button/share-internal` | `f04d7319bf353c3df43ad41aa97d9c027f531be7` | 分享 |
| `interactive-button/star` | `e1413878be684a3bb805d6101c64bb9d9b84e09f` | 收藏 |
| `interactive-button/send` | `4acaea4bcd4058ec6b0781f44fb2d1a33024cafd` | 发送 |
| `interactive-button/home` | `35de7c5215c70787995294bf4fe465a77ef7908d` | 首页 |
| `interactive-button/at` | `02d8a0a7dfd90467a170a487ba20ec943e7a4fc3` | 提及 |
| `interactive-button/sync` | `610be4e432cfffd915011dfdc9b2c7eb04dc58ef` | 同步 |
| `interactive-button/poweroff` | `cdd57ffaa894334ad3f0f578e9e50086ff43bd5c` | 开关/停用 |
| `interactive-button/select-all` | `30be3af434d18a7b4190035e475a821bcda77ef6` | 全选 |

抽象规则：

- 图标按钮必须有 Tooltip，除非上下文中文本已经完整解释动作。
- 图标只表达动作类别，不承载业务状态。
- 相邻 icon action 的数量应少；复杂动作集用 dropdown-menu。

### 4.5 Input

资产：`input`，component set，key `5c2532a1a6dbe38cf6d5e80871d7b18115770049`。

已验证属性：

| 轴/属性 | 值 |
|---|---|
| `尺寸` | 大 / 中 / 小 / 迷你 |
| `状态` | 默认 / 悬停 / 聚焦 / 禁用 |
| `填充` | false / true |
| `前缀`、`后缀` | true / false |
| `前置标签`、`后置标签` | true / false |
| `替换文本` | text |
| `前缀图标`、`后缀图标` | instance swap |

抽象规则：

- 单行短文本录入用 Input。
- 搜索不要用 Input 手拼图标，优先 `search-box`。
- 带单位、协议、固定前后缀时使用前置/后置标签。
- 禁用表示不可编辑；只读信息优先用文本或 Descriptions，不要伪装成禁用输入框。

### 4.6 InputNumber

资产：`InputNumber`，component set，key `418031d9a74634ab9066ee71fa83514ab9b620f5`。

已知属性：

| 轴/属性 | 值 |
|---|---|
| `尺寸` | 大 / 中 / 小 / 迷你 |
| `状态` | 默认 / 悬停 / 聚焦 / 禁用 |
| `按钮模式` | false / true |

抽象规则：

- 只用于数字录入；范围连续调节可与 Slider 配合。
- 有单位时用后缀或旁侧单位文本。
- 若精度重要，InputNumber 优先于 Slider。

### 4.7 Textarea

资产：`textarea`，component set，key `ccc01adbf138f1be8452755411eb5d17fa9c74ae`。

已验证属性：

| 轴/属性 | 值 |
|---|---|
| `状态` | 默认 / 悬停 / 聚焦 / 禁用 |
| `填充` | false / true |
| `字数统计` | true / false |
| `替换文本` | text |

抽象规则：

- 用于长文本、多行说明、脚本、表达式、原因、备注。
- 字数限制明确时开启 `字数统计`。
- 复杂代码编辑器如果无专用组件，Textarea 只作为低保真承载，不应伪装完整 IDE。

### 4.8 Search Box

资产：`search-box`，component set，key `fe97f48d8c31769cfce27c8c65331418ffeeb907`。

已验证属性：

| 轴/属性 | 值 |
|---|---|
| `尺寸` | 大 / 中 / 小 / 迷你 |
| `类型` | 默认 / 文字按钮 / 图标按钮 |
| `填充` | false / true |
| `替换文本` | text |

抽象规则：

- 用于关键词检索、列表筛选、轻量搜索。
- `默认` 适合实时或回车搜索。
- `文字按钮` 适合需要明显提交动作。
- `图标按钮` 适合紧凑工具栏。

### 4.9 Password / Mentions / InputTag / Input Group

已检索资产：

| 组件 | key | 语义 |
|---|---|---|
| `password` | `320b7bcfd4476502cbd7bb20a442480eb1157886` | 密码或敏感文本 |
| `Mentions` | `1465d5e5153ed7a54457a81e27ae403af6437e43` | 提及用户、对象或实体 |
| `inputtag` | `57d0a2333f7f490cfb537e0173f2548817698bf3` | 标签化输入 |
| `input-group` | `9be7c05fc03cd4d8eb6b21022a48ef46cdd9c3c5` | 多段组合输入 |

抽象规则：

- 敏感信息用 Password，不用普通 Input 加图标手绘。
- 可重复离散值用 InputTag，不用逗号文本模拟。
- 多字段短组合用 Input Group；复杂结构改用 Form。

### 4.10 Select / Selection

资产：`select`，component set，key `48a47bfdde25bafdfb53ecc75aa3aa296c52777d`。

已验证属性：

| 轴 | 值 |
|---|---|
| `尺寸` | 大 / 中 / 小 / 迷你 |
| `填充` | false / true |
| `多选` | false / true |
| `悬停` | false / true |
| `聚焦` | false / true |
| `禁用` | false / true |

相关资产：

| 组件 | key | 用途 |
|---|---|---|
| `selection-item` | `f14ea80e55089bb5af4d5f9a8772012c8752620d` | 下拉选项 |
| `treeselect` | `1eb25fc05e0d6d70012928c2fe74a2d5209fcb25` | 树选择 |
| `treeselect-dowmenun` | `6c164260e441b5896208e830de8815fcf51de3c0` | 树选择弹层 |

抽象规则：

- 枚举项较多时用 Select。
- 选项很少且需要直接比较时用 Radio，而不是 Select。
- 需要多选时用 `多选=true` 或 Checkbox Group，根据是否需要折叠来决定。
- 层级复杂时用 TreeSelect，不用 Select 模拟缩进层级。

### 4.11 Cascader / Tree / TreeSelect

已检索资产：

| 组件 | key | 语义 |
|---|---|---|
| `cascader` | `87bfe6358e79d00cb0cce3b98ed452dc6a8cde32` | 级联选择 |
| `cascader/popup-inner` | `ea39a5c4abba6f1ed2897865b3f92b170d023a9a` | 级联弹层 |
| `tree` | `50d0377e5c824d6f7aa0d1e339b042e1011dc596` | 树 |
| `treeselect` | `1eb25fc05e0d6d70012928c2fe74a2d5209fcb25` | 树选择 |
| `treenode/1st-level` | `adbe1612e26bde4eb007effe05e7eb55276c09a7` 等 | 树节点 |
| `treenode/final-level` | `02a195f65147096348877fd2a34251f16c940e3a` / `1e01f0016599e03bf4c3ff4bd77418e88c79ff89` | 末级节点 |
| `components/tree-switcher` | `f8158fd0ae29425249f8820b054d6569868a7b87` | 展开/收起 |

抽象规则：

- Cascader 表达明确路径选择，适合层级浅且路径重要。
- Tree 表达整体层级浏览，适合展开、折叠、批量选择。
- TreeSelect 表达在输入框里选择层级对象，适合表单或筛选。
- 不要用普通 Select 加缩进文本假装树；用户会误解可搜索、展开和父子选择关系。

### 4.12 Datepicker / Timepicker

已检索资产：

| 组件 | key | 语义 |
|---|---|---|
| `datepicker` | `a595056a6370b1183373ce445adcbcc8720a5d04` | 日期选择 |
| `datepicker-dropdown` | `ac24ac1e1e01f442c02c27bc21156fa90114a0ed` | 日期弹层 |
| `timepicker` | `0d22f94950bee89222bc8d905b23142cd7fc33d6` | 时间选择 |
| `timepicker-dropdown` | `b895fae0da93d92432886dba2807eae26748c9a1` | 时间弹层 |
| `general/calendar` | `1874ce511dcf725febe3a2661f1ae32186a82847` | 日历图标/子结构 |
| `components/date-cell` | `3a06e51c0bb73a219222782424077471226b7e77` | 日期单元 |
| `picker-cell/year` | `48ae0ebf1cbd74b24fff172580baecb85f974f26` | 年份单元 |
| `picker-cell/month` | `2488b006afe5a83a13b65a962c5e1d4a20bf7a3c` | 月份单元 |
| `components/time-item` | `0426be9b5fe478edb60183b97e1485b7ea268584` | 时间项 |

已知 Datepicker/Timepicker 属性：

| 轴 | 值 |
|---|---|
| `尺寸` | 中 / 大 / 小 / 迷你 |
| `填充` | false / true |
| `范围` | false / true |
| `悬停` | false / true |
| `禁用` | false / true |

抽象规则：

- 选择单个日期/时间用单值模式。
- 选择起止区间用 `范围=true`，不要并排拼两个 input。
- Calendar、date-cell、time-item 是弹层结构件；除非要展示完整日期选择面板，否则优先用父组件。

### 4.13 Radio

已检索资产：

| 组件 | key | 语义 |
|---|---|---|
| `radio-group` | `2aaaf048ac288fb3f839445e5864fe72048b48f6` | 单选组 |
| `radio-button-group` | `bf1a6d2c9b01b98b5e40f2a042e3ac54adddd426` | 按钮式单选 |
| `components/radio` | `dae5f8e7ab032d96a848b28cbcdd026d6a4524e7` | 单选项 |
| `components/radio-button` | `26c0d573b2ae2356199af205d417d7001b7d4e79` | 按钮式单选项 |

已知属性：

| 组件 | 轴 |
|---|---|
| `radio-group` | `布局=水平/垂直`，数量轴表示选项数量或选中位置 |
| `radio-button-group` | `尺寸=大/中/小/迷你`，`数量=2/3/4/5`，`↳ 选中=1/2/3/4/5` |

抽象规则：

- 用于少量互斥选项。
- Radio Group 更适合表单语义；Radio Button Group 更适合强视觉切换。
- 不要用 Select 替代 2-4 个清晰互斥选项。

### 4.14 Checkbox

已检索资产：

| 组件 | key | 语义 |
|---|---|---|
| `checkbox` | `d67cdf7b859956574fb45ca06aa53789b6925a24` | 单个复选框 |
| `checkbox-group` | `9846385aac6eb45423ca2c5c865471b04faa4cda` | 复选框组 |
| `components/table-cell/checkbox` | `45154d9ee8fab22b85af6d7fafb41775ffe087e5` | 表格行选择 |
| `components/table-column/check-box` | `26adaff9bf72b2b814ced7b6c495195ab070dbca` | 表头选择列 |

已验证属性：

| 轴/属性 | 值 |
|---|---|
| `选中` | true / false |
| `半选` | true / false |
| `禁用` | true / false |
| `悬停` | true / false |
| `显示标签` | true / false |
| `替换文本` | text |

抽象规则：

- 用于多选、附加设置、确认勾选。
- `半选=true` 用于父级部分选择，不表示不确定业务状态。
- 表格批量选择必须使用 table-cell/table-column 相关资产。

### 4.15 Switch

资产：`switch`，component set，key `559eb7d4377ec0e7a5cef8cd6cd5e1862407ca2f`。

已验证属性：

| 轴 | 值 |
|---|---|
| `类型` | 圆形 / 矩形 / 线性 |
| `尺寸` | 默认 / 小 |
| `开启` | true / false |
| `禁用` | false / true |
| `滑块图标` | false / true |
| `滑轨内容` | 无 / 图标 / 文字 |

相关资产：

| 组件 | key |
|---|---|
| `switch-dot/default` | `1d1a2cfbc6107a16a4212a2d946bb8170c8fb5b1` |
| `switch-dot/icon` | `acac713be12bb33fc870a47c6d81c01c6f8a4194` |
| `components/table-cell/switch` | `fb83e27bfc9dbcc77a9b1f5e28f1629793b93798` |
| `components/table-column/switch` | `3d16e1d293ce5020d1703b1ad0296b296e9e4251` |

抽象规则：

- 只用于立即生效的二元开关。
- 需要提交保存后才生效的设置，应使用 Checkbox、Radio、Select 或表单字段。
- 不要 resize Switch；使用 `尺寸` variant。
- 文案放在旁侧，不把 switch 拉成长胶囊承载句子。

### 4.16 Slider

已检索资产：

| 组件 | key | 语义 |
|---|---|---|
| `slider` | `136eea5546ec7dc2fc34a10b27930382bbd3faed` | 滑动输入 |
| `slider/default` | `3565742c29df6d6edaf22d51bd9815be9af98c6d` | 基础滑块 |
| `components/slider-mark` | `7bcbb188204c2e5c45c4a682beee89628f738430` | 刻度 |
| `components/marks` | `2de5dd992f628dccd49adffa7546c5d7f71924de` | 标记 |

抽象规则：

- 用于连续值、范围感、权重、比例、阈值调节。
- 精确录入优先 InputNumber。
- mark 只放刻度或短标签，不放解释段落。

### 4.17 Upload

已检索资产：

| 组件 | key | 语义 |
|---|---|---|
| `upload` | `1a2d6415793f0ef880f9637aaa633b2cd9ad5311` | 上传容器 |
| `upload-trigger` | `98a1fc9a96d7c353bb059dcd075c5a1dc71f2a7f` | 触发器 |
| `upload/list` | `2f790c06d2940bb2f8ee1b6618978bbbff2dfc9d` | 文件列表 |
| `upload/file-list-item` | `844149c19565d47a208fd1729fd02115a7747bd5` | 文件项 |
| `upload/picture-card` | `19f38038691630c55427f374a3198962df1d2a52` | 图片卡片 |
| `upload/picture-list-item` | `b5e8ad924e2825d1632ea13371c5be6958e4fb3d` | 图片列表项 |
| `interactive-button/upload` | `bf89a1a664517abaf082e299edf5c52f43bccf04` | 上传图标按钮 |

抽象规则：

- 文件导入用 trigger + list。
- 图片或媒体用 picture-card/list。
- 上传状态要由文件列表项表达，不要只放一个按钮。

### 4.18 Transfer

已检索资产：

| 组件 | key |
|---|---|
| `transfer` | `2f5a4c1434619e5572bee4f7ad09a3f945cc942b` |
| `transfer-tree` | `4ddd195d8a31be46469d8aa9ab6c3d4cf1bd139d` |

抽象规则：

- 用于从候选集合移动到已选集合。
- 层级对象选择用 `transfer-tree`。
- 小量多选不要用 Transfer，Checkbox Group 或 Select 多选更轻。

### 4.19 Table

资产：`table`，component set，key `c29c7405d6688b0e00fe66ab9ae807a1a10932ed`。

已知属性：

| 轴 | 值 |
|---|---|
| `尺寸` | 大 / 中 / 小 / 迷你 |
| `边框` | 依资产定义选择 |
| `分页` | true / false |
| `列数量` | 通过 table column/cell 结构控制 |

表格资产：

| 组件 | key | 用途 |
|---|---|---|
| `components/table-cell/header` | `1a8b7981cf2ff552070f951b860ffdfb8979f06c` | 表头 |
| `components/table-cell/text` | `beea3441917266bb6aa76ac1034373e26f570bb3` | 文本单元格 |
| `components/table-cell/link` | `c2868d542f055699d0e0a9c6030477452532a96b` | 链接单元格 |
| `components/table-cell/status` | `442f36863be74518a16ced2e552e7c85259a61b5` | 状态单元格 |
| `components/table-cell/tag` | `b2cc464efd6ff610f49aedf4122e2bbae71a301c` | 标签单元格 |
| `components/table-cell/action` | `0a186cc80424b1fea3c52c71b00427fb9fb01204` | 操作单元格 |
| `components/table-cell/avatar+text` | `b0dd201b440f622d2ade9e8fb614a11ef1f51ad5` | 头像 + 文本 |
| `components/table-cell/switch` | `fb83e27bfc9dbcc77a9b1f5e28f1629793b93798` | 行内开关 |
| `components/table-cell/sort` | `501dff5a9cd03bd9e8f0459415863e8a298cbdc8` | 排序 |
| `components/table-cell/checkbox` | `45154d9ee8fab22b85af6d7fafb41775ffe087e5` | 行选择 |
| `components/table-cell/expand icon` | `6357649a4768dbed3eedcfc9002d6ebdb059cbcd` | 展开 |
| `components/table-column/text` | `0cbfa22dd4b007631f789a5ed109e2b1dc7e965e` | 文本列 |
| `components/table-column/tags` | `6d3d46073ce8dbe006b938836f478aca7d2e0f34` | 标签列 |
| `components/table-column/actions` | `3fdbdbe2aea07ce9764a6cedba5277f2830028e1` | 操作列 |
| `components/table-column/check-box` | `26adaff9bf72b2b814ced7b6c495195ab070dbca` | 选择列 |
| `components/table-column/switch` | `3d16e1d293ce5020d1703b1ad0296b296e9e4251` | 开关列 |
| `components/table-column/sort` | `fc743d6cbe661600710ee39a9ddd3a9ded5c205e` | 排序列 |
| `components/table-column/expand icon` | `2f1b93b8f82cee00f9b2c45d126c88375321a092` | 展开列 |
| `components/table-column/text+icon` | `eece65d2e4571992661a04693cab81aa50991781` | 文本 + 图标列 |

已验证 table-cell 关键属性：

| 组件 | 属性 |
|---|---|
| `table-cell/action` | `尺寸=大/中/小/迷你`，`数量=1/2/3` |
| `table-cell/header` | `尺寸=大/中/小/迷你`，`排序=true/false`，`筛选=true/false`，`搜索=true/false`，对应激活态 |
| `table-cell/link` | `尺寸=大/中/小/迷你`，`替换文本` |
| `table-cell/text` | `尺寸=大/中/小/迷你`，`替换文本` |
| `table-cell/status` | `尺寸=大/中/小/迷你` |
| `table-cell/tag` | `尺寸`，`数量` |

抽象规则：

- 表格用于高密度、多列、可比较数据。
- 每行必须有统一 row frame 和统一 row height。
- 同一行内所有 cell 垂直居中；状态、Tag、Switch、Action 不可撑高行。
- 不要手绘表头、分割线、状态点、行内按钮。
- 行内动作 1-3 个直接展示；更多动作放入 dropdown。
- 可排序/筛选/搜索的列必须使用 header 对应 variant，而不是手绘图标。

### 4.20 Pagination

已检索资产：

| 组件 | key | 语义 |
|---|---|---|
| `pagination` | `cf5aadeb7e5da90c76c7908f1b8411f917590c64` | 标准分页 |
| `pagination-simple` | `eb5fda4627f97de22abbb964dacc74f010fc0f69` | 简洁分页 |
| `components/pagination-prev` | `aee632f376ef584ce175e6fa50673e8d33d38796` | 上一页 |
| `components/pagination-next` | `78ab4e2762af33b0bb41202bdf16386a364b787b` | 下一页 |
| `components/pagination-item` | `f5af4ba76f1a2b1f28942e3a543411c51d48aadb` | 页码项 |

已验证属性：

| 组件 | 轴/属性 |
|---|---|
| `pagination` | `尺寸=大/中/小/迷你`，`禁用=true/false`，`跳转=true/false`，`展示条目=true/false`，`展示总数=true/false` |
| `pagination-item` | `尺寸=大/中/小/迷你`，`选中=true/false`，`悬停=true/false`，`禁用=true/false` |
| `pagination-prev/next` | `尺寸=大/中/小/迷你`，`悬停=true/false`，`禁用=true/false` |

抽象规则：

- 标准分页用于完整列表；简洁分页用于小浮层或空间紧张区域。
- 分页属于数据区域 footer，不属于表格最后一行。
- 当前页必须通过选中态表达。

### 4.21 Tag

资产：`Tag`，component set，key `6f0545784a46e460f1543b22ee4977d5b5fa8dea`。

已验证属性：

| 轴/属性 | 值 |
|---|---|
| `尺寸` | 大 / 中 / 小 / 迷你 |
| `颜色` | 默认 / 浪漫红 / 仙野绿 / 碧涛青 / 活力橙 / 晚秋红 / 黄昏 / 青春紫 / 品红 / 极致蓝 / 海蔚蓝 |
| `填充` | true / false |
| `边框` | false / true |
| `显示图标` | true / false |
| `可关闭` | true / false |
| `替换文本` | text |
| `替换图标` | instance swap |

相关资产：

| 组件 | key |
|---|---|
| `tag-deletion` | `56dfc6eeba84f3f35fb2d77c2e09f3f49064cf18` |
| `general/tag` | `0bb5f1e2d5646d89350995109d28a82378c6a41b` |
| `general/tags` | `3dd9328b48cada15d7a75242815438ff1db690d2` |

抽象规则：

- Tag 表示短标签、分类、属性、局部状态。
- 可关闭筛选条件用 `可关闭=true`。
- 非强流程状态可用 Tag；流程状态和表格状态优先 badge/status。
- Tag 文本应短，宽度随内容收缩，不要拉伸成按钮。

### 4.22 Badge

已检索资产：

| 组件 | key | 语义 |
|---|---|---|
| `badge/status` | `7e944999d56d064725832e3ddee2f49da24ba1ff` | 状态文字 |
| `badge/dot` | `7cfe721bbaad7cb6457188ac44c4629e818ec476` | 状态点 |
| `badge/count` | `bc950625ebc59a00312d6c46edcceb4845894bbe` | 数量角标 |
| `badge-icon` | `842f8459190869edb786f8c18525e667854151ab` | 图标角标 |
| `badge-text` | `a423f6d039697b8a181c582337e1ca7de31bda5f` | 文本角标 |
| `badge-avatar` | `4671b311d917a50fc231bc1818ead8907a067fe2` | 头像角标 |

已验证属性：

| 组件 | 轴/属性 | 值 |
|---|---|---|
| `badge/status` | `类型` | 默认 / 错误 / 进行中 / 成功 / 提醒 |
| `badge/status` | `替换文本` | text |
| `badge/dot` | `颜色` | red / orangered / orange / gold / lime / green / cyan / arcoblue / purple / pinkpurple / magenta / gray |

抽象规则：

- `status` 表达有语义的状态。
- `dot` 是极轻提示；若需要文字解释，使用 `status`。
- `count` 表达未读、待处理、数量提醒。
- 状态语义建议：默认=中性/未开始，进行中=处理中，成功=完成/健康，提醒=待关注，错误=失败/异常/危险。

### 4.23 Alert / Message

资产：

| 组件 | key | 语义 |
|---|---|---|
| `Alert` | `32a44b8ea5466db43bb9e9137bc37e0eab742067` | 页面内反馈 |
| `Message` | `8c6cc521a238d25a40fd85ff6683f378868e08cb` | 全局短提示 |

Alert 已验证属性：

| 轴/属性 | 值 |
|---|---|
| `类型` | 信息 / 成功 / 警告 / 错误 |
| `标题` | true / false |
| `图标` | true / false |
| `操作按钮` | true / false |
| `可关闭` | true / false |

抽象规则：

- Alert 用于页面内持续可见信息。
- Message 用于操作后的短暂反馈，不承载复杂内容。
- 信息=说明，成功=正向结果，警告=风险但未失败，错误=失败或阻塞。
- 有行动建议时可开启操作按钮；可关闭只在提示不是必读时使用。

### 4.24 Modal Confirm / Modal / Drawer / Tooltip

资产：

| 组件 | key | 语义 |
|---|---|---|
| `modal-confirm` | `764ca7820829428d596e1768f0db8243a7a9bb61` | 确认对话框 |
| `modal` | `9c33cc38495d259645d948f55d6c93b25fc16eb5` | 对话框 |
| `drawer` | `b17398ef2a7e4fe8fa6c2d966f0193f3a2d2330e` | 抽屉 |
| `Tooltip` | `18f27123620809941a9e16e691751306d642affd` | 文字气泡 |

modal-confirm 已验证属性：

| 轴 | 值 |
|---|---|
| `信息` | false / true |
| `成功` | false / true |
| `警告` | false / true |
| `危险` | false / true |

抽象规则：

- `modal-confirm` 用于阻断式二次确认；危险、不可逆、批量影响必须使用。
- `Modal` 用于短流程、少量字段、局部预览。
- `Drawer` 用于长表单、详情、编辑流程，保留原页面上下文。
- `Tooltip` 只承载短说明；长解释用 Popover、Alert、Drawer 或说明区。
- 有 footer 的弹层必须有清晰 header；按钮放 footer 右侧，次要在左、主要在右。

### 4.25 Tabs

已检索资产：

| 组件 | key | 语义 |
|---|---|---|
| `tabs/line` | `1b8c79f7bd5976e857ff4f83187272eb1c636b25` | 基础线性 Tabs |
| `tabs/rounded` | `91fc6ff61fe667abb1509d1c70d29b3d6367c831` | 圆润 Tabs |
| `tabs/card` | `06df27dea60cee1ff38c9767ed1e9b7e45cb3ae4` | 卡片 Tabs |
| `tabs/card-gutter` | `b0825d0e3b968918e7be715017dee50bc7973103` | 卡槽 Tabs |
| `tabs/text` | `61acbbaa1adae8a78489147c20e3ef0c1ec93e89` | 文本 Tabs |

已验证 `tabs/line` 属性：

| 轴 | 值 |
|---|---|
| `布局` | 水平 / 垂直 |
| `图标` | false / true |
| `附加` | false / true |
| `滚动` | false / true |

子组件 `_components/tab` 已验证属性：

| 轴/属性 | 值 |
|---|---|
| `尺寸` | 大 / 中 / 小 / 迷你 |
| `选中` | true / false |
| `悬停` | false / true |
| `禁用` | false / true |
| `显示图标` | true / false |
| `替换文本` | text |
| `替换图标` | instance swap |

抽象规则：

- Tabs 用于同层级内容切换。
- `tabs/line` 是默认页面/模块切换。
- `tabs/card` 和 `tabs/card-gutter` 适合容器内分组。
- `tabs/text` 适合轻量切换。
- 数量多或容器窄时启用滚动或改为 Menu/Select。

### 4.26 Steps

已检索资产：

| 组件 | key | 语义 |
|---|---|---|
| `steps` | `f5d9afadc348dff929a0e204ce119308c56be61e` | 步骤条 |
| `steps-dot` | `1ca8aad78256e7c23aacdc39d5759e5e896e4923` | 点状步骤条 |
| `steps-arrow` | `9d7cb33d3406963d0c0b8996e7a46db1a2cbcac8` | 箭头步骤条 |
| `steps-navigation` | `df38f236c3cb68463168a0109e2cf7f1d84ef26f` | 导航步骤条 |
| `components/steps-item` | `865b19f2a9b17e65fdc300f556286997c8cd56a5` | 步骤项 |
| `components/steps-item-icon` | `b13b0bd982999df734df30602c2636c1b8a04b1c` | 图标项 |
| `components/steps-item-dot` | `94019a84eeffe3e3d79868076dbb7b5ae6105d98` | 点状项 |
| `components/steps-item-arrow` | `d4848a7455334e31d41b7a2dec5695ea72bf1380` | 箭头项 |
| `components/steps-item-navigation` | `c478e4d21a9aff4c7f0f5c3ba0483a452df239d1` | 导航项 |

已验证属性：

| 组件 | 轴/属性 |
|---|---|
| `steps` | `布局=水平/垂直`，`数量=3/4/5/6`，`小型=true/false`，`描述=true/false`，`连接线=true/false` |
| `steps-item` | `布局=水平/垂直`，`小型=true/false`，`状态=完成/进行中/等待/错误`，`图标=true/false`，`描述=true/false`，`链接线=true/false` |
| `steps-item-icon` | `状态=完成/进行中/等待/错误` |

抽象规则：

- Steps 只表达有顺序的流程、向导、进度。
- 不用 Steps 表达无序状态列表。
- marker 只放数字、勾号、错误图标或系统 marker；业务文案放 title/description。
- 容器宽度不足或步骤过多时改用垂直布局。

### 4.27 Menu / Dropdown / Anchor

已检索资产：

| 组件 | key | 语义 |
|---|---|---|
| `menu` | `039758bf18b61dd42058a0df3184df826b2656df` | 菜单 |
| `dropdown-menu` | `66906f611d00f4f9a86ce8f582b17d3a92f99ae6` | 下拉菜单 |
| `Extended menu` | `1bd578a7954b3f2f01f24e6fafd23e4dbdbb4844` | 扩展菜单 |
| `pop-menu-item/1st-level` | `04d90e7da273efd07d4b146af9f4118f2d11283b` | 弹出菜单一级项 |
| `pop-menu-item/2nd-level` | `e44c4d79921d16e77be2c08af14cff923fe02ae6` | 弹出菜单二级项 |
| `vertical-menu-item/1st-level` | `5a912a784ea010fdb2ad6f90ee57ca17232d2d91` | 垂直一级菜单项 |
| `vertical-menu-item/2rd-level` | `f0905b5ae39666e755b6fe1d2ba9d6e79f018768` | 垂直二级菜单项 |
| `horizontal-menu-item/1st-leve` | `2d9ef2a7677c63a8fcdb912752aaba0779098189` | 水平菜单项 |
| `components/dropdown/menu-item` | `9d5ca28e2401acf21eadfe080682e22bb9f46dcf` | 下拉菜单项 |
| `components/dropdown/menu-item-group` | `08e51df7f14a22604a6fd84e39cc31069d0832e9` | 分组 |
| `components/dropdown/menu-item-devider` | `d53c3f5f393e8ae762f99b0229280842b72ac0e2` | 分割线 |
| `Anchor` | `e05484af35673b7445b04461548815a8e090766e` | 锚点 |

抽象规则：

- Menu 表达层级导航。
- Dropdown 表达当前上下文的动作集合。
- Anchor 表达页面内目录和滚动定位。
- 不用 Tabs 表达深层级导航；不用 Dropdown 承载页面主导航。

### 4.28 Form

已检索资产：

| 组件 | key |
|---|---|
| `form` | `a181887f88b381324a3ab0192ae243975699383f` |
| `form-item` | `18b02f4a4ab0b2412648efc794a5a240ffe393b6` |

已知 form-item 属性：

| 轴/属性 | 值 |
|---|---|
| `布局` | 依资产定义，如水平/垂直 |
| `类型` | 依资产定义，如默认/错误等 |
| `必填` | true / false |
| `提示` | true / false |
| `辅助文字` | true / false |

抽象规则：

- 每个输入控件都应由 form-item 承载标签、必填、提示、辅助文字和错误信息。
- 错误不应只是散落红字，应归属到字段。
- 表单按钮放 footer；字段解释靠近字段。

### 4.29 Empty / Carousel / BackTop

已检索资产：

| 组件 | key | 语义 |
|---|---|---|
| `empty` | `25df138f4dd76ecd26eded7028e63fb6553ce8db` | 空状态 |
| `Carousel` | `3218ad7cef4d901fbe1ec7a442f48da175842985` | 轮播 |
| `components/indicator-bottom` | `25146c8ffdb76e55dd4af0c1d26912e11071cf72` | 轮播指示器 |
| `BackTop` | `e1c066c7c9b4eaca30241c8a71cf6be7987a09c4` | 返回顶部 |

抽象规则：

- Empty 用于无数据、无结果、无权限、未配置等状态；需要给出下一步动作。
- Carousel 不适合高频操作型中后台，只有内容轮播、公告、引导时使用。
- BackTop 用于长滚动页面，不用于普通短页面。

### 4.30 Description / Metric Helpers

已检索资产：

| 组件 | key | 语义 |
|---|---|---|
| `descriptions-inline-vertical` | `519cef1675b5d0480e34c65528c7b480224c05be` | 单列描述列表 |
| `components/description-item/label` | `9ca96c60833f532549edd7c3c736cc62d6c1309e` | 描述项标签 |
| `components/description-item/value` | `66059db30583dcc1550efd2584d73ae99ba9a8f8` | 描述项值 |
| `components/bordered-item/label` | `7ef7698dd3fc1e89b3b62f772df7909f8f9b578c` | 带边框标签 |
| `components/bordered-item/value` | `22c57ccba9e4f44079008ef96029421d2dc2081a` | 带边框值 |
| `components/trend` | `3e259d6474c376e23aa0c2ff43c25dd454cb29e0` | 趋势 |
| `components/percentage` | `bcfd152ac37fa2a296676745fd17646dfaaac05e` | 百分比 |

抽象规则：

- 键值信息用 Descriptions，不手动对齐散文本。
- 趋势/百分比用于指标辅助，不作为主状态。
- 数字、趋势、单位和解释文本要有清晰层级。

### 4.31 数据可视化图表库

补充 library：`源力设计体系-图表库`，library key `lk-a3da2a4c46bf632b0087bf59bbf692c935855e5e43ccdc92b9bc4d9211e6480d468df1801feddcc717033d4eb400c04580f8d7c6f817fd9db44f597ec0727931`。

图表库用于 Arco 中后台页面的数据可视化区域。表单、筛选、表格、卡片容器、状态、动作仍使用 Arco；折线、面积、柱状、Toplist、饼图、环形图、图例、tooltip、阈值线等图表本体使用图表库组件。

已验证 8 个 component set：

| 组件 | key | 适合场景 |
|---|---|---|
| `折线图` | `62d6b59603766fdb416ff787eec5d21800264694` | 标准时序趋势、指标变化、峰谷判断 |
| `折线组合/dataLinesCombo` | `fb05d0401cd1498466b2d5ee86d5a7723bc95a39` | 趋势 + tooltip + 阈值 + 对比 + 预测，适合值班台主图 |
| `AreaChart 面积图` | `99fdb5caaa7ae3a429f0bb83022f737cd34caa01` | 趋势背后的规模感、吞吐量、累计量变化 |
| `BarChart 柱状图` | `a83efa5b5ba4efbdb96694268b50e43a61bee971` | 分类对比、周期对比、多系列并列/堆叠比较 |
| `Toplist 条形图` | `6acea515cbcd1ae970ef5627425bd55cbda137ff` | TopN 排名、对象/Owner/服务/地域分布 |
| `Card/PieChart` | `a414c3e671b3619d480d4932b83d9969b7ebbe03` | 带卡片、标题、图例的完整占比模块 |
| `Component/PieChart` | `ce1607d6b31f82f34fc33fe342bdcfd04eb33b9e` | 纯饼图/环形图，嵌入已有 Arco Card 或自定义模块 |
| `Mini_pie_chart` | `4813f178fd503bfdad6cef802eb09ca927d4e733` | 小面积 2-5 类占比提示，适合表格行、概览卡、小浮层 |

#### 4.31.1 组件选择规则

| 需求 | 首选组件 | 不建议 |
|---|---|---|
| 看“今天/近 24 小时发生了多少，趋势是否抬升” | `折线图`、`折线组合/dataLinesCombo` | 用饼图表达时间变化 |
| 看“是否超过阈值、是否有预测段、hover 后读具体点位” | `折线组合/dataLinesCombo` | 手绘阈值线、tooltip 或游标 |
| 看“量感、面积、吞吐是否持续扩大” | `AreaChart 面积图` | 多条面积互相遮挡的精细对比 |
| 看“不同类别/时间段谁更高” | `BarChart 柱状图` | 用 Toplist 承载时间序列 |
| 看“Top 服务/Owner/地域/模板分布” | `Toplist 条形图` | 用饼图展示超过 5 个分类 |
| 看“少量固定分类的占比” | `Card/PieChart`、`Component/PieChart`、`Mini_pie_chart` | 用柱状图表达纯构成关系 |

告警中心/值班台类页面建议：

- 主趋势：`折线组合/dataLinesCombo`，展示近 24 小时告警量、高优事件量、昨日对比、阈值线和 tooltip。
- 维度分布：`Toplist 条形图`，展示服务、Owner、Region、规则模板、告警套餐等 TopN。
- 等级占比：分类只有 Notice/Warning/Critical 等少量固定项时用 `Card/PieChart` 或 `Component/PieChart`。
- 关键指标卡：数字和趋势符号使用 Arco Metric Helpers；不要把小饼图当作主信息。

#### 4.31.2 通用图表属性语义

| 属性 | 语义 | 使用规则 |
|---|---|---|
| `Item` / `分类数量 Item` | 分类数量 | 必须贴合真实分类数；饼图超过 5 类优先换 Toplist |
| `数量 #of lines` / `线数量` | 数据系列数 | 表示 series 数，不是数据点数量 |
| `Show Scale` | 是否显示坐标刻度 | 需要读具体量级时打开；只看走势可关闭 |
| `Show Legend` | 是否显示图例 | 单系列可关闭，多系列必须打开 |
| `图例 legend=数值对比 metrics` | 图例中展示数值对比 | 适合 dashboard 中展示当前值、环比、同比 |
| `状态 state` / `State` / `悬浮 Hover` | 交互态 | 只用于 hover/focus 原型态，不表达业务状态 |
| `Color mode` / `色彩模式` | 图表色彩强度 | 主图用常规；辅助、小面积、背景弱区可用轻亮 |
| `适配方式 responsive` | 宽度变化时的柱/间距策略 | 类目固定用固定柱宽；类目数量变化用固定间距 |

#### 4.31.3 折线图

属性：

| 属性 | 值 |
|---|---|
| `数量 #of lines` | 1 / 2 / 3 / 4 / 5 / 6 |
| `布局 layout` | 上下 stacked / 左右 side by side |
| `图例 legend` | 默认 Default / 数值对比 metrics |
| `Show Scale` | boolean，默认 false |
| `Show Legend` | boolean，默认 true |

使用规则：

- 用于标准趋势判断，例如告警事件趋势、高优事件趋势、MTTA/MTTR 变化。
- 多条线表示同一量纲下的多个系列；不同量纲不要强行放在同一折线图。
- `Show Scale` 默认可关闭，减少 dashboard 噪音；需要精确读数或运维排障时打开。
- `legend=metrics` 适合图例旁展示当前值、环比或同比。

#### 4.31.4 折线组合/dataLinesCombo

属性：

| 属性 | 值 |
|---|---|
| `类型 Type` | 默认 default / 平滑 smooth / 大数据 big data |
| `线数量` | 1 / 2 / 3 / 4 / 5 / 6 |
| `对比 Compare` | True / False |
| `预测 Projection` | True / False |
| `tooltip 位置 position` | 右 right / 左 left |
| `Show Threshold` | boolean，默认 false |
| `Show Tooltip` | boolean，默认 true |

使用规则：

- 优先用于值班台、监控台、告警趋势主图。
- 需要展示阈值线时打开 `Show Threshold`，不要手绘横线。
- 需要同比/昨日/上周对比时打开 `对比 Compare`。
- 需要预测段或未来趋势示意时打开 `预测 Projection`。
- 点位很密集时使用 `类型 Type=大数据 big data`；需要柔和走势时用 `平滑 smooth`。
- tooltip 位置按画布空间选择，避免遮挡关键峰值或图例。

#### 4.31.5 AreaChart 面积图

属性：

| 属性 | 值 |
|---|---|
| `数量 #of lines` | 1 / 2 / 3 / 4 / 5 / 6 |
| `布局 layout` | 上下 stacked / 左右 side by side |
| `图例 legend` | 默认 Default / 数值对比 metrics |
| `Show Scale` | boolean，默认 true |
| `Show Legend` | boolean，默认 true |

使用规则：

- 用于强调趋势背后的规模感，例如吞吐量、告警总量、事件积压量。
- 不适合很多系列的精细比较；面积互相遮挡时改用折线图。
- 多系列时必须保留图例，避免颜色含义不清。

#### 4.31.6 BarChart 柱状图

属性：

| 属性 | 值 |
|---|---|
| `类型 type` | 基础/分组柱 default / 堆叠 stacked / 百分比堆叠 stacked part to whole |
| `数量 #of lines` | 1 / 2 / 3 / 4 |
| `适配方式 responsive` | 固定柱宽 fixed width / 固定间距 fixed gap |
| `状态 state` | 默认 Default / 悬浮 hover / 聚焦 focused |
| `Show Scale` | boolean，默认 true |
| `Show Legend` | boolean，默认 true |

使用规则：

- 基础/分组柱用于并列比较。
- 堆叠柱用于每个分类内部构成。
- 百分比堆叠用于比较构成比例，不用于比较绝对数。
- 系列数超过 4 或分类过多时，应拆分或改用 Toplist。

#### 4.31.7 Toplist 条形图

属性：

| 属性 | 值 |
|---|---|
| `类型 type` | 基础/分组柱 default / 堆叠 stacked / 百分比堆叠 stacked part to whole / 特殊 special case / 特殊 special case 2 |
| `数量 #of lines` | 1 / 2 / 3 / 4 |
| `适配方式 responsive` | 固定柱宽 fixed width / 固定间距 fixed gap |
| `状态 state` | 默认 Default / 悬浮 Hover / 聚焦 Focus |
| `Show Legend` | boolean，默认 true |

使用规则：

- 用于 TopN 和维度分布，例如服务告警量 Top10、Owner 待处理事件 Top10、Region 分布。
- 单指标排名用基础形态；每个对象内部还要看等级构成时用堆叠；只看构成比例时用百分比堆叠。
- 分类名较长时优先用 Toplist，而不是竖向柱状图。
- `特殊 special case` 只在现有 variant 视觉正好符合目标时使用，不作为默认选择。

#### 4.31.8 饼图 / 环形图

`Card/PieChart` 属性：

| 属性 | 值 |
|---|---|
| `类型 Type` | 饼图 PieChart / 环形图 DonutChart |
| `布局 Layout` | 上下 UP To Down / 左右 Left To Right |
| `数据展示 Statistic` | On / Off |
| `分类数量 Item` | 2 / 3 / 4 / 5 / 6 / 7 / 8 / 9 / 10 |
| `总数值 Sum` | On / Off |
| `状态 State` | 默认 Default / 悬浮 Hover |
| `色彩模式 Color mode` | 常规 Regular / 轻亮 Light |

`Component/PieChart` 属性：

| 属性 | 值 |
|---|---|
| `类型 Type` | 饼图 PieChart / 环形图 DonutChart |
| `分类数量 Item` | 2 / 3 / 4 / 5 / 6 / 7 / 8 / 9 / 10 |
| `数值标注 Data Annotation` | On / Off |
| `色彩模式 Color mode` | 常规 Regular / 轻亮 Light |
| `悬浮 Hover` | On / Off |
| `总数值 Sum` | On / Off |
| `放大比率 Ratio` | 2:1 / 1:1 |

`Mini_pie_chart` 属性：

| 属性 | 值 |
|---|---|
| `Type 类型` | 饼图 PieChart / 环形图 DonutChart |
| `数据展示 Statistic` | On / Off |
| `分类数量 Item` | 2 / 3 / 4 / 5 |

使用规则：

- 饼图/环形图只用于少量构成关系，不用于时间趋势或 TopN 排名。
- 完整 dashboard 模块优先 `Card/PieChart`；已有 Arco Card 容器里嵌图用 `Component/PieChart`；表格行或小卡片用 `Mini_pie_chart`。
- 环形图需要中心总数时打开 `总数值 Sum`。
- 分类超过 5 个时，用户很难读清占比差异，优先改用 `Toplist 条形图`。
- `数值标注 Data Annotation` 用于读具体占比；标签太密时关闭，改由图例展示。

### 4.32 Icon / Primitive Components

Arco library 包含大量 icon、direction、tips、edit、general、components/header/footer/item 等子资产。

抽象规则：

- icon 资产只表达图形语义，不承载业务文本。
- `tips/*` 图标用于反馈类型；配合 Alert、Message、modal-confirm、Tooltip 使用。
- `direction/*` 图标用于展开、跳转、方向、分页、下拉。
- `edit/*` 图标用于编辑、复制、排序等动作。
- `components/header/footer/item` 是结构子件；除非父组件缺失，否则不直接拼页面。

## 5. 高组件化实施规则

### 5.1 导入优先级

1. 优先导入公开父组件，如 `Button`、`input`、`select`、`table`、`tabs/line`。
2. 父组件不能表达时，导入公开子组件，如 `badge/status`、`components/table-cell/action`。
3. 图表区域优先导入 `源力设计体系-图表库` 的公开图表 component set，如 `折线图`、`Toplist 条形图`、`Card/PieChart`。
4. 只有构建复杂组合时才使用内部 `components/...` 子件。
5. 不手绘已有控件的状态、边框、图标、表格单元格、分页、Tabs、折线、柱、饼图、图例、坐标轴或 tooltip。

### 5.2 组合边界

- 页面布局 Frame 可以自建。
- 交互控件必须组件化。
- 数据表必须用 table/table-cell 体系。
- 数据图表必须用图表库体系；Arco Card 只承载外层容器，不负责手绘图表本体。
- 弹层和确认态必须用 modal/drawer/modal-confirm 体系。
- 状态、标签、角标必须用 Tag/Badge 体系。
- 表单字段必须由 form-item 组织。

### 5.3 文本写入

Figma API 修改中文文本时：

- 先读取目标文本层，确认它是内容层。
- 需要时加载或替换为可用中文字体，如 `Noto Sans SC Regular/Medium/Bold`。
- 不写入 icon、dot、step marker、checkbox 方块、switch 滑块等结构层。
- 文本长度变化后检查容器宽高和 auto-layout，不允许文字溢出或遮挡。

### 5.3.1 组件实例内容写入规则

控件内容必须写入组件实例本身，禁止用覆盖层伪造内容或样式。这里的“覆盖层”包括但不限于 `Select cover`、`Input cover`、`Button text cover`、`Switch cover`、`Text / ⌄`、放在控件上方的业务文本、遮挡默认 placeholder 的矩形。

正确顺序：

1. 创建或定位 Arco 组件实例后，先读取 `instance.componentProperties`。
2. 如果组件暴露 `TEXT` property，例如 `替换文本`、`placeholder`、`Button Text`，必须用 `instance.setProperties()` 写入。
3. 如果组件没有暴露可用 `TEXT` property，但实例内部有明确内容文本层，例如 Select 的 `title`、Input 的 `input`、Button 的 `Button Text`，必须加载该文本层字体后直接修改 `characters`。
4. 如果内部文本层也无法可靠修改，不要交付 cover 方案；应改用可覆盖文本的同类公开组件，或停下来说明该组件资产缺少必要文本槽位。

禁止做法：

- 不得把 Arco 组件当成灰色底板，再用 sibling `TEXT` 或 `RECTANGLE` 覆盖其默认内容。
- 不得用隐藏/可见的假箭头模拟 Select、Dropdown、Cascader 的展开图标。
- 不得隐藏组件默认文本后，在组件外部相同位置另放业务文本来假装组件内容。
- 不得为了“看起来像组件”保留一个不可维护的组件实例，同时把真实可见内容放在外层。
- 不得在最终交付中保留命名为 `* cover`、`Text / ⌄`、`fake-*`、`overlay-*` 的控件伪装层。

允许例外：

- 独立的字段 label、helper text、错误提示、单位文本、表格外的说明文字可以是组件外部文本，因为它们不是控件内部内容。
- 图表库内部为了裁切无关装饰可使用明确命名的 `Chart viewport/*`，但不能裁掉业务图形、图例、标签或数值。

### 5.3.2 真实组件实例交付协议

使用 Arco 组件不是“把组件放在画布上看起来像”，而是让可见控件本身就是可维护的组件实例。交付前必须确认用户看到的文本、状态、图标、尺寸和禁用态来自组件实例或实例内部内容层，而不是来自覆盖层、手绘图标或兄弟文本。

通用流程：

1. 搜索并导入公开父组件，创建实例后立即读取 `componentProperties`。
2. 先用组件暴露的属性覆盖文本、状态、尺寸、类型、图标、禁用态和交互态。
3. 属性不暴露文本时，定位实例内部唯一业务文本层，加载字体后写入 `characters`，并保证该文本层仍在实例内部。
4. 写入后检查实例内部文本的 `visible`、`opacity`、`fills`、`fontName`、`fontSize`、`textAutoResize` 和 `absoluteBoundingBox`，避免文字实际存在但不可见或被裁切。
5. 如果以上方式不能得到可靠控件，必须换同类组件、拆成组件允许的公开子件，或记录资产缺口；不能用 cover、mask、opacity 或外部文本让截图暂时正确。

常用控件规则：

- Select：必须使用 `select` 实例表达输入框；下拉菜单展开态使用 `dropdown-menu`、`selection-item` 或组件库中的下拉选项子件。选中值应写入 Select 实例内部 `title` 或暴露文本属性；右侧箭头使用组件自带 icon，不手写 `⌄` 或额外图标。
- Input / SearchBox：输入值、placeholder、前后缀和禁用态应来自 input/search-box 组件本身。不得用灰色矩形 + 外部文本伪装输入框；不得把搜索 icon 手绘到 input 上。
- Button：按钮文本必须来自 Button 实例属性或内部 `Button Text`；图标按钮使用 icon slot/instance swap。不得用空按钮底板加外部文本，也不得用外部矩形覆盖按钮颜色。
- Tag / Badge / Status：状态语义来自组件 variant 和颜色，不用圆点 + 文本手拼。错误、失败、危险不可使用成功色；处理中、待处理、未知和禁用必须区分。
- Form field：字段 label/helper/error 可以是外部文本，但控件主体必须是真实组件实例。label/control 应在同一栅格里，不用 cover 修补错位。
- Dropdown / Cascader / TreeSelect：需要层级、搜索、多选或展开面板时，使用对应组件或公开子件；不要用普通 Select 加缩进文本、假箭头和手写菜单项模拟。

图层审计规则：

- 同一个控件区域内，业务文本只能有一个真实来源。若组件实例内部已有文本层，不能再放同位置 sibling 文本。
- 组件实例附近出现名称包含 `cover`、`fake`、`overlay`、`Text / ⌄`、`Button text`、`Select value`、`Input value` 的节点时，默认视为问题，除非它们是明确的字段 label/helper 或非控件说明。
- 审计时不能只看可见图层，也要扫描 `visible=false` 的隐藏伪装层；隐藏 cover 会误导后续维护者。
- 批量修复时，先在一个页面验证文本写入和截图，再推广到同类组件；不要一次性删除所有内部 placeholder 或所有 sibling 文本。

### 5.4 密度规则

- 默认页面和表单用 `尺寸=中`。
- 密集表格、筛选栏、行内操作可用 `小`。
- 页面主操作、关键确认、宽松弹层可用 `大`。
- `迷你` 只用于小空间辅助动作或组件内部按钮。

### 5.5 状态映射规则

| 真实语义 | 推荐组件 | 推荐值 |
|---|---|---|
| 中性、默认、未开始、普通 | `badge/status` | 默认 |
| 处理中、加载中、进行中 | `badge/status` | 进行中 |
| 完成、成功、健康、已启用 | `badge/status` 或 `Tag` | 成功 / 仙野绿 |
| 需要关注、待确认、风险提示 | `badge/status` 或 `Alert` | 提醒 / 警告 |
| 错误、失败、异常、危险 | `badge/status`、`Alert`、`modal-confirm` | 错误 / 危险 |
| 分类、属性、来源、短标签 | `Tag` | 按语义选择颜色 |
| 数量提醒 | `badge/count` | count |

### 5.6 浮层与动作态

- Modal、Drawer、Confirm 应作为独立 top-level frame 表达，不要覆盖在主页面画板里造成遮挡。
- 需要展示触发关系时，用 frame 命名、短说明或 prototype 连接。
- 每个浮层应有 header、body、footer 的清晰结构。
- Footer 右侧放按钮组；取消/返回在左，主要/确认在右。
- 危险确认的主按钮必须使用危险语义。

### 5.7 表格结构

- 一行数据必须放在同一个 row frame。
- 各列 cell 高度一致。
- 行分割线由 row 或 table 统一绘制。
- 操作列、状态列、标签列里的组件垂直居中。
- 表头排序、筛选、搜索必须使用 header variant。
- 表格下方分页与表格保持清晰间距并右对齐。

### 5.8 布局质量闸门

从 PRD 或说明生成中后台原型时，组件实例正确不等于页面可交付。写入 Figma 后必须做一次截图级 QA，重点检查以下问题：

- 表单必须有稳定的 label/control 栅格。标签列应在卡片内，宽度固定且右侧留出 16-24px 间距；控件列必须从同一 x 坐标开始。不要把 `form-item` 默认 label、手写 label 和手写 value 混在一起造成重叠。
- 如果 Arco 组件的 `TEXT` property 无法可靠覆盖，必须改为修改组件实例内部的真实文本层；不能用矩形或外部文本覆盖组件默认 placeholder。若组件没有可编辑文本槽位，应更换同类组件或记录资产缺口，不交付伪装控件。
- 列表页必须先定义唯一的 `contentLeft` / `contentRight`。顶部操作区、指标卡组、筛选栏、表格 header/body、表格 footer 分页必须共享同一右边界；不要让表格比筛选栏窄，也不要让分页独立贴到另一个右边界。
- 卡片内表格的列宽总和必须小于内容区宽度：`sum(columnWidth) + leftPadding + rightPadding <= containerWidth`。右侧操作列、标签列、表格 cell 不能被卡片裁切，也不能露出空白竖条。
- Pagination 默认不要开启 `展示条目` 或 `跳转`，除非 footer 预留了足够宽度。空间不足时只保留页码和总数，避免 page-size selector 挤出表格区域。设置实例尺寸时必须覆盖完整可见页码结构，不要让实例 width 小于内部 showTotal、prev、page item、ellipsis、last、next 的总宽度。
- 数据不足一页时，Pagination 不应紧贴最后一行表格；应吸附到列表内容区底部或预留的 table footer 区域，和最后一行至少保持 96px 以上呼吸区，避免页面下半部显得被截断。
- 表格内非交互 Tag 应按文本内容 hug，不要拉满单元格或保留关闭/图标槽造成大片空白。业务域、等级、短状态等 Tag 通常只需要 `text width + 16px` 左右内边距，短等级 Tag 可用 42-46px 的稳定宽度。
- 范围、过滤条件、DSL、表达式等高出错成本输入，不应默认做成纯 `textarea`。优先拆成条件构建器：字段 `select`、操作符 `select`、值域 `Tag`/多选输入、行级新增/删除动作，并提供只读生成表达式预览。纯文本输入只作为高级编辑或兜底入口。
- 条件构建器必须按字段类型选择控件：枚举字段用 `select`/Tag 多选，布尔字段用 switch/radio，数值字段用 InputNumber，时间字段用 date/time picker；不要让所有字段共用一个自由输入框。
- 需要用户编写表达式、模板、变量插值或脚本片段时，必须在编辑区附近提供“可用变量/字段”清单。变量应以代码样式 token 展示，配含义说明和复制动作；变量清单不应只是长说明文字，也不要挤成难读的标签堆。
- Dashboard、值班台、监控台必须先定义信息任务：看趋势用折线/折线组合，看规模用面积，看对比用柱状，看排行用 Toplist，看少量构成用饼/环形；不要因为视觉占位方便而随意换图表类型。
- 图表的 `State`、`Hover`、`Focus` 只表达交互态，不表达告警已处理、失败、待确认等业务态。业务态仍应使用 Tag、badge/status、Alert、表格状态列或详情文案。
- 图表系列数必须和图例数量一致；`数量 #of lines`、`线数量`、`分类数量 Item` 要贴合真实数据结构。不要保留组件默认的 `P90/P95/Item 1` 等样例文案。
- 图表容器必须给出稳定高度和宽度；图例、tooltip、坐标刻度、标题、筛选控件不能压到图形区域，也不能与 Arco Card header/footer 重叠。
- Modal/Confirm 内的 Radio、Checkbox、Alert、表格不能同时使用组件内置文本和额外手写说明覆盖同一区域。若需要长说明，应拆成两行：第一行选项标题，第二行弱说明，并保持 8-12px 垂直间距；内容下移后必须同步调整弹窗 body/footer 高度，避免底部按钮压线或出界。
- Drawer、Modal 等组件如果只是为了保留资产实例，不得以低透明度留在可见画布中；应放到不可见参考区或设为 `visible=false`。低透明度组件残影会干扰真实内容判断。
- 右侧详情栏、预览卡片、复盘数据等纵向区块必须显式留白：区块标题、辅助状态、卡片之间至少 12-16px；长 KV 文本不能穿过分割线或覆盖后续标题。
- 修复样式时不要大范围深扫组件实例内部节点。优先操作已知 frame 的直接子节点；如需修改实例内部文本，先截图验证，避免隐藏所有 `placeholder` 或 overlay 时误删有效业务内容。
- 每次批量生成或修复后，至少截图检查：一个列表页、一个表单页、一个弹窗页、一个详情页。检查默认文案残留、文字溢出、裁切、重叠、分页越界、组件残影。

### 5.9 返工高发问题防线：容器收口、表格归位、图表完整

以下问题在 Figma 原型中高频返工：组件外层 frame 高度没有随内容收口、表格或 Alert 浮在容器外、图表库组件被错误裁切、画布 frame 没有随页面内容增高。出现这些问题的根因通常不是单个组件选错，而是写入后没有做结构边界审计。

常见根因：

- 只移动了子节点，没有同步调整父级 Card、Modal、Drawer、Popover、页面 frame 的高度。
- 用绝对坐标拼页面后，没有在内容增删、文本换行、表格列宽变化后重新计算父容器 `bottom`。
- 把 imported component 的外观当成最终边界，忽略了 instance 内部默认 header、legend、sample text、tooltip 或行列子节点的真实 `absoluteBoundingBox`。
- 表格只改了 table 外框宽度，没有同步压缩或移动直接列 cell，导致最后一列仍伸出容器。
- 图表库组件为了适配版面被放进裁切 frame，但没有确认图形主体、图例、坐标和业务标签都在可见视口内。
- 截图 QA 只看局部页面，没有用 Figma API 检查所有 top-level frame 和关键容器的可见子节点边界。

硬性修复原则：

- 每个 top-level 页面 frame 的高度必须由最终可见内容决定：`frameHeight >= maxVisibleChildBottom - frameY + bottomPadding`。中后台页面底部建议至少保留 64-96px 空白；长表单、长抽屉和流程页可以更高，但不能让内容贴底或出界。
- Card、Modal、Drawer、Popover、Confirm、Alert 容器必须包住其可见内容。内容下移、表格加行、说明换行、按钮组增减后，必须同步调整容器高度和后续区块 y 坐标。
- 表格必须同时满足三层宽度关系：`sum(columnWidth) <= tableWidth`、`tableWidth + cardPadding * 2 <= cardWidth`、`tableRight <= contentRight`。如果修改 table width，必须检查所有直接列 cell/header/action cell 的 `x + width` 是否仍在 table 内。
- 表格、Alert、图表、分页不能靠“视觉上盖住”来解决溢出。除非是明确命名的图表 viewport/crop，否则不要用 mask 或 clipping 掩盖布局错误。
- 图表库组件如果需要裁切，只能裁切在明确命名的图表视口里，例如 `Chart viewport / trend`、`Chart viewport / toplist`，并设置 `clipsContent=true`。裁切后必须保证用户需要读的图形主体、图例、行标签、数值和 tab 不被裁掉；不能保留组件默认的 `我是标题`、`Item 1`、`P90/P95` 等样例信息。
- Toplist/条形图用于分布时，优先完整展示条形、标签和数值。若图表组件默认结构难以直接表达业务行标签，可保留图表本体作为可视化组件，同时在同一容器内用清晰业务文本补充行标签和数量；但业务文本必须在容器内且与条形一一对应。
- 修完一个局部问题后，必须联动检查相邻区块和整页画布。不要只修红框区域；红框通常只是父容器高度、后续区块 y 坐标或全页 frame 高度失效的表现。

交付前必须做程序化边界审计：

- 对每个 top-level frame，遍历所有 `visible !== false` 且有 `absoluteBoundingBox` 的后代节点；如果该节点没有处于显式 `clipsContent=true` 的裁切祖先内，则它必须完全落在页面 frame 内。
- 对每个关键容器（名称或类型包含 `Card`、`Modal`、`Drawer`、`Popover`、`Confirm modal`、`Arco Table`、`Table`、`Alert`），遍历其可见后代；除明确命名的图表 viewport/crop 外，后代必须完全落在容器内。
- 对图表 viewport/crop，单独检查：容器有稳定宽高、`clipsContent=true`、图形主体可见、业务标签和数值完整、没有样例文案残留。
- 对列表页和卡片内表格，单独检查 table 直接子节点右边界，避免 table 外框已收窄但最后一列仍溢出。
- 程序化审计通过后，再截图复核列表页、表单页、弹窗/抽屉页、图表页和详情页；截图用于发现视觉问题，脚本用于发现结构边界问题，两者缺一不可。

## 6. 设计前检查清单

1. 是否已在绘制前确认并加载 `Arco Design System (New)` 和 `源力设计体系-图表库` 两个 required library？
2. 是否已用 `search_design_system` 搜索 Arco Design System (New)，且限定 Arco library key？
3. 搜索结果是否确认 `libraryName = "Arco Design System (New)"`？
4. 若页面包含图表，是否已用 `search_design_system` 搜索 `源力设计体系-图表库`，且限定图表库 library key？
5. 是否优先使用公开父组件？
6. 是否避免用内部 `components/...` 直接拼已有父组件？
7. 每个组件的 `VARIANT` 是否只表达组件设计轴？
8. 是否没有把交互态当业务态？
9. 是否没有用禁用态解释失败、未开始、无权限等业务原因？
10. 表单字段是否由 `form-item` 承载？
11. 搜索是否使用 `search-box`，而非 input + 手绘 icon？
12. 枚举、层级、多选、时间范围是否选择了专用组件？
13. 表格是否使用 table/table-cell/table-column/pagination 体系？
14. 状态是否按强度选择 Tag、badge/status、Alert、Message、Confirm？
15. 危险动作是否使用危险语义和确认框？
16. 图标按钮是否提供 Tooltip？
17. Steps marker 是否只包含 marker，不包含业务长文案？
18. Switch 是否只用于立即生效二元开关？
19. Modal/Drawer/Confirm 是否有 header/body/footer 清晰结构？
20. 文本是否写入内容层且不溢出？
21. 是否避免手绘已有组件状态、边框、分割线和图标？
22. 是否在最终交付前检查组件实例仍保留为 instance？
23. Select/Input/Button 等控件内容是否写入组件 `TEXT` property 或实例内部文本层，而不是外部覆盖层？
24. 画布中是否不存在可见或隐藏的 `Select cover`、`Input cover`、`Button text cover`、`Text / ⌄`、`fake-*`、`overlay-*` 等伪装控件层？
25. 表单 label/control 是否在同一栅格内，且没有 label 与控件重叠？
26. 表格列宽总和是否严格小于容器内容宽度？
27. Pagination 是否因为 page-size selector 或跳转输入越界？
28. Modal/Drawer 内是否存在低透明度组件残影、双文本源或区块标题重叠？
29. 是否已用截图抽检列表、表单、弹窗、详情四类页面？
30. 列表页顶部操作、指标卡、筛选栏、表格、分页是否共享同一内容右边界？
31. Pagination 实例宽度是否覆盖内部完整页码结构，且少量数据时已吸附到列表底部？
32. 表格中的业务域、等级、短状态 Tag 是否按内容收窄，而不是拉满单元格？
33. 范围/过滤/DSL 输入是否已优先拆成字段、操作符、值域和加删行，而不是纯 textarea？
34. 条件构建器是否根据字段类型选择 select、Tag 多选、InputNumber、radio/switch 或时间控件？
35. 表达式/模板编辑区附近是否提供可复制的变量清单，并解释每个变量的含义？
36. 图表区域是否已搜索并使用 `源力设计体系-图表库`，而不是手绘折线、柱、饼、图例、坐标轴或 tooltip？
37. 图表类型是否匹配用户任务：趋势=折线/折线组合，规模=面积，对比=柱状，排行=Toplist，少量构成=饼/环形？
38. 图表的 `数量 #of lines`、`线数量`、`分类数量 Item` 是否和真实系列/分类数量一致？
39. 图表里是否没有残留 `P90/P95/Item 1/我是标题` 等默认样例文案？
40. 图表 hover/focus/State 是否只用于交互态，没有被拿来表达业务状态？
41. 每个 top-level 页面 frame 是否已按最终可见内容重新收口或增高，并保留 64-96px 底部空白？
42. Card、Modal、Drawer、Popover、Confirm、Alert 是否包住内部可见内容，没有表格、提示、按钮或文本浮在容器外？
43. 表格外框、直接列 cell、卡片内容区、页面 contentRight 是否同时对齐，没有最后一列伸出或被裁切？
44. 图表库组件是否有足够 viewport，趋势线、条形、图例、行标签、数值和 tab 是否完整可见？
45. 若使用图表裁切，裁切容器是否明确命名、设置 `clipsContent=true`，且只裁掉组件内部无关默认区域而非业务信息？
46. 批量生成或修复后，是否已用 Figma API 检查所有页面 frame 和关键容器的 `absoluteBoundingBox` 溢出？
47. 是否已区分“图表 viewport 的有意裁切”和“普通容器布局错误”，没有用 mask/clipping 掩盖表格或 Alert 溢出？
48. 截图 QA 是否覆盖至少一个图表页，并确认图表没有默认样例文案、图例颜色不匹配、分布行不完整或被裁切？
49. Select 是否是真实 `select` 实例，选中值写入实例内部或文本属性，箭头来自组件自身而不是手写字符/外部 icon？
50. Input/SearchBox 是否是真实组件实例，placeholder/value/search icon 来自组件自身，没有 `Input cover` 或手绘搜索 icon？
51. Button 文案和颜色是否来自 Button 实例属性或内部文本层，没有空按钮底板加外部文本？
52. Dropdown、Cascader、TreeSelect 展开态是否使用对应菜单/选项组件，而不是 Select + 手写选项拼装？
53. 控件区域是否只有一个业务文本来源，没有组件内部文本和 sibling 文本同时存在？
54. 批量替换组件文本后，是否检查了内部文本层的 fill、opacity、font、auto-resize 和 bounding box，确认不是“结构正确但截图不可见”？

## 7. Figma API 实施备注

- 使用本技能前必须先通过 Figma 工具自动确认 required libraries 可用；若当前 Figma 工具提供 `get_libraries`，先检查 `Arco Design System (New)` 和 `源力设计体系-图表库` 是否在可用库列表中。
- 若使用 `use_figma` 做预加载或导入，必须先加载 `figma-use` 技能，并在工具参数中传 `skillNames: "figma-use"`。
- 搜索组件：`search_design_system({ fileKey, query, includeLibraryKeys: [library_key] })`。
- 搜索 Arco 组件时使用 Arco library key；搜索图表组件时使用 `源力设计体系-图表库` library key，避免跨库同名组件混入。
- 如果任一 required library 无法加载或搜索不到目标组件，停止绘制该部分并报告依赖缺口；不要用手绘控件、默认 Figma shape、外部文本 cover 或非目标 library 组件替代。
- 预加载应采用“探测 + 懒导入”：先搜索并验证 library/component key，再按当前页面实际需要导入组件；不要导入整库，也不要在画布保留探测节点。
- 导入 component set：`figma.importComponentSetByKeyAsync(componentKey)`。
- 导入单组件：`figma.importComponentByKeyAsync(componentKey)`。
- 创建实例后先读取 `instance.componentProperties`，再设置属性。
- 设置属性时优先用可读属性名，例如 `类型`、`尺寸`、`状态`。
- 控件文案写入必须遵守 `setProperties(TEXT property) -> 修改实例内部 TEXT 子层 -> 更换组件/报告资产缺口` 的顺序；禁止用 `cover`、外部文本或假箭头伪造组件内容。
- 对 Select/Input/Button 等控件，写入后必须读取并校验最终可见文本层：`characters` 是目标文案，`visible !== false`，`opacity > 0`，`fills` 非透明，`absoluteBoundingBox` 位于组件实例内且未被父级裁切。
- 展示下拉展开态时，不要在 select 上覆盖一个自制菜单；应导入 `dropdown-menu`、`selection-item` 或对应公开下拉组件，并用 prototype/命名表达触发关系。
- 对未知组件，不猜 variant 值；先读取 `componentPropertyDefinitions` 或实例 `componentProperties`。
- 对中文文本，先加载字体，再写入。
- 如果需要变量绑定，先确认变量 collection 和 variable key；当前 Arco Design System (New) 可见 collection 本身无变量。
- 写入或修复页面后，必须运行一次边界审计脚本：切到目标 page，遍历 top-level frame、关键容器和 table 直接子节点，用 `absoluteBoundingBox` 判断可见内容是否超出父容器；明确命名的图表 viewport/crop 可作为例外，但必须单独检查图形和业务标签完整。
- 写入或修复页面后，必须运行一次组件伪装层审计：扫描所有可见和隐藏节点名，若存在 `Select cover`、`Input cover`、`Button text cover`、`Switch cover`、`Text / ⌄`、`fake-*`、`overlay-*` 等用于控件内容伪装的节点，必须删除或改为组件实例内部覆盖后才能交付。
- 组件伪装层审计必须同时检查同一区域的双文本源：若一个 Arco 实例内部已有文本，且其外部 4px 内存在同内容或替代内容的 sibling 文本/矩形，应视为 cover 风险并修正。
- 审计脚本不要把所有 `clipsContent=true` 都视为通过。普通 Card/Table/Modal 内出现 clipping 往往是布局错误；只有名称明确为 `Chart viewport/*`、`Chart crop/*`、`Chart mask/*` 的图表视口才可被当作有意裁切。
- 对表格的宽度修复不能只 `resize(table)`；还要检查并调整 table 直接子节点的列宽、x 坐标和 action cell，确保直接子节点右边界不超过 table 右边界。
- 对外层 frame 高度修复不能只拉大画板；还要按 `maxVisibleChildBottom + bottomPadding` 收口，避免出现大面积空白或后续 frame 被重叠。

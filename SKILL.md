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
updated_at: 2026-06-08
language: zh-CN
purpose: "长期复用 Arco Design System (New) 在 Figma 原型中的组件选择、variant 语义、信息表达、数据可视化图表选择和组件化实施规则"
---

# Arco Design System (New) 抽象使用规范

## 0. 使用原则

本技能沉淀 Arco Design System (New) 的组件资产、属性语义和原型组件化规则，并补充已订阅的源力数据可视化图表库使用规则，不绑定任何具体业务。后续使用 Arco 生成 Figma 原型时，先读取本文件，再按目标产品场景选择组件。

### 0.0 强制执行协议：先门禁，后绘制

本协议必须在每次加载 skill 后优先执行。它用于把本文件中的设计规则转成可验证动作，避免执行者读过规则但继续用矩形、文本和手绘结构拼页面。

#### 0.0.0 组件选型矩阵是绘制前置产物

开始绘制前，必须先为当前需求中的每一个界面元素建立组件选型矩阵。这里的“界面元素”包括但不限于：按钮、表格、筛选、表单项、状态、标签、提示、抽屉、弹窗、步骤条、分页、Tabs、卡片、列表、图表、时间线、树、选择器、上传、空状态、加载、导航、面板、配置项、审计记录和可视化图形。

组件选型矩阵每一行必须包含：

- `需求元素`：该元素在页面中承担的真实产品职责。
- `信息形态`：单值、枚举、状态、列表、表格、流程、配置表单、反馈、导航、可视化等。
- `候选组件`：至少列出最相关的 Arco 组件或组件族；不能只写“自定义”。
- `最终组件`：明确选择的组件名称、组件库来源、组件 key 或 Figma 组件名称。
- `已核对属性`：逐项记录该组件可用的 variant、component properties、文本插槽、图标插槽、尺寸、状态、禁用态、颜色语义、布局约束和交互状态。
- `语义映射`：需求状态如何映射到 Arco 的组件属性，例如 `status=success/warning/error/processing/default`、`type=primary/secondary/text`、`disabled=true`、`checked=true`。
- `拒绝项`：说明为什么没有选择其他候选组件。
- `证据方式`：说明是通过 `search_design_system`、组件实例、`componentPropertyDefinitions`、既有页面实例或本文件组件库读取确认。

硬性门禁：

1. 没有组件选型矩阵，不得开始绘制。
2. 没有读取并核对组件属性，不得开始绘制。
3. “找不到合适组件所以手绘”不是允许结论，而是阻塞信号；必须继续查找组件库、当前文件实例、组件集属性和同类页面用法。
4. 控件、表格、状态、表单、抽屉、弹窗、图表、导航、分页、步骤、Tabs、提示、空状态等不得用 `RECTANGLE + TEXT` 伪装。
5. 只有页面网格、业务画布承载区、说明性容器等不替代组件语义的结构性布局，才可以使用 Frame、Auto Layout、Line 等基础节点；一旦基础节点被用来模拟已有组件，即为不合格。
6. 如果组件库确实缺少某类业务可视化能力，必须停止并说明“组件资产缺口”，不得交付一个看似完成但由手绘模拟的版本。

#### 0.0.1 为什么规则会反复失效

常见根因不是不知道规则，而是规则没有进入执行链路：

1. 规则埋在长文档中，执行时只读了组件库和视觉原则，没有把表格、字体、状态、抽屉、容器审计变成每次写入的必经步骤。
2. Figma API 用矩形和文本创建页面最快，若没有硬性禁止，执行者会为了推进速度手绘按钮、表格、状态和遮罩。
3. 只做截图 QA 会漏掉结构问题：截图看起来像按钮或表格，但图层上可能是矩形、外贴文字、fake status 或 cover。
4. 局部返修只改用户红框，会让同类错误继续存在于其它 frame；缺少跨 frame 批量审计。
5. 组件搜索失败或属性写入失败时，没有强制停下报告缺口，而是退化成手绘兜底。

#### 0.0.2 每次绘制必须遵守四段式流程

1. **组件选型矩阵**：在写入前先把当前需求拆成界面元素，并逐项填写 `需求元素 / 信息形态 / 候选组件 / 最终组件 / 已核对属性 / 语义映射 / 拒绝项 / 证据方式`。矩阵未完成时不得进入 Figma 绘制。
2. **组件预检**：确认 Arco library 和图表 library 可用，并搜索本次会用到的 `Button`、`table`、`table-cell/status`、`table-cell/action/link`、`select`、`input`、`steps`、`drawer`、`modal`、`Alert`、`pagination`、图表组件。缺失时停止对应绘制，不手绘替代。
3. **构造约束**：凡是组件库已有的控件、表格、状态、分页、浮层、步骤、图表，必须用实例或实例内部文本层构造；禁止用矩形、外部文本、圆点、线条或 cover 伪装。
4. **跨 frame 审计**：每次批量写入或返修后，必须扫描所有目标 frame，不只扫描当前截图页。审计范围至少包括：右侧空白、容器高度、表格列宽、操作列边界、状态颜色、主按钮数量、Drawer 遮罩层数、Drawer 宽度、流程/依赖线是否穿过文本。
5. **截图复核**：程序化审计通过后，再截图检查关键页面。截图用于发现视觉问题，不能替代组件实例和边界审计。

#### 0.0.3 交付前硬性阻断项

命中以下任一项时不得交付，应返工或报告组件/权限缺口：

1. 表格不是 table/table-cell 体系，或表头不是灰底语义。
2. 表格操作列像贴在表格外侧，或操作列超出表格列宽。
3. 表格 status 字段没有语义色，或用普通文本、Tag、手绘圆点表达流程状态。
4. 已知组件被矩形、外部文本、cover、fake layer 伪装。
5. 当前需求没有组件选型矩阵，或矩阵中的 `最终组件`、`已核对属性`、`证据方式` 为空。
6. 以“没找到组件”为理由手绘控件、状态、表格、表单、图表、浮层或反馈组件。
7. 新增文本没有使用 Arco scale、组件原字体或当前文件已验证字体。
8. 页面右侧存在连续大空白，且没有明确用途，例如浮层状态、批注区或画布留白。
9. Card、Drawer、Modal、Alert、表格或图表内容超出父容器，或父容器高度没有随内容收口。
10. Drawer 存在双层遮罩、遮罩未覆盖完整上下文、宽度不匹配任务复杂度、footer 按钮漂浮或压住内容。
11. 同一流程步骤出现多个无法解释差异的页面状态。
12. 局部修复后没有对同类页面和组件做批量审计。

#### 0.0.4 执行者必须在交付说明中报告的 QA 结果

交付或返修完成时，至少报告以下结果，不能只说“已修复”：

1. 组件选型矩阵结果：本次覆盖了哪些界面元素，每类元素选择了哪个 Arco 或图表组件，是否读取过真实属性。
2. 组件预检结果：Arco library、图表 library、关键组件是否可用。
3. 表格审计结果：表格数量、是否使用 table 体系、操作列是否在表格内、status 是否有语义色。
4. 布局审计结果：是否存在右侧大空白、容器高度异常、内容溢出。
5. 浮层审计结果：Drawer/Modal 数量、遮罩层数、宽度和 footer 是否合规。
6. 跨 frame 审计范围：检查了哪些 frame 或哪类页面，而不是只检查用户截图页。

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

### 0.2 运行环境边界

使用本技能生成 Figma 原型不要求用户本地安装 Figma Desktop。Figma 是云端设计文件，正常情况下通过浏览器中的 Figma 文件和 Codex/Figma connector 即可写入、查看和编辑原型。

必要条件：

1. 当前会话具备可用的 Figma 工具，例如 `use_figma`、`search_design_system`、`get_screenshot`、`create_new_file` 等。
2. 执行者已通过 Figma connector 认证，能访问目标 Figma 文件或创建新文件。
3. 目标文件所在团队可以访问 `Arco Design System (New)` 和 `源力设计体系-图表库` 两个团队 library。
4. 如果要写入已有文件，必须有对应文件的编辑权限；只读权限只能截图、读取或分析，不能更新原型。

不必要条件：

- 不要求安装 Figma Desktop 客户端。
- 不要求用户手工在本机下载组件库。
- 不要求把 Figma library 导出成本地文件。

不可自动绕过的情况：

- Figma connector 未安装、未授权或不可用。
- 当前账号没有目标文件编辑权限。
- 当前账号或文件没有团队 library 权限。
- Figma library 已被下架、重命名、取消发布或组件 key 变化。

遇到这些情况时，应明确报告缺口和所需权限；不要改用手绘组件、截图临摹或非团队 library 组件伪装完成。

### 0.3 依赖打包边界

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

### 0.4 强制门禁：禁止用手绘伪装已有组件

以下门禁是交付前必须满足的硬条件。违反时应停止绘制或返工，不要用“截图看起来差不多”的方式继续交付。

#### 0.4.1 先查组件，再设计布局

根因：多数返工不是视觉审美问题，而是没有先确认组件库资产，导致用矩形、文本、圆点、线条、遮罩和 cover 拼出一个不可维护的假组件。

1. 绘制任何控件、表格、分页、状态、弹层、步骤条、图表前，必须先用 `get_libraries` / `search_design_system` / 既有页面实例检查是否有可用组件。
2. 若存在 Arco 或图表库组件，必须导入组件实例并通过 variant / component properties / 实例内部文本层表达内容和状态。
3. 只有在确认目标 library 没有父组件、没有公开子组件、也没有同类可替代组件后，才允许用自建 frame/text 做低保真业务内容承载；此时必须在交付说明里记录资产缺口，不能把它伪装成 Arco 组件。
4. `Button`、`input`、`select`、`search-box`、`radio-button-group`、`switch`、`checkbox`、`table/table-cell`、`pagination`、`Alert`、`modal-confirm`、`modal`、`drawer`、`steps`、`badge/status`、`Tag`、图表库图表等，都属于必须优先搜索和使用的组件，不得手绘替代。
5. 不允许把任何组件实例当底板，再用外部文本、矩形、圆点、线条、图标、遮罩、透明块或 cover 图层覆盖成另一个内容、状态或样式。若组件内容或样式写不进去，应改写组件属性、实例内部文本层、variant、swap 实例、更换组件或记录资产缺口。
6. 这条规则适用于所有组件，不只适用于 `Select`、`Tag`、`Button`、`Input`、`Drawer`、`Modal`。包括但不限于导航、表格、分页、Steps、Alert、Card、Form、Radio、Checkbox、Switch、Badge、Status、Tooltip、Dropdown、DatePicker、Tree、Tabs、Upload、Empty、图表组件和业务组件。

#### 0.4.2 浮层、遮罩和确认态必须使用组件体系

根因：手绘抽屉、弹窗和遮罩经常出现遮罩透明度不对、header/body/footer 不对齐、底部按钮压线、背景上下文残影和交互边界不清。

1. 抽屉必须使用 `drawer` 组件或以 `drawer` 实例为壳；Modal 使用 `modal`，二次确认使用 `modal-confirm`，页面内提示使用 `Alert`，不要用矩形手绘。
2. 遮罩必须来自浮层组件本身，或作为明确命名的 `Overlay / modal mask`、`Drawer mask` 并匹配组件规范；不得随意画半透明矩形模拟。
3. 弹层内必须有清晰 header、body、footer。内容增减后同步调整 body/footer 高度，按钮组不得压住表单、说明或表格。
4. 浮层背景只展示用户可理解的上下文，不展示产品内部逻辑、实现规则或调试说明。

#### 0.4.3 表格状态、Tag 和颜色语义

根因：把流程状态做成 Tag 或手绘圆点，会导致颜色、语义和行高不稳定，表格读起来像补丁。

1. 表格内所有流程状态、执行结果、告警状态、同步状态、任务状态必须使用 `components/table-cell/status` 或 `badge/status`，不得使用 `Tag` 或手绘圆点 + 文本。
2. `Tag` 只用于分类、属性、来源、短标签、筛选条件，不用于表格流程状态。
3. 状态和标签颜色必须和语义一致：成功/已完成=绿色，失败/错误/危险=红色，警告/待关注/跳过=橙色，进行中/处理中=蓝色，默认/未知/未通知=灰色。不得为了视觉统一把所有状态做成同色。
4. 表格状态列、操作列、标签列必须垂直居中，行高由表格组件控制，不得用额外文本或矩形贴在表格上。

#### 0.4.4 表格、分页和列宽不得贴补丁

根因：表格超宽后只改外框或在右侧贴操作列，会让表头、行、操作列和分页不共享同一个内容边界。

1. 表格内容超出页面或卡片宽度时，必须重新压缩所有列宽，并同步调整每个 header/cell/action cell 的 `x` 与 `width`。不能在原表格外侧叠加一列、一个按钮或一个分页作为补丁。
2. 表格列宽必须满足 `sum(columnWidth) <= tableWidth`，且 `tableRight <= contentRight`。操作列不能超出表格右边界。
3. 分页必须使用 `pagination` / `pagination-simple` 组件，不能手绘页码、箭头或总数。
4. 分页放在列表/页面内容区 footer 的右下角，与表格最后一行保持清晰间距。中后台长列表建议预留 24-32px footer 上间距和 24px 右边距；数据不足一页时也不能紧贴最后一行。
5. 顶部操作区、筛选栏、表格、分页必须共享同一 `contentRight`，不能各自贴不同右边界。

#### 0.4.5 图表容器必须先定高度和边距

根因：图表库组件默认结构复杂，直接缩放或裁切后容易盖住标题、筛选控件、图例、tooltip、坐标轴和底部说明。

1. 图表本体必须使用 `源力设计体系-图表库`，不手绘折线、柱、饼、图例、坐标轴、游标或 tooltip。
2. 每个图表卡片必须先定义稳定的 header 区、chart viewport 区和 footer/说明区。图表 viewport 需要固定宽高，不能和标题、筛选、tab、图例、操作按钮共用同一高度。
3. 图表距离容器内边距、标题、筛选、图例和底部说明都必须留出可读间距。不得让图形区域覆盖文字或操作。
4. 如果需要裁切，只能裁切明确命名的 `Chart viewport/*` 或 `Chart crop/*`，并在截图中确认业务图形、标签、图例、数值完整可见。
5. 图表组件的默认样例文案、默认图例和默认系列数必须替换成真实业务语义。

#### 0.4.6 Steps 步骤条必须使用真实组件

根因：手绘步骤条或把标题写进圆点，会破坏 Arco 的流程语义，后续状态修改也不可维护。

1. 步骤条必须使用 `steps` / `components/steps-item` 组件，不得用圆形、线条和文本手绘。
2. 普通数字步骤的圆点内只能显示步骤数字或组件自身图标；主标题和副标题必须在圆点外侧/右侧，不能写进圆点里。
3. 每个 step 的 title 和 description 必须按组件内的文本槽或内部文本层写入，并保持同一基线和同一左对齐规则。
4. 步骤状态使用组件 variant：完成、进行中、等待、错误。不得用颜色覆盖或额外圆点表达状态。

#### 0.4.7 不把产品内部逻辑画进用户原型

根因：把“未开启时隐藏入口”“这里调用接口”“失败后向后继续”等内部规则直接写在画面上，会让用户误以为这是产品可见文案。

1. 原型画面只展示用户会看到、会操作、会理解的信息和状态。
2. 产品内部逻辑、实现规则、接口调用、门禁条件、数据口径说明应沉淀在 PRD、评审说明或隐藏注释区，不应作为页面正文、按钮文案、表格内容或浮层文案展示。
3. 若必须表达规则，应转译成用户语义。例如不要写“未开启聚合时批量处理入口隐藏”，应表现为关闭态列表没有批量入口，或在详情页提供对应操作。

#### 0.4.8 字体必须来自 Arco scale，不硬编码 PingFang

根因：Figma 写入中文文本时如果硬编码 `PingFang SC` / `苹方`，当前运行环境经常因为字体不可用而拦截写入；即使写入成功，也会偏离 Arco scale 的字号、字重和行高规则。

1. 所有新增文本必须优先使用 Arco Design scale / Typography style 中定义的字体、字号、字重和行高，不得自行硬编码 `PingFang SC`、`苹方` 或其它未验证字体。
2. 修改 Arco 组件实例内部文本时，必须优先读取该文本层现有 `fontName` 并加载原字体；不要把组件内部字体替换成另一个手写默认字体。
3. 创建组件外业务文本前，必须先从当前文件已有 Arco 文本样式、相邻 Arco 组件文本层或 `search_design_system(includeStyles=true)` 确认可用字体。确认失败时停止并记录字体/样式缺口，不要盲写 PingFang。
4. 程序化写入前必须执行字体预检：`listAvailableFontsAsync` 或加载目标文本层当前字体；只有字体存在且 `loadFontAsync` 成功后才能写入 `characters`。
5. 需要 fallback 时，只能使用在当前文件/运行环境已验证可用、且与 Arco scale 对齐的字体；fallback 也必须同步字号、字重和行高，不得只换 family。
6. 交付前检查所有新增或修改文本层：不能出现未加载字体、透明字体、不可见字体、字号/行高明显脱离 Arco scale、或因为 fallback 导致文本溢出。

### 0.5 失败模式与交付 QA 门禁

本节沉淀跨项目复用的返工原因。它不依赖具体业务，适用于所有使用 Arco Design System (New) 和图表库输出的 Figma 中后台原型。若命中任一失败模式，必须先修复再交付。

#### 0.5.1 所有组件实例内容必须改组件本身，不得外贴 cover

根因：把真实 Arco 组件当作底板，再叠加 `Select cover`、`Input cover`、`Tag inner cover`、`Button text cover` 等外部文本或矩形，会让画面看似接近，但图层不可维护、属性不可追踪，后续一改状态就出现重影和错位。

1. 使用任何组件实例后，必须优先通过 `componentProperties`、variant、文本 property、instance swap、嵌套实例属性或组件内部文本层修改内容、状态和样式。
2. 如果组件暴露了文本属性，必须使用文本属性写入；如果没有暴露文本属性，进入实例内部查找对应文本层，加载其原字体后修改 `characters`。
3. 如果组件状态需要变化，必须修改组件 variant 或语义属性；不得用外部色块、圆点、Tag、文字颜色、半透明遮罩覆盖组件原状态。
4. 如果实例内部文本、图标、状态或样式不可编辑，或找不到正确属性/文本层，不允许在外部加 cover；应搜索更合适的 Arco 子组件、使用可配置的同类组件，或记录组件库缺口并停止该组件的正式绘制。
5. 对旧页面返修时，必须先删除或隐藏旧的外贴 cover、旧文本、旧状态点、旧色块，再替换为真实组件属性或真实组件实例；不能在 cover 上再覆盖一层新的组件。
6. 交付前用图层名和结构扫描所有正式页面可见节点：`cover`、`inner cover`、`text cover`、`label cover`、`value cover`、`manual tag`、`fake select`、`fake status`、`fake button`、`fake input`、`mask patch`、`overlay patch` 等命名或等价结构若存在，默认判定为未通过，除非它们是组件库实例内部自带且未被用作业务覆盖。
7. 判断标准不是“截图看起来像”，而是“选中图层后仍是可维护的组件实例，并且内容、状态、颜色、尺寸能通过组件属性或内部槽位解释”。凡是依靠外部遮盖才能成立的组件，都必须返工。

#### 0.5.2 表格必须作为整体构造，不能逐列贴补丁

根因：表格返工高发于把表头、行、状态、操作、分页当作独立块拼装，导致行高不一致、状态重影、列边界不齐、分页漂浮和操作列溢出。

1. 表格必须以同一个 table 容器和统一的 column model 构造：表头、每行 cell、状态 cell、操作 cell、展开行、分页共享同一 `x`、`width`、列宽和右边界。
2. 每一行必须使用同一行高。中后台列表默认使用 44px 或 48px 行高；密集表格可用 40px，但同一表格内不得混用。
3. 状态列必须使用 Arco `components/table-cell/status` 或 `badge/status` 实例。不得使用 `Tag`、手绘圆点、文本色、矩形底色来表达流程状态。
4. 分类列可使用 `Tag` 或 `components/table-cell/tag`，但 Tag 必须位于 cell 内部并垂直居中；不要把 Tag 贴在表格上方。
5. 表格行内若出现重影，优先检查是否同时存在旧文本、旧 Tag、旧圆点和新 Status 实例；必须删除或隐藏旧层，而不是调整透明度遮盖。
6. 表格列宽调整时，必须同步更新 header cell、body cell、expanded row、footer 和 pagination；不能只改外框宽度。
7. 可展开表格必须把二级内容放在表格展开区域内，展开内容同样共享表格宽度；不得把二级表格浮在主表格上方或页面外。

#### 0.5.3 Select 和下拉菜单必须使用真实下拉组件

根因：用灰色矩形、文字和 chevron 手拼 Select，会让尺寸、hover、disabled、menu、placeholder、多选和对齐全部失真。

1. 所有枚举选择必须使用 Arco `select`、`treeselect`、`cascader` 或对应 table-cell select 实例；不得用矩形和箭头手绘。
2. Select 展示值必须通过组件文本属性或内部文本层修改，不得在组件上方叠文字。
3. 需要展示打开态时，Select 本体仍使用真实组件，下拉面板使用 `dropdown-menu`、`selection-item` 或 Select 组件自带展开态；选项行高、hover、selected 状态必须来自组件。
4. 多选 Select 必须使用多选属性和真实 chip/tag 区域；不能把多个 Tag 塞进普通 Select 输入框。
5. 表单中的 Select、Input、InputNumber、Textarea 要遵循同一尺寸和基线；常规表单默认中号，紧凑筛选默认小号，同一行不得混用。

#### 0.5.4 图表库组件必须完整承载阅读任务

根因：图表库组件直接缩放、裁切或保留默认样例，会出现坐标轴缺失、tooltip 遮挡、图例与曲线颜色不一致、环图被裁切、标题仍是“我是标题”等问题。

1. 每张图表必须先明确阅读任务：趋势、分布、排行、构成、对比、异常峰值、阈值判断。再选择折线、面积、柱状、Toplist、饼图或环图。
2. 图表卡片结构固定为 header、chart viewport、legend/summary、footer insight。图表本体不得和标题、tab、筛选器、底部说明共用同一个高度。
3. 静态概览态不要展示 hover tooltip、游标线或下载/刷新工具栏；只有在专门表达 hover 交互态的 frame 中才展示 tooltip。
4. 图表必须替换默认样例文本、默认图例、默认系列名、默认数值和“我是标题”等占位内容。
5. 折线图必须展示清晰坐标轴、刻度、图例和系列颜色映射；图例颜色必须与线条颜色一一对应。
6. 构成图必须优先保证完整可见，不得裁切环图/饼图；若空间不足，改用 Toplist 或条形图，而不是缩到不可读。
7. 维度分布若需要排序、定位压力来源或支持切换维度，优先使用 Toplist/条形图；只有 2-5 个固定构成项才使用饼图/环图。
8. 截图 QA 时逐个检查：标题可读、图形完整、坐标轴完整、图例正确、数值完整、无默认文案、无 tooltip 残留、无工具栏误露。

#### 0.5.5 卡片、表单和容器必须随内容收口

根因：组件外层 frame 高度固定、内容变化后不收口，会导致大片空白；相反高度过小会让表格、提示、图表或按钮浮在容器外。

1. 页面主内容、卡片、表单区、图表区、表格区优先使用 Auto Layout；外层 frame 高度应 `Hug contents` 或在写入后按子节点 bottom 重新计算。
2. 固定高度只用于明确的 viewport、图表绘图区、滚动容器或页面尺寸；固定高度区域必须显式命名并设置 overflow/clip 预期。
3. 每个容器必须满足：`max(child.bottom) + bottomPadding <= container.bottom`。不满足时必须调整容器高度或子节点位置。
4. 同一卡片内标题、说明、控件、表格、提示之间必须使用统一 spacing token。常规卡片内边距建议 24px，紧凑卡片 16px；不要出现 0-8px 的意外贴边或 60px 以上的非设计空白。
5. 表单字段同一行必须共享 label baseline、控件高度和垂直中心；跨行字段使用统一行距。
6. 页面交付前必须逐 frame 检查外层 frame 是否刚好包住内容；若底部留下大面积空白或内容浮出容器，视为 QA 未通过。

#### 0.5.6 Drawer / Modal / Confirm 的遮罩和宽度必须符合任务复杂度

根因：手绘浮层常出现遮罩只盖一部分页面、抽屉过窄、header/body/footer 压线、背景页面残影抢焦点。

1. Drawer 必须覆盖在当前页面上下文之上，并有完整页面遮罩；遮罩宽高必须覆盖整个 frame，不得只盖内容区或只盖右侧。
2. Drawer 宽度按任务复杂度选择：简单查看 480px 左右，复杂表单或带表格配置 560-720px；内容超过宽度时改布局或改 Modal，不要压缩到不可读。
3. Drawer 内部必须有 header、body、footer 三段；footer 按钮固定在底部，body 可滚动时必须留出 footer 安全间距。
4. Modal 用于阻断式短任务；Drawer 用于保留上下文的长任务；Confirm 用于风险确认。不要把完整编辑任务塞进 Confirm，也不要把二次确认做成大页面。
5. 浮层内的表单、表格、Alert、按钮仍必须使用 Arco 组件；浮层只是容器，不是手绘豁免区。

#### 0.5.7 选项卡、模式卡和接入方式必须有明确组件语义

根因：把模式选择做成两个普通卡片或按钮，容易出现高度不一、边框状态不清、推荐标记外贴、用户不知道是互斥选择还是普通说明。

1. 互斥模式优先使用 `radio-button-group`、`radio-group` 或 Arco 可选卡片模式；若使用卡片式选项，必须保留 radio/selected 语义。
2. 模式卡高度、内边距、标题基线和说明行高必须一致；选中态通过组件状态或统一边框/背景表达，不得只用外贴蓝框。
3. 推荐语义应转译为用户收益或默认选中状态，不要在卡片里写“默认选项”这类实现文案。
4. 互斥模式对应的后续配置必须拆成不同 frame 或在同一 frame 中只展示当前模式字段；不要让用户同时看到两套互斥配置。

#### 0.5.8 交付前结构化 QA 流程

每次使用本技能输出或修复 Figma 原型，必须执行以下 QA 顺序。不要只看画面局部截图。

1. Library QA：确认 Arco 和图表库均已加载，关键控件来自正确 library。
2. Layer QA：扫描正式页面可见节点，发现 `cover`、手绘 status、手绘 select、手绘 chart、旧文本重影时先修复。
3. Component QA：抽样检查 Button/Input/Select/Status/Tag/Table/Drawer/Chart 是否仍为组件实例，内容是否写入组件本身。
4. Layout QA：逐 frame 检查容器高度、右边界、列宽、内边距、行高、tab/steps 对齐、footer 位置。
5. Semantic QA：状态颜色与语义一致，分类和流程状态分离，按钮优先级清楚，危险动作有确认。
6. Chart QA：图表类型匹配阅读任务，坐标轴/图例/数值完整，默认样例和 tooltip 残留已清理。
7. Screenshot QA：对修改过的每个 frame 生成截图复核。若截图里出现裁切、重影、漂浮、默认文案、工具栏遮挡或组件错位，必须继续修复。


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


### 3.1 LLM 组件决策协议：从产品意图到组件属性状态

本节不是要求死记一张表，而是要求大模型在每次使用 Arco Design System (New) 设计前执行同一条决策路径：先把用户需求拆成可表达的产品意图，再检索组件候选，最后把意图绑定到真实组件、真实属性和真实状态。下面的映射表只是检索和校验材料，不是替代判断的清单。

本节依据当前 Figma 文件已订阅并可检索的两个 library 核验：`Arco Design System (New)` 和 `源力设计体系-图表库`。若用户口头称“原力设计体系图标库”，先用 `get_libraries` 核对当前文件真实订阅名；当前文件中可用的是 `源力设计体系-图表库`，用于数据可视化图表，不是通用 icon button 库。通用图标动作优先使用 Arco 的 `interactive-button/*`、`direction/*`、`tips/*` 等组件。

#### 3.1.1 大模型必须执行的组件决策路径

每一个可见 UI 单元都必须经过以下路径，哪怕最终不向用户展示这份推理。若时间有限，也要在内部按这个顺序判断，不能跳到手绘。

1. 定义用户意图：用户是在执行动作、录入信息、选择条件、阅读列表、识别状态、处理反馈、进入浮层、切换视图、推进流程、查看图表，还是面对空态。
2. 判断信息形态：是单值、多值、互斥、层级、范围、长文本、表格行、流程状态、分类标签、时序数据、构成占比、排行数据，还是确认风险。
3. 判断状态来源：这是交互态、业务态、数据态、权限态、加载态、风险态还是图表 hover/focus 态。业务态不得塞进控件交互态。
4. 查候选组件：先查本节索引、`## 4. 组件资产详解` 和既有页面实例；仍不确定时用 `search_design_system` 精确查组件名，并限定正确 library key。
5. 读取真实属性：如果组件属性不确定，导入 component set 并读取 `componentPropertyDefinitions`；不要猜属性名，不要通过外部覆盖模拟属性。
6. 选择组件和属性：根据产品意图设置组件族、variant、boolean property、instance swap 和文本层。属性必须能回答“这个产品逻辑为什么对应这个状态”。
7. 排除错误替代：明确确认没有把 Button 当导航、Tag 当流程状态、Input 当搜索框、Checkbox 当即时开关、手绘圆点当 Status、手绘矩形当 Modal/Drawer、手绘折线当图表。
8. 布局后复核：检查组件仍是实例，表格 cell/分页/步骤/图表没有脱离容器，没有外贴补丁，没有默认样例文案残留。

#### 3.1.2 内部组件决策卡

执行设计或修复时，为每个复杂区域在内部形成以下决策卡。通常不需要画在原型里，也不需要逐条发给用户；当用户要求 review 或返工时，可以把这些决策卡作为说明依据。

```text
ComponentDecision
- product_intent: 用户要完成/读取/判断什么
- information_shape: 单值/多值/互斥/层级/范围/表格/状态/趋势/占比/确认风险
- state_source: 交互态/业务态/数据态/权限态/风险态/图表交互态
- chosen_component: library + component/component set name
- component_key_or_instance_source: component key / component set key / 当前文件既有实例来源
- required_properties: variant/boolean/text/instance swap
- verified_property_source: componentPropertyDefinitions / instance.componentProperties / 既有实例属性
- semantic_state: 成功/错误/进行中/提醒/默认/危险/警告/选中/禁用等
- rejected_alternatives: 为什么不用手绘/Tag/Button/Input/其他组件
- qa_check: 是否仍为组件实例；是否无溢出、无补丁、无默认样例文案
```

当无法填写 `chosen_component`、`component_key_or_instance_source`、`required_properties` 或 `verified_property_source` 时，不允许继续手绘；必须继续检索组件、读取组件集属性、查找当前文件既有实例，或记录组件库缺口并停止交付。

#### 3.1.3 组件选择硬门禁

1. 组件来源门禁：凡是 Arco 或图表库已有的控件、状态、图表、分页、弹窗、抽屉、步骤、表格 cell，必须使用真实组件实例。
2. 属性真实性门禁：所有状态都必须来自组件属性、variant、内部文本层或组件内 icon swap；不得用外部覆盖物伪造。
3. 语义一致门禁：产品状态先归类为成功、错误、进行中、提醒、默认、危险、警告、分类、交互态，再选择 Status、Tag、Alert、Button、Modal 或图表交互态。
4. 容器边界门禁：表格操作列、状态列、分页、图表 viewport、Modal/Drawer footer 必须属于同一布局边界，不得后贴。
5. 业务文案门禁：原型只展示用户可见语义，不展示“接口调用、隐藏入口、失败后继续、内部判断”等实现逻辑。
6. 缺口处理门禁：检索不到组件时，要说明 library 缺口；不能用“看起来像”的手绘组件完成正式 Arco 原型。

#### 3.1.4 通用属性轴语义

| 属性/状态轴 | 适用组件 | 产品语义 | 使用规则 |
|---|---|---|---|
| `尺寸=大/中/小/迷你` | Button、Input、Select、Pagination、表格 cell 等 | 控件密度和页面层级 | 中后台默认用中；表格/紧凑筛选可用小；不要手动缩放组件 |
| `状态=默认/悬停/聚焦/激活/禁用` | Button、Input 等交互控件 | 用户交互态 | 只表达交互，不表达业务生命周期；业务状态另用 Status/Tag/Alert |
| `类型` | Button、Alert、modal-confirm、图表等 | 组件主形态或信息类型 | 按产品任务选，不按视觉喜好选 |
| `种类=标准/危险/警告/成功` | Button | 动作风险和反馈强度 | 删除、强制覆盖、清空、撤销必须危险；高风险可恢复用警告；成功按钮少用 |
| `填充=true/false` | Input、Select、Tag | 背景强弱和容器适配 | 表单默认用非填充；弱背景或灰底工具区可用填充；不靠填充表达业务状态 |
| `禁用=true/false` | Select、Switch、Pagination、Checkbox 等 | 当前是否可操作 | 禁用不是失败或未开始；原因要通过 Tooltip、Alert 或说明文案补充 |
| `开启=true/false` | Switch | 立即生效的二元开关 | 用于按根因聚合、启停规则、启停同步等即时开关；若需保存后生效，不用 Switch |
| `选中/半选` | Checkbox、表格 checkbox | 多选或父子部分选择 | 半选只表达部分子项被选中，不表达不确定业务态 |
| `多选=true/false` | Select | 单选/多选枚举 | 多个枚举值且需要折叠用 Select 多选；少量平铺选择用 Checkbox Group |
| `数量` / `#of lines` / `分类数量 Item` | Button Group、Radio Button Group、Steps、table-cell/action、图表 | 可见选项数、步骤数、操作数、系列数或分类数 | 必须等于真实业务项数量；不要保留组件默认数量 |
| `布局=水平/垂直` | Radio Group、Steps、Tabs、图表卡片 | 信息排列方向 | 宽页面优先水平；长流程、左侧向导、抽屉内表单可用垂直 |
| `范围=true/false` | Datepicker、Timepicker | 起止时间选择 | 范围时间用组件范围态，不并排手拼两个 Input |
| `显示图标` / `替换图标` | Button、Tag、Alert、interactive-button | 动作或信息类型辅助识别 | 图标只辅助语义，不单独承载业务状态；图标按钮必须配 Tooltip |
| `可关闭=true/false` | Tag、Alert | 可移除筛选或可关闭提示 | 筛选条件 Tag 可关闭；关键风险 Alert 不建议可关闭 |
| `操作按钮=true/false` | Alert | 页面内提示是否带行动 | 只有用户可直接修复/查看时开启；不要把内部逻辑写成按钮 |
| `跳转/展示条目/展示总数` | Pagination | 分页信息完整度 | 主列表用标准分页并展示必要总数；小弹层可用简洁分页 |
| `类型=默认/错误/进行中/成功/提醒` | `badge/status` | 流程/执行/处理状态 | 表格内状态必须走 status，不用 Tag 或手绘圆点 |
| `State/Hover/Focus` | 图表库图表 | 图表交互态 | 只表达 hover/focus，不表达告警处理状态、同步结果或任务状态 |
| `Show Scale` / `Show Legend` / `Show Tooltip` / `Show Threshold` | 图表库图表 | 坐标、图例、tooltip、阈值线是否展示 | 和数据阅读任务一致；阈值/告警线用 Threshold，不手绘横线 |

#### 3.1.5 产品意图参考索引

| 产品逻辑/用户意图 | 必选组件 | 关键属性/状态 | 不允许的做法 |
|---|---|---|---|
| 页面最重要的提交、创建、保存、执行 | `Button` | `类型=主要按钮`，`种类=标准`，尺寸按区域密度 | 用黑色文字当按钮；多个主按钮抢焦点 |
| 次要动作、取消、返回、预览 | `Button` | `类型=次要按钮/线框按钮/文本按钮` | 用主按钮表达低优先级动作 |
| 删除、强制覆盖、清空、撤销、批量关闭 | `Button` + `modal-confirm` | Button `种类=危险`；Confirm `危险=true` | 普通蓝按钮直接执行破坏性动作 |
| 高风险但可恢复动作 | `Button` + `modal-confirm` / `Alert` | Button `种类=警告`；Confirm `警告=true` | 用红色危险表达所有风险，削弱真正删除语义 |
| 表格行内查看、编辑、同步、去处理 | `components/table-cell/action` 或 `components/table-cell/link` | action `数量=1/2/3`；Link 使用蓝色语义 | 外贴黑色文本按钮；操作列脱离表格宽度 |
| 超过 3 个行内动作或次级动作集合 | `dropdown-trigger` + `dropdown-menu` | menu item 分组、危险项独立 | 把 5-6 个 Link 横向铺满表格 |
| 纯图标工具动作：刷新、搜索、设置、历史、下载、上传、同步、更多 | `interactive-button/*` + `Tooltip` | 使用匹配图标组件；Tooltip 写动作名 | 手绘 SVG 或用无解释图标让用户猜 |
| 文本跳转、详情入口、外部链接 | `Link` / `components/table-cell/link` | 默认蓝色；危险 Link 只用于负向动作 | 用普通文本伪装可点击 |
| 单行短文本录入 | `input` | `替换文本`，按密度选尺寸；可用前后缀 | 用矩形 + 文字手绘输入框 |
| 搜索列表、关键字筛选 | `search-box` | 类型按是否有提交按钮选择 | 用 input + 搜索图标手拼 |
| 多行说明、原因、表达式、DSL、脚本 | `textarea` | 需要限制时启用字数统计；可作为表达式编辑低保真承载 | 用多行文本框外贴边框；把代码编辑器画成不可维护假组件 |
| 数字、阈值、持续时间、重试次数 | `InputNumber` | 有单位时用后缀/旁侧单位 | 用普通 Input 或 Slider 表达精确数字 |
| 连续范围、权重、比例调节 | `slider` | mark 只放短刻度；精确值另配 InputNumber | 用 Slider 输入必须精确的阈值 |
| 敏感信息、密钥 | `password` | 使用密码组件 | 普通 Input 加眼睛图标手绘 |
| 可重复标签、tagKV、对象标签条件 | `inputtag` / `Tag` / Form 内 Select+Input 组合 | 标签输入用 inputtag；已选条件用 Tag；结构化 key/value 用 Select + Input | 用纯文本逗号串或自画标签 |
| 插入变量、模板占位符、表达式辅助插入 | `textarea` + `dropdown-menu`/`selection-item` + `Tag` | 变量候选用下拉；已插入变量作为表达式文本或短 Tag 提示 | 手绘变量 chip、把内部变量解析逻辑写进页面 |
| 少量互斥选项（2-4 个） | `radio-group` / `radio-button-group` | Radio Group 用于表单；Radio Button Group 用于强切换 | 用 Select 隐藏少量关键选项 |
| 告警配置里的检测模式：智能告警/静态规则 | `radio-button-group` | `数量=2`，选中当前模式 | 用两个普通按钮或文本块模拟 |
| 智能告警触发源/检测模型/模型版本 | `select` | 单选；选项多时配搜索 | 用测试链接或映射表替代触发源选择 |
| 开关类即时能力：按根因聚合、启用同步、启用通知 | `switch` | `开启=true/false`，旁侧写清楚开关名称 | 用 Radio/Checkbox 表达即时开关；用“聚合/不聚合”口语文案当选项 |
| 多选对象、通知渠道、适用条件复选 | `checkbox` / `checkbox-group` | 需要父子层级才用半选 | 用 Tag 当复选控件 |
| 级联对象：业务域/Region/组件/对象层级 | `cascader` / `tree` / `treeselect` | 路径选择用 Cascader；浏览/批量用 Tree；表单筛选用 TreeSelect | Select 中用缩进文本假装层级 |
| 候选和已选集合迁移：对象/人员/规则批量选择 | `transfer` / `transfer-tree` | 层级对象用 transfer-tree | 小量多选过度使用 Transfer |
| 日期、时间、近 7 天、时间范围 | `datepicker` / `timepicker` | 区间用 `范围=true` | 两个 input 手拼起止时间 |
| 上传文件、导入规则、导入对象清单 | `upload` | trigger + list 表达上传状态 | 只放一个按钮，不展示上传文件状态 |
| 主导航、侧边导航、页面模块切换 | `menu` / `tabs/*` / `Anchor` | 页面级导航用 Menu；同级视图用 Tabs；长页面定位用 Anchor | 用按钮组当导航 |
| 同页同级视图：概览/事件/规则/历史 | `tabs/line` / `tabs/rounded` | 水平布局；当前态来自组件 | 手绘下划线或色块 |
| 流程向导、同步步骤、影响预览进度 | `steps` / `steps-dot` / `steps-arrow` | `数量` 等于真实步骤；title/description 在圆点外；状态用组件 variant | 圆点里写主标题；线条和圆形手绘步骤条 |
| 页面内规则说明、风险提醒、同步失败摘要 | `Alert` | `类型=信息/成功/警告/错误`；必要时 `操作按钮=true` | 把内部逻辑或调试说明画成正文 |
| 操作完成后的短反馈 | `Message` | 类型对应成功/错误/信息 | 用固定 Alert 表达短暂 toast |
| 二次确认、删除确认、强制覆盖确认 | `modal-confirm` | `信息/成功/警告/危险` 仅一个为 true | 自画弹窗和遮罩 |
| 短任务、少量表单确认 | `modal` | header/body/footer 完整 | 用普通 Frame 伪装 Modal |
| 复杂编辑、通知策略编辑、高级接收人、适用对象条件 | `drawer` | Drawer 自带遮罩；内部用 Form/Table/Steps | 手绘抽屉遮罩；把抽屉做成独立页面 |
| 悬浮解释、禁用原因、超长文本完整内容 | `Tooltip` | 位置按触发点选择，迷你态用于紧凑区域 | 用页面正文解释每个图标 |
| 无数据、无权限、无匹配 | `empty` / `空状态` | 文案写用户可理解原因和下一步 | 空白区域或手绘插画 |
| 核心数值、指标概览 | `statistic` / `components/number` / `components/percentage` / `components/trend` | 数值、百分比、趋势拆开表达 | 用普通大字号文本堆数值 |
| 表格普通文本 | `components/table-cell/text` | 尺寸跟 table 一致 | 外贴文本，导致行高不一致 |
| 表格对象名/详情入口 | `components/table-cell/link` | 蓝色链接语义 | 黑色操作文字或普通 Text |
| 表格状态：告警状态、任务状态、同步结果、P0 未认领、删除结果 | `components/table-cell/status` / `badge/status` | status 类型：默认/错误/进行中/成功/提醒；颜色按语义 | 用 Tag 或手绘圆点表达状态 |
| 表格分类：业务域、告警源、模板类型、对象等级、标签 | `components/table-cell/tag` / `Tag` | Tag 颜色按分类语义，文本短 | 用 Status 表达纯分类，或 Tag 表达流程状态 |
| 表格行选择、批量处理 | `components/table-cell/checkbox` + 表头 checkbox | 半选用于部分选择 | 外贴 checkbox 或圆点 |
| 表格行内开关 | `components/table-cell/switch` | 表格 cell 内保持统一行高 | 普通 switch 外贴在表格上 |
| 表格分页 | `pagination` / `pagination-simple` | 主列表用标准分页；右下角留 24-32px 间距 | 手绘分页或紧贴表格最后一行 |

#### 3.1.6 状态颜色语义参考

| 业务语义 | 首选组件 | 组件属性/颜色语义 | 示例 |
|---|---|---|---|
| 成功、已完成、已解决、已同步、健康 | `badge/status` / `components/table-cell/status` | `类型=成功`，绿色 | 已解决、同步成功、任务已完成 |
| 失败、错误、危险、超时、删除失败 | `badge/status` / `Alert` / `modal-confirm` | `类型=错误` 或 Confirm `危险=true`，红色 | 同步失败、SLA 超时、删除失败 |
| 进行中、处理中、同步中、已认领处理中 | `badge/status` | `类型=进行中`，蓝色 | 处理中、同步中、执行中 |
| 提醒、警告、待关注、跳过、风险确认 | `badge/status` / `Alert` | `类型=提醒` 或 Alert `类型=警告`，橙色 | 独立态跳过、待确认、规则风险 |
| 默认、未知、未开始、未通知、草稿 | `badge/status` | `类型=默认`，灰色 | 未认领、未开始、草稿 |
| 分类、来源、等级、业务域、标签 | `Tag` / `components/table-cell/tag` | Tag 色彩按语义选择，文本短 | P0、Argos、Engine、CN-抖音推荐 |

注意：当这些语义出现在表格状态列时，优先用 `components/table-cell/status`。当 P0/P1/P2 在业务上是“告警等级”且位于表格等级列，可以用 Tag；如果设计目标是圆点+文字状态读法，则必须用 status cell，并按等级色彩调整内部状态语义。

#### 3.1.7 图表阅读任务参考

| 数据阅读任务 | 必选图表组件 | 关键属性 | 不允许的做法 |
|---|---|---|---|
| 近 7 天/近 24 小时告警数量趋势、响应趋势、解决趋势 | `折线图` 或 `折线组合/dataLinesCombo` | `数量 #of lines`/`线数量` 等于真实系列；需要阈值线时 `Show Threshold=true` | 用曲线随意手绘；用饼图表达时间变化 |
| 同一时间维度多指标对比，如告警数/误报数/SLA 达成率 | `折线组合/dataLinesCombo` | `对比 Compare=True`；必要时 `预测 Projection=True` | 多张小图无必要拆分，或保留默认 P90/P95 文案 |
| 趋势背后的规模/累计感，如事件量面积变化 | `AreaChart 面积图` | `数量 #of lines`、`布局 layout`、`Show Legend` | 用柱状图表达连续趋势量感 |
| 离散类别/周期对比，如各告警源事件数、各业务域事件数 | `BarChart 柱状图` | `类型 type=基础/分组柱/堆叠/百分比堆叠`；`状态 state` 只表达 hover/focus | 手绘柱子、坐标轴或图例 |
| TopN 排名和维度分布，如业务域 Top、对象 Top、规则 Top | `Toplist 条形图` | `数量 #of lines` 对齐系列；标签和数值必须完整 | 条形图被容器裁掉；用普通进度条拼排行 |
| 少量固定分类占比，如告警等级占比、来源占比 | `Component/PieChart` / `Mini_pie_chart` | `类型 Type=饼图/环形图`；`分类数量 Item` 等于分类数 | 用柱状图表达纯构成，或手绘饼图 |
| 带标题、图例和统计信息的完整占比卡片 | `Card/PieChart` | `布局 Layout`、`数据展示 Statistic`、`总数值 Sum` | 用 Arco Card 内手绘扇区和图例 |

图表组件的 `State/Hover/Focus` 只表达图表交互态，不表达告警事件状态。告警事件状态仍使用 `badge/status`、表格 status cell、Alert 或详情文案。

#### 3.1.8 已核验组件族索引

| Library | 组件族 | 已核验属性/状态轴 | 适用产品表达 |
|---|---|---|---|
| Arco | `Button` | `类型`、`种类`、`形状`、`尺寸`、`状态`、`显示图标`、图标 swap | 所有可点击动作和操作优先级 |
| Arco | `input` / `textarea` / `password` / `InputNumber` / `search-box` / `inputtag` / `input-group` / `Mentions` | 尺寸、状态、填充、前后缀、字数、标签输入、提及 | 文本、表达式、数字、搜索、标签、敏感信息输入 |
| Arco | `select` / `selection-item` / `treeselect` / `cascader` / `tree` | 尺寸、填充、多选、悬停、聚焦、禁用、层级节点 | 枚举、层级、对象路径和下拉选择 |
| Arco | `radio-group` / `radio-button-group` / `checkbox` / `checkbox-group` / `switch` | 布局、数量、选中、半选、开启、禁用、滑轨内容 | 单选、多选、即时开关、模式切换 |
| Arco | `datepicker` / `timepicker` | 尺寸、范围、填充、悬停、禁用 | 单点时间和时间范围 |
| Arco | `upload` / `transfer` / `transfer-tree` | 上传 trigger/list，候选/已选迁移 | 文件导入、对象/人员/规则批量迁移 |
| Arco | `table` + `components/table-cell/*` + `components/table-column/*` | header/text/link/status/tag/action/checkbox/switch/sort/expand | 中后台高密列表、状态、操作、批量选择 |
| Arco | `pagination` / `pagination-simple` | 尺寸、禁用、跳转、展示条目、展示总数 | 列表分页和浮层分页 |
| Arco | `Tag` / `badge/status` / `Alert` / `Message` | Tag 色彩/填充/可关闭；Status 类型；Alert 类型/标题/操作/关闭 | 分类标签、状态点、页面提示、短反馈 |
| Arco | `modal-confirm` / `modal` / `drawer` / `Tooltip` / `dropdown-menu` / `dropdown-trigger` | 信息/成功/警告/危险、抽屉壳、Tooltip 位置、菜单项 | 确认、编辑、复杂配置、解释、动作收纳 |
| Arco | `tabs/*` / `menu` / `Anchor` / `steps/*` | Tabs 布局/图标/滚动；Menu 层级；Steps 布局/数量/描述/连接线 | 同级视图、页面导航、锚点、流程进度 |
| Arco | `interactive-button/*` / `direction/*` / `tips/*` | 图标 action、方向、提示语义 | 紧凑工具动作、展开/收起、提示图标 |
| 源力图表库 | `折线图` / `折线组合/dataLinesCombo` | 系列数量、布局、图例、阈值、tooltip、对比、预测 | 时序趋势、多指标对比、阈值分析 |
| 源力图表库 | `AreaChart 面积图` | 系列数量、布局、图例、坐标 | 趋势规模和累计感 |
| 源力图表库 | `BarChart 柱状图` / `Toplist 条形图` | 类型、系列数量、适配方式、图表交互态 | 分类对比、排行、分布 |
| 源力图表库 | `Component/PieChart` / `Card/PieChart` / `Mini_pie_chart` | 饼/环形、分类数量、统计、总数值、色彩模式、hover | 构成占比和占比卡片 |


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
- 分页属于列表/页面内容区 footer，不属于表格最后一行；必须使用 `pagination` / `pagination-simple` 组件，并右对齐到统一的 `contentRight`。
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
- 非强流程状态可用 Tag；表格内流程状态、执行结果、告警状态、同步状态、任务状态必须使用 `components/table-cell/status` 或 `badge/status`，不得用 Tag。
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

- Steps 只表达有顺序的流程、向导、进度；圆点/图标区域只放数字或组件自身图标，标题和副标题必须写在圆点外侧的文本槽里。
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
- 表格下方分页必须放在列表/页面内容区 footer 的右下角，和最后一行保持清晰间距，并右对齐到统一的 `contentRight`。

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
- 图表容器必须先划分 header、chart viewport、footer/说明区，并给出稳定高度和宽度；图例、tooltip、坐标刻度、标题、筛选控件不能压到图形区域，也不能与 Arco Card header/footer 重叠。
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
- 表格必须同时满足三层宽度关系：`sum(columnWidth) <= tableWidth`、`tableWidth + cardPadding * 2 <= cardWidth`、`tableRight <= contentRight`。如果内容超宽，必须压缩各列并同步检查所有直接列 cell/header/action cell 的 `x + width` 是否仍在 table 内；不得在表格外侧叠加补丁列或补丁按钮。
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

### 5.10 返工复盘经验：从局部修补到结构修复

这些规则来自原型返工中反复出现的通用问题，不绑定具体业务。遇到用户指出“难看、看不懂、组件不真实、容器包不住、字体不对、图表不对、浮层不对”时，先按本节复盘，不要只修截图红框。

#### 5.10.1 不只修红框，先收口整页结构

红框通常只是问题显性的地方，根因可能在父容器、相邻区块、全页 frame、隐藏旧图层或同一区域的双文本源。

- 修复前先重新定义本页的 `contentLeft`、`contentRight`、栅格、卡片宽高、底部留白和操作区边界。
- 修复一个局部后，必须检查相邻区块、后续区块 y 坐标、页面 frame 高度和右侧留白，不能只移动红框内的节点。
- 不允许用大块矩形、遮罩、白底 cover 或低透明度图层盖住旧内容来制造“截图正确”。旧层应直接改对、隐藏到参考区或删除。
- 不得在最终页面保留可见或隐藏的 `cover`、`fake`、`overlay`、`patch`、`backup` 等维护风险图层，除非它们是明确的组件 mask 或图表 viewport。

#### 5.10.2 字体是前置门禁，不是截图后再修

字体不一致会放大所有排版问题：文本溢出、按钮重叠、表格行高错位、步骤条数字错位和图表标签裁切。

- 开始写入前，先确认当前文件可用 Arco 字体和中文 fallback；新增文本必须遵守 Arco Typography scale。
- 批量写入后，必须做整页字体审计：`fontName`、字号、字重、行高、`textAutoResize` 和文本框宽高都要检查。
- 如果某个组件实例内部原字体可加载，优先沿用组件原字体；不要把所有文本强行替换为一个手写默认字体。
- 发现字体不对时，应回到样式和组件层修复，不能通过缩小字号、压缩文本框或裁切来掩盖。

#### 5.10.3 图表组件先选阅读任务，再处理容器

图表返工的根因通常是没有先定义用户要读什么，或把完整图表卡片塞进已有卡片导致标题、tab、tooltip、图例和样例文案互相覆盖。

- 先按阅读任务选图表：趋势用折线/面积，离散对比用柱状，排行用 Toplist，构成用饼图/环图；不要用同一种图表硬套所有维度。
- 若页面已有 Arco Card，只能在其 `Chart viewport/*` 中放图表本体；不要把带完整 header 的图表卡片再嵌进另一个卡片。
- 图表容器必须完整包住标题、筛选、tab、图例、图形主体、行标签、数值和底部结论；排行图尤其要检查最右侧数值和最底部行。
- 必须替换或移除图表库默认样例信息，例如 `我是标题`、`报错总数`、`Item`、`P90/P95`、`to.service` 等。
- 曲线图不能手绘斜线和散点；如果组件无法表达目标趋势，应换合适的图表组件或记录图表库缺口。

#### 5.10.4 流程图和依赖图要有设计语法

流程、依赖、配置关系不是自由连线。画得直观的关键是先定义信息语法，再画节点和连线。

- 复杂流程优先使用泳道、阶段行、列式步骤或分层依赖图；不要把所有节点散落在自由画布上。
- 节点卡片必须稳定展示核心信息：节点名称、状态、类型、负责人/角色、必要进度或风险摘要。信息放不下时用详情抽屉，不要把所有参数塞进卡片。
- 连接线必须在节点下层，不能穿过标题、状态、按钮或说明文本；硬依赖、软依赖、可选节点、关键路径要用清晰图例区分。
- 选中、失败、阻塞、进行中、完成、未开始、可选/未选等状态必须有不同视觉语义，且与 Arco 状态色一致。
- 对象创建、模板选择、模板实例化或规则配置时，如果对象之间存在依赖关系，必须展示依赖预览和影响范围；不能只用平铺 Checkbox 或普通列表。

#### 5.10.5 配置页不要把所有参数挤在主编排页

主编排页负责结构和关系，节点参数页负责具体配置。把二者混在一起会导致画面拥挤、依赖关系看不清、表单也不完整。

- 流程/模板编排页只展示阶段、节点、依赖、关键路径和节点入口。
- 节点配置必须进入单独 Drawer 或 Modal；不同节点类型使用不同表单参数，不得共用一套看似通用但无法落地的字段。
- 人工确认节点重点配置责任人、协同人、完成标准、证据要求、审批/确认方式、ETA/SLA、提醒和升级策略。
- 自动化节点重点配置执行方式、脚本/API、输入变量、环境、鉴权、超时、重试、失败处理、回写结果和执行日志。
- 模板选择或实例化流程中，必须同时展示节点选择状态和依赖影响：必选节点不可取消，可选节点取消后要说明影响范围和后续可补录方式。

#### 5.10.6 浮层、抽屉和确认态要完整表达上下文

浮层不是把一个面板放到右侧或中间。用户需要看到当前上下文、遮罩范围、标题、主体、底部动作和关闭方式。

- Drawer 必须有完整遮罩、右侧面板、header、body、footer 和关闭入口；面板宽度、遮罩透明度、底部按钮位置要符合组件规范。
- Modal/Confirm 必须使用真实组件，按钮不能重叠，主次动作顺序清晰，危险动作使用危险按钮。
- 浮层 body 增加内容后，footer 必须同步下移或固定在底部；不得让表单、上传项、Alert 或按钮漂出容器。
- 独立展示浮层状态时，应单独建 frame 展示触发后的页面状态，而不是在同一个主页面里用半透明层临时拼接。

#### 5.10.7 新建、编辑、选择类流程必须表达依赖和影响

向导页不能只画输入框。用户在做选择、反选、保存或发布时，最需要知道“选了什么、依赖谁、取消会影响什么、下一步会生成什么”。

- Step 只表达流程进度，数字只放数字或组件图标，标题放在圆点外；不要把步骤文案写进 marker。
- 节点选择页应优先使用依赖预览、阶段泳道和右侧影响面板；平铺 Checkbox 只能作为辅助摘要。
- 填写页必须区分用户输入、系统继承、自动生成和只读结果；不要用同一灰底输入框表达所有字段。
- 确认页必须展示最终将生成/修改的对象、被跳过的可选项、风险提示、责任人和下一步动作。

#### 5.10.8 交付前必须做双重审计

截图只能发现视觉问题，Figma API 才能发现结构和隐藏图层问题。正式交付前两者都要做。

- 截图 QA 覆盖：列表页、详情页、表单页、向导页、图表页、抽屉页、弹窗页、空态/禁用态。
- 程序化审计覆盖：字体、组件实例、双文本源、隐藏 cover、容器溢出、表格列宽、图表样例文案、frame 高度、按钮重叠。
- 审计必须同时扫描可见和 `visible=false` 图层；隐藏的伪装层会误导后续维护。
- 若用户指出视觉质量问题，下一轮修复前先重新跑审计清单，不要在原错误结构上继续叠补丁。

### 5.11 复杂中后台原型返工经验

本节沉淀复杂中后台原型中高频返工问题。它适用于工作台、对象列表、创建向导、详情页、流程编排、模板编辑、异常处理、数据分析、系统配置等多模块平台页面，不绑定具体业务域。

#### 5.11.1 先用验收问题组织页面，不按功能名堆页面

复杂原型不能只是页面集合。每个页面必须回答一个明确的用户验收问题，页面存在的理由要能被一句话说清楚。

- 绘制前先列出关键验收问题，并映射到页面或浮层。若一个页面回答不了任何问题，应合并或删除。
- 同一个流程步骤不能出现多个内容和样式不同的页面。若确实需要多个状态，必须命名为同一任务的不同状态，例如“预览态、选择态、有限编辑态”，并在画面中体现差异。
- 向导步骤数量应只表达用户心智中的主流程，不把系统内部校验、推荐计算、依赖重算、有限编辑等内部阶段拆成过多 step。
- 新增页面前优先检查现有 frame 是否能承载该状态；除非用户明确要求，不要新建“重组页”“新版本页”来绕开旧页面问题。
- 页面标题、副标题和内容必须一致；不能标题表达一个任务，主体却仍是普通表单或另一个步骤。

#### 5.11.2 顶部操作和主按钮必须服从页面职责

按钮不是越多越完整。复杂平台页面尤其要避免全局复制同一组操作，导致每页都有无关主按钮。

- 顶部按钮只放当前页面真正拥有的全局动作。创建类主入口只应出现在对象列表或明确的创建入口页；不要复制到工作台、详情、异常处理、分析、系统配置等无关页面。
- 导出、生成报表、发布等全局动作只出现在具备对应语义和数据范围的页面；普通详情、编辑器、系统配置不应默认出现。
- 每个页面区域默认只允许一个主按钮。若同一区域出现两个以上蓝色主按钮，必须重新定义主次：主动作用 primary，次动作用 secondary/outline/text。
- 筛选按钮属于筛选区动作，不能和页面主 CTA 抢焦点；表格行内操作只能使用表格 action/link 语义，不要把所有操作按钮都改成蓝底主按钮。
- 操作按钮靠右时，要靠内容区右边界，不靠画板右边界；按钮组与标题区、筛选区、表格区要共享同一个 contentRight。

#### 5.11.3 右侧空白和容器对齐必须一次性全局扫描

右侧大空白通常不是单个红框问题，而是页面栅格、内容宽度、父容器和 frame 收口没有统一。

- 批量绘制或返修后，必须一次性扫描所有 top-level frame：页面内容区右边界、主要容器右边界、右侧栏右边界和画板右边界的差值。
- 中后台 1440 画板常用内容区应在侧边导航后保持 24-40px 左右页边距；除非是刻意留给浮层或批注，否则右侧连续空白不应超过 64-96px。
- 两栏布局要明确比例。常规详情页建议主区 60-70%、右侧栏 30-40%；右侧栏内容少时应收窄右栏并扩展主区，不能保留一整列空白。
- 卡片、表格、筛选栏、图表和右侧面板必须共享同一栅格；不要出现上方卡片满宽、下方内容缩进、右栏漂浮的错位。
- 修复任一页面右侧空白后，必须复查同一模块所有相邻页面，避免“用户指出一页才修一页”。

#### 5.11.4 表格必须符合 Arco 表格视觉和结构

表格问题往往会被误判成文字或颜色问题，实际是没有按表格组件边界组织列。

- 表头必须使用 Arco 表格头部灰底语义；不要用白底或业务色表头。
- 状态列必须使用 Status/Badge 的语义色：成功绿、失败红、待处理/风险橙、进行中蓝、未知灰。不要让状态列全部灰色，也不要只用普通文本。
- 操作列必须是表格列的一部分，与表头、行、边框和 hover 区域共享同一列宽；不能像贴在表格右侧的独立文本块。
- 表格中的操作列使用白底蓝字 link/action；这条规则只适用于表格操作列，不允许把页面所有按钮都改成蓝色。
- 表格上方的批量操作区不是表格行内操作。批量主动作最多一个，次动作使用 secondary/outline，并与表格右边界对齐。

#### 5.11.5 选择态和运行态不能混用颜色语义

对象选择、模板选择、依赖预览属于配置态，不是执行态。不要用红绿橙表达还没有发生的失败、完成或风险。

- 选择页只用蓝色表示已选，灰色表示未选/可选，禁用 checkbox 表示必选或不可取消。
- 必选项不需要额外大面积文字强调；使用 disabled checked checkbox 和轻量说明即可。
- 可取消项使用 enabled checkbox；取消后在影响面板展示计划、关键路径、风险、成本、范围或后续补录影响。
- 只有对象进入运行、执行、发布或处理态后，才使用成功绿、失败红、等待橙、进行中蓝。
- 同一个依赖图组件在“预览/选择/运行”不同页面必须切换语义：预览看结构，选择看可选性，运行看状态和阻塞。

#### 5.11.6 流程图和依赖线必须可解释，不追求装饰性

流程图、依赖图或 DAG 的目标是让用户看懂关系，不是把所有关系画出来。依赖线乱、箭头错位、颜色太多时，应减少信息而不是继续加线。

- 先确定阶段泳道和节点列，再画线。节点位置要让主要依赖尽量水平或垂直，减少斜线和交叉。
- 连接线必须从节点边缘锚点出发，到目标节点边缘结束；箭头不能落在节点内部、文字上、状态 Tag 上或空白处。
- 关键路径只用一种主色突出；硬依赖用中性色实线；软依赖/可选依赖用灰色虚线。不要同时用红、绿、橙、紫等多色表达结构关系。
- Tooltip 或边详情只解释当前选中的边，不要让多个浮层同时出现。
- 如果依赖过多，优先提供“只看关键路径”“按阶段折叠”“选中节点上下游”视图，而不是在一张图里展示全部边。

#### 5.11.7 Alert、Message 和异常提示必须按反馈强度选择

反馈组件颜色不能为了醒目随意改。错误的蓝色 Message 会让阻断风险看起来像普通信息。

- `Message` 用于短暂操作反馈，不用于承载大段页面规则或启动阻断说明。
- 页面内持续可见的校验结果、异常、依赖影响使用 `Alert`；信息=蓝，成功=绿，警告=橙，错误=红。
- “存在待确认项，确认后可继续”属于警告，不应使用蓝底信息提示。
- “校验失败，阻止继续”属于错误或警告，具体取决于是否完全阻断；不能用 primary 蓝底按钮样式伪装提示。
- 异常提示要说明对象、原因、影响、可执行动作和关闭条件，不能只写“高风险”或“异常”。

#### 5.11.8 产品语义要落到界面，不把设计说明暴露给用户

分析页、配置页、异常处理页等页面常见问题是把 PRD 里的“管理问题、核心图表、下钻去向、产品逻辑”直接画成用户文案。

- PRD 语义应转译成可操作界面：筛选条件、指标卡、图表、排行、明细表、下钻入口、配置项和审计日志。
- 不要在页面上直接写“管理问题”“核心图表”“下钻去向”“产品逻辑上需要体现”等设计说明。
- 分析页必须能回答“发现了什么、影响谁、去哪里处理”；至少包含筛选、指标、趋势/排行、下钻结果和动作。
- 异常处理页必须能回答“是什么异常、为什么产生、影响链是什么、现在该怎么办、如何关闭、关闭证据是什么”。
- 系统配置页必须用角色权限、通知/集成/凭据、审计日志等真实配置或记录承载语义，不用说明文字替代功能。

#### 5.11.9 抽屉宽度、遮罩和内容密度要按任务校验

抽屉过窄、双层遮罩和内容挤压会直接破坏可读性。

- 复杂配置抽屉宽度建议 520-640px；简单详情抽屉不低于 480px。若内容包含表格、长表单或多区块，优先 600px 以上。
- 同一浮层状态只能有一层遮罩；不得出现背景被两层不同透明度遮罩覆盖。
- 抽屉 body 内部要有清晰 section，不要把表单、表格、Alert、说明文字直接堆满。
- 抽屉 footer 必须固定在底部或跟随内容自然收口，按钮右对齐，主次顺序清晰。
- 抽屉截图 QA 要检查：面板宽度、遮罩范围、header/body/footer、关闭入口、底部按钮、内容是否被压缩或裁切。

#### 5.11.10 返修必须按模块批量修，不按截图红框逐点补丁

当用户指出同类问题多次出现时，说明规则没有沉淀到全局。

- 一旦出现“右侧空白”“操作列贴边”“主按钮过多”“颜色语义错”“同一步骤多页面重复”等问题，必须把同类规则应用到全部 frame。
- 修一页表格后，应扫描所有表格；修一页右侧空白后，应扫描所有页面 frame；修一个流程图或依赖图后，应扫描所有关系图、节点图和连线视图。
- 每次返修后输出一份简短 QA 结果：扫描范围、命中的页面、修复的类别、仍需人工确认的例外。
- 不要把局部截图反馈理解成只改红框内元素；红框通常代表一类系统性问题。

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
14a. 表格内流程状态、执行结果、告警状态、同步状态、任务状态是否全部使用 `components/table-cell/status` 或 `badge/status`，没有使用 Tag、普通文本或手绘圆点？
14b. Status 和 Tag 的颜色是否与语义一致：成功=绿、失败=红、警告/跳过=橙、进行中=蓝、默认/未知=灰？
15. 危险动作是否使用危险语义和确认框？
16. 图标按钮是否提供 Tooltip？
17. Steps marker 是否只包含数字或组件自身图标，主标题和副标题是否在圆点外侧且对齐？
18. Switch 是否只用于立即生效二元开关？
19. Modal/Drawer/Confirm 是否有 header/body/footer 清晰结构？
20. 文本是否写入内容层且不溢出？
20a. 新增/修改文本是否使用 Arco scale / Typography style / 组件原字体，没有硬编码 `PingFang SC`、`苹方` 或其它未验证字体？
20b. 字体写入前是否已通过 `loadFontAsync` 成功加载，字号、字重和行高是否与 Arco scale 一致？
21. 是否避免手绘已有组件状态、边框、分割线和图标？
22. 是否在最终交付前检查组件实例仍保留为 instance？
23. Select/Input/Button 等控件内容是否写入组件 `TEXT` property 或实例内部文本层，而不是外部覆盖层？
24. 画布中是否不存在可见或隐藏的 `Select cover`、`Input cover`、`Button text cover`、`Text / ⌄`、`fake-*`、`overlay-*` 等伪装控件层？
25. 表单 label/control 是否在同一栅格内，且没有 label 与控件重叠？
26. 表格列宽总和是否严格小于容器内容宽度？
27. Pagination 是否使用真实 `pagination` / `pagination-simple` 组件，位于列表/页面内容区 footer 右下角，和最后一行保持间距，并与表格右边界/contentRight 对齐？
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
44. 图表库组件是否有足够 viewport，趋势线、条形、图例、行标签、数值和 tab 是否完整可见，并且没有覆盖标题、筛选、操作按钮或底部说明？
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
55. 用户可见原型是否没有出现内部逻辑、实现规则、接口调用、门禁条件或调试说明文案？
56. 是否没有因为字体 fallback 导致按钮、表格 cell、Tag/Status、图表标签或弹层标题文本溢出？
57. 是否没有只修局部红框，而是重新检查了整页 frame、相邻区块、隐藏旧层和父容器边界？
58. 流程图/依赖图是否有清晰语法：泳道或阶段、节点状态、依赖类型、关键路径、图例和不穿透文字的连接线？
59. 模板选择、对象创建、节点/步骤选择或类似向导是否展示了依赖预览和取消影响，而不是只有平铺 Checkbox？
60. 节点配置、规则配置或参数配置是否放在单独 Drawer/Modal 中，并按类型展示不同参数，而不是挤在主编排页？
61. 图表卡片是否按阅读任务选择图表类型，且没有把完整图表卡片嵌入另一个卡片造成 header/tab/tooltip/图例重叠？
62. 独立展示浮层状态时，是否有完整遮罩、面板、header/body/footer、关闭入口和按钮组，没有用临时矩形拼接？
63. 是否没有使用白底 cover、遮罩或裁切来隐藏旧内容、错误布局、默认文案或重叠按钮？
64. 每个页面是否能回答明确的验收问题，且不存在同一步骤下内容和样式都不同的重复页面？
65. 顶部操作是否只保留当前页面真实拥有的全局动作，没有把创建、导出、发布、生成报表等动作复制到所有页面？
66. 每个区域是否最多一个主按钮，筛选、批量操作、表格行内操作是否已区分主次和组件语义？
67. 是否已全量扫描所有 frame 的右侧空白、内容右边界和容器对齐，而不是只修用户截图红框里的页面？
68. 表格表头是否为灰底，状态列是否有语义色，操作列是否在表格列宽内且只使用白底蓝字 link/action？
69. 对象选择、模板选择、依赖预览等配置态是否只用蓝/灰/禁用表达选择关系，没有混用运行态红绿橙？
70. 流程图/依赖图/DAG 是否使用阶段泳道、稳定节点列、少量颜色、清晰图例和准确锚点，连接线没有穿过文字或状态组件？
71. Alert/Message 颜色是否符合反馈强度：持续异常或阻断说明用 Alert，阻断/警告不用蓝色信息提示伪装？
72. 分析、异常处理、系统配置等页面是否把 PRD 语义转译成筛选、指标、图表、明细、配置、审计和动作，而不是展示设计说明文字？
73. Drawer 是否只有一层遮罩，宽度匹配任务复杂度，且 header/body/footer/关闭入口/底部按钮完整？
74. 若用户指出一类问题，是否已对同类页面和组件做批量审计并修复，而不是只改单个截图区域？
75. 是否已为本次需求建立组件选型矩阵，并覆盖所有可见界面元素？
76. 组件选型矩阵中每一项是否都有 `最终组件`、`已核对属性` 和 `证据方式`？
77. 是否没有因为“没找到组件”而退化成手绘模拟；找不到时是否继续搜索组件库、组件属性和既有实例？
78. 是否只在不替代组件语义的布局承载区使用基础 Frame/Line，而没有用基础图层模拟控件、表格、状态、反馈或浮层？

## 7. Figma API 实施备注

- 使用本技能前必须先通过 Figma 工具自动确认 required libraries 可用；若当前 Figma 工具提供 `get_libraries`，先检查 `Arco Design System (New)` 和 `源力设计体系-图表库` 是否在可用库列表中。
- 若使用 `use_figma` 做预加载或导入，必须先加载 `figma-use` 技能，并在工具参数中传 `skillNames: "figma-use"`。
- 搜索组件：`search_design_system({ fileKey, query, includeLibraryKeys: [library_key] })`。
- 搜索 Arco 组件时使用 Arco library key；搜索图表组件时使用 `源力设计体系-图表库` library key，避免跨库同名组件混入。
- 绘制前必须把搜索结果整理成组件选型矩阵；每个需求元素都要有最终组件、组件来源和已核对属性。
- 对每个最终组件，必须读取 `componentPropertyDefinitions` 或实例 `componentProperties` 后再使用；不要只凭组件名称或截图判断组件可用。
- 若搜索不到目标组件，先扩大同义词、组件族、当前文件既有实例和组件集属性检索范围；仍找不到时停止并报告组件资产缺口，不进入手绘。
- 如果任一 required library 无法加载或搜索不到目标组件，停止绘制该部分并报告依赖缺口；不要用手绘控件、默认 Figma shape、外部文本 cover 或非目标 library 组件替代。
- 预加载应采用“探测 + 懒导入”：先搜索并验证 library/component key，再按当前页面实际需要导入组件；不要导入整库，也不要在画布保留探测节点。
- 导入 component set：`figma.importComponentSetByKeyAsync(componentKey)`。
- 导入单组件：`figma.importComponentByKeyAsync(componentKey)`。
- 创建实例后先读取 `instance.componentProperties`，再设置属性。
- 设置属性时优先用可读属性名，例如 `类型`、`尺寸`、`状态`。
- 控件文案写入必须遵守 `setProperties(TEXT property) -> 修改实例内部 TEXT 子层 -> 更换组件/报告资产缺口` 的顺序；禁止用 `cover`、外部文本或假箭头伪造组件内容。
- 对 Select/Input/Button 等控件，写入后必须读取并校验最终可见文本层：`characters` 是目标文案，`fontName` 来自 Arco scale 或组件原字体，`visible !== false`，`opacity > 0`，`fills` 非透明，`absoluteBoundingBox` 位于组件实例内且未被父级裁切。
- 展示下拉展开态时，不要在 select 上覆盖一个自制菜单；应导入 `dropdown-menu`、`selection-item` 或对应公开下拉组件，并用 prototype/命名表达触发关系。
- 对未知组件，不猜 variant 值；先读取 `componentPropertyDefinitions` 或实例 `componentProperties`。
- 对中文文本，必须先从 Arco scale / 当前文本层 / 已有 Arco 组件文本样式确认字体，再 `loadFontAsync` 写入；禁止硬编码 `PingFang SC` / `苹方`。
- 如果需要变量绑定，先确认变量 collection 和 variable key；当前 Arco Design System (New) 可见 collection 本身无变量。
- 写入或修复页面后，必须运行一次边界审计脚本：切到目标 page，遍历 top-level frame、关键容器和 table 直接子节点，用 `absoluteBoundingBox` 判断可见内容是否超出父容器；明确命名的图表 viewport/crop 可作为例外，但必须单独检查图形和业务标签完整。
- 写入或修复页面后，必须运行一次组件伪装层审计：扫描所有可见和隐藏节点名，若存在 `Select cover`、`Input cover`、`Button text cover`、`Switch cover`、`Text / ⌄`、`fake-*`、`overlay-*` 等用于控件内容伪装的节点，必须删除或改为组件实例内部覆盖后才能交付。
- 组件伪装层审计必须同时检查同一区域的双文本源：若一个 Arco 实例内部已有文本，且其外部 4px 内存在同内容或替代内容的 sibling 文本/矩形，应视为 cover 风险并修正。
- 审计脚本不要把所有 `clipsContent=true` 都视为通过。普通 Card/Table/Modal 内出现 clipping 往往是布局错误；只有名称明确为 `Chart viewport/*`、`Chart crop/*`、`Chart mask/*` 的图表视口才可被当作有意裁切。
- 对表格的宽度修复不能只 `resize(table)`；还要检查并调整 table 直接子节点的列宽、x 坐标和 action cell，确保直接子节点右边界不超过 table 右边界。
- 对外层 frame 高度修复不能只拉大画板；还要按 `maxVisibleChildBottom + bottomPadding` 收口，避免出现大面积空白或后续 frame 被重叠。
- 对复杂中后台批量返修，必须增加跨 frame 规则审计：顶部按钮白名单、每区主按钮数量、表格 action cell 边界、状态颜色、右侧空白、重复 step、Drawer 遮罩层数量、流程图/依赖图连接线是否穿过文本。

# 11｜当前缺口与清晰度评估（审计反馈）

> 本文档是依据对《Things 高保真复刻研究》全书（01–10 及 HarmonyOS H00–H08 完整规格）的**通读形成的现状审计与反馈**，供后续读者与接手模型快速定位"当前还缺什么、哪些还不清晰"。它不是研究结论，而是对"当前状态"的外部评估。整理日期：Gap 审计。

## 0. 结论先行

- 三种维度的"清晰"程度并不同：**数据结构（领域模型）最清晰；交互的"语义/契约层"清晰；UI 的"结构与契约"清晰**。
- 真正的模糊集中在两点：**① UI 的精确视觉值**，**② 一批具体交互/边界行为**（多为 PENDING 证据缺口）。
- 最大的工程缺口有四个：**① 真机证据资产与测量管线不在仓库**；**② 尚无真正的 HarmonyOS 工程与可自动验收闭环**；**③ 规格只写到组件层，页面层与 Todo 级交互未成规格**；**④ 状态/任务台账缺失且 09.4 已过期**。

---

## 1. 当前缺什么（按影响排序）

### 1.1 真机证据资产本身不在仓库（最致命）

整套高保真的地基是"iPhone 真机录屏/截图"。规格里反复引用 `用户原始 512×1108 录屏`、具体帧与 epx 测量，但这些**原始媒体不在仓库**——仓库里只有从它们推导出的文字规格。

后果直接：H02/H03/H04/H07/H08 里几乎所有视觉值都标注 `PROVISIONAL` / `CALIBRATION_REQUIRED` / `PENDING`（checkbox 蓝、deadline 粉、字号、行高、radius、shadow、rowHeight……全部"等 A 级 crop 校准"）。

只要没有这套原始证据，**"高保真复刻"就无法从 `PARTIAL_VERIFIED` 升级为闭环**。这是与项目"高保真"目标直接冲突、却无法自证的根本缺口。H08-B 也明确指出"设计生成图不能作为 Token 事实源"，进一步确认只能靠真实证据。

### 1.2 还没有真正的 HarmonyOS 工程（验收流水线是 spec 但未实现）

仓库目前**纯文档**（`doc/fty1/things-gao-bao-zhen-fu-ke-yan-jiu`），没有任何 ArkTS/ArkUI 代码、`DevVisualLab`、Fixture 数据、Golden 截图、Inspector/Geometry 采集、截图 diff 或 CI。

H00 那句 `Spec → Fixture → Implement → Simulator → Inspector → Diff → VERIFIED` 目前只停留在**原则**层面：

- 没有工程骨架 → 组件无法"实现"；
- 没有 Visual Lab + Fixture → 无法稳定复现；
- 没有 Inspector + Golden + diff harness → 无法拦截回归。

换句话说，**H02–H08 目前是 `SPEC_COMPLETE`，没有一个是 `IMPLEMENTED`**，更谈不上 `VERIFIED`。而"可自动验收闭环"恰是这个项目区别于"AI 觉得像"的核心价值。

**另：本机是否具备 HarmonyOS 开发环境（DevEco Studio / ArkTS SDK / 模拟器）尚未确认**。若没有，`截模拟器 Golden` 这一步便无法执行，验收无从谈起。

### 1.3 规格只写到"组件层"，页面层与 Todo 级交互尚未成规格

- **Overlay 剩余**：H08-E ReminderPicker、F TagPicker、G MovePicker、H RepeatEditor、I Confirmation / ActionMenu Regression——未写。
- **Todo 级交互组件**：Multi-select / BatchActionBar、MagicPlus、Todo DragCoordinator、QuickFind——在 04/05 有研究，但**没有正式 H-spec**（组件优先路线与交互层之间有个断点）。
- **页面组合**：Main Lists / Today / Inbox / Upcoming / Anytime / Someday / Logbook / Area / Project / QuickFind 的页面级规格 + Page Golden 尚未写（07.5 只给了 fixture 示例）。这是剩余最大的一块纯规格工作。

### 1.4 状态/任务台账缺失且已过期

- `09.4` 仍写"推进到 H05-A"，实际规格已到 H08-D——**状态文档滞后**。
- 缺一张**如实记录每项 `SPEC_COMPLETE / IMPLEMENTED / VERIFIED` 与依赖**的活台账，也缺"当前在做哪项、下一项是什么、关键路径"的说明。10.5 给了阅读顺序，但没给**执行进度**。这对新接手的模型（包括我）都是额外的定位成本。

### 1.5 只有靠本机取证才能补的产品边界（Pending）

这批 09.3 列出的 Pending **不是规格能解决的，必须靠 iPhone 录屏/截图**：Heading 整组 drag 完整表现、After-Completion Repeat Editor 呈现、Create Next Copy 反馈、Pause/Resume/Stop 菜单、Deadline overdue 视觉、Deadlines 列表页、Project-level Deadline、Project 有未完成 children 时的 Complete/Cancel 流程、Group-by-List × This-Evening 嵌套，以及一大批精确 token。**这些是"证据缺口"，不是"写作缺口"。**

### 1.6 一套"可判断对错"的测量/审计工具

即便有了模拟器截图，目前"从视频/截图抽 epx、取色样本、标 baseline、计算 gap"的**测量管线**只是用了 `epx / C 级` 的话术，仓库里没有把证据→数值的采集脚本或数据表，也没有"diff 阈值/容差"的可执行定义。否则 `P0/P1/P2` 优先级与 `±1vp` 都只是文档口号。

---

## 2. UI / 交互 / 数据结构 清晰度评估

| 维度 | 结构 / 语义 / 契约 | 精确值 / 具体行为 |
|---|---|---|
| **数据结构** | ✅ 很清晰（实体、关系、不变量、Command、UI-State 分离） | ⚠️ 缺一份 canonical schema；部分 Projection 与索引细节 |
| **交互** | ✅ 清晰（架构、事件隔离、状态机、手势仲裁、命令边界） | ⚠️ 一批行为 PENDING（Return / 拖拽 / 动效 / undo / 边界 flow） |
| **UI** | ✅ 清晰（组件树、slot、layout 契约、anchor、token、禁止项） | ❌ 精确视觉值普遍未定，需证据 + Golden |

**注：结构/语义/契约层已经足够清晰到可以开始写规范并按规范开发；真正模糊的是"精确值"与"一批交互/边界行为"，而这两类无法靠写文字补齐，只能靠真机证据 + 模拟器 Golden 闭环。**

### 2.1 数据结构（清晰度最高）

**清晰的**：实体与关系（Todo / Project / Area / Heading / ChecklistItem / Tag / Repeat(Template/Series/Copy)，`Area⊃(Project⊃Heading⊃Todo, Todo)`、Project 可无 Area、Project 不嵌套、Heading 只属一个 Project）；时间语义（`StartMode=OnDate|Anytime|Someday`、`evening` 仅 Today+true、`Evening≠固定时钟`、`DeadlineValue=LocalDate|None`、Deadline⊥Start）；Tag（direct/inherited/effective 不复制）；生命周期（Open/Completed/Canceled、IsLogged 独立、Delete 独立命令）；排序（`ContextOrder` 上下文相关）；**UI State 与 Domain State 明确分离**；完整 **Command 清单**；明确的 Projection/Query 集合。

**仍不够清晰**：
- 缺一份**统一、字段级、类型级的 canonical schema / ERD**——实体字段多散落在 02/03 的散文里（Todo 完整字段、Project/Area 完整字段、`IsLogged`、`notes`、repeat relation、order、派生字段），没有一块"单一事实源"类型定义。H05/H06/H07 有零散 interface，但未汇总成完整 Domain schema。
- **ChecklistItem 模型偏薄**（title/status/rank），内部排序 identity、canceled 呈现仍是 Pending。
- **Projection membership 的具体算法未写完**：Today/Upcoming 候选来源（Today Start、Deadline、carryover、repeat occurrence、This-Evening × Group-by 嵌套边界）只说"统一 Projection Engine 负责"，但规则本身未冻结，部分还 Pending。
- **Search 索引（NavigationIndex / ContentIndex）只给了方向**，未给细节。
- **同步 / 离线 / tombstone、提醒调度、通知**只是 E 级提法，未规格化。

### 2.2 交互（语义层清晰，一批行为 PENDING）

**清晰的（架构/语义/契约）**：Todo 原地展开而非独立 Route、展开连续性状态机；Checkbox 点击与 Row 展开严格隔离；Selection Mode 与 SelectionController 分离；Drag 状态机、手势仲裁、`DragIntent → Domain Command`、drop 前不写 DB；When/Deadline/Reminder 走统一 Overlay（H08-A/B）的 anchor/placement/keyboard/focus/coexistence 契约；Complete/Cancel/Delete 独立语义；Magic Plus / Quick Find 的 intent-compression 定位。

**仍不清晰（具体行为，多为 Pending）**：
- **政策类行为**：Title/Notes/Checklist 的 Return、空标题 commit、空 Notes、Checklist 空行 Return/Backspace/blur（`PENDING-TITLE` / `CHECKEDIT` 系列）。
- **拖拽/动效细节**：Heading 整组 drag preview/insertion/haptic、Todo/Checklist/Area drag 的 scale/shadow/haptic/阈值、reflow 曲线、auto-scroll——全部 PROVISIONAL/Pending。
- **边界 flow**：>100 行粘贴、空行/bullet 归一化、Undo 粒度、URL 点击、task-list marker tap、full-swipe——Pending。
- **键盘/焦点/外部关闭的确切策略**（per-picker keyboard/outside/focus-restore）——Pending。
- **若干完整页面交互流程**：Deadlines 页、Project 带未完成 children 时 Complete/Cancel、Pause/Resume/Stop、Create Next Copy 反馈、This-Evening × Group-by 嵌套——都缺 A 级证据。

这些是**证据缺口**，写规格补不上。

### 2.3 UI（结构与契约清晰，精确值几乎全未定）

**清晰的（结构/契约，其实很扎实）**：组件树与 Slot 架构（Row = CheckboxSlot + TitleLine + SecondaryLine + Trailing；ExpandedTodo 的 EditorContent / ActionBar 分区）；**Layout/Geometry 硬契约**（anchor：`titleEditor.left == collapsedTitle.left ±1vp`、`notes.textLeft == title.textLeft ±1vp`、hit target 与视觉分离、Stable ID）；**明确的禁止项**（无 Card/Divider/Shadow、禁系统默认 Checkbox、禁 Unicode ✓/↻、禁 Material/Card 视觉、禁 literal color/size）；Token 化与视觉语言总则（安静、平坦、内容优先、留白即 IA）。

**仍不清晰（精确视觉值）**：几乎所有颜色/字号/行高/间距/圆角/阴影/图标尺寸/row height 都是 `PROVISIONAL` / `CALIBRATION_REQUIRED` / `PENDING`（checkbox 蓝、deadline 粉、overlay 深色、Heading 视觉、drag preview 等）。没有原始证据 + 模拟器 Golden，精确视觉无法落地。

---

## 3. 建议的下一步

1. **先补"单一事实源"**：把散落字段收拢成一份统一的 Domain schema（实体字段 / 类型 / 关系 / 派生值与 Projection 规则），作为后续对照与实现的唯一依据。这是数据结构里最明显、且现在就**不需要新证据**就能做的缺口。
2. **同步一份如实进度台账**：修正 09.4，标清每项 `SPEC_COMPLETE / IMPLEMENTED / VERIFIED`、依赖与关键路径；列出"当前正在做 / 下一项"。这能直接减少接替成本。
3. **开始真实工程 + 验收环**：搭建 HarmonyOS 工程骨架、DevVisualLab + Fixture + Golden + Inspector/Geometry/diff。这是让"高保真"自证的前提。
4. **补齐剩余规格**：H08-E~I、Todo 级交互（Multi-select / MagicPlus / Drag / QuickFind）与页面组合 + Page Golden。
5. **取证补 A 级**：针对 09.3 的 Pending 清单做真机录屏/截图，解锁精确视觉与行为。

> 一句话：现在最缺的不是"再写一份规格"，而是 **(a) 把真机证据与测量管线建起来、(b) 把 HarmonyOS 工程 + Visual Lab + Golden/diff 验收环真正实现、(c) 补齐 H08-E~I、Todo 级交互与页面组合规格**。其中 (a)(b) 是让"高保真"能自证的前提，否则项目会一直停在"规格完备但无法验证"状态。

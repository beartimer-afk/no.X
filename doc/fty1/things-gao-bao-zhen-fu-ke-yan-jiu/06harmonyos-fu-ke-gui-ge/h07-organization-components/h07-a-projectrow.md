# H07-A｜ProjectRow

> HarmonyOS 26 / ArkUI｜正式开发规格｜可独立验收

## 定位

ProjectRow 是 Project 在 Main Lists / Area / 系统 Projection 中的摘要与导航行；它不是进入 Project 页面后的 `ProjectHeader`。

ProjectRow 负责 Project identity、title、progress、少量状态与导航；ProjectHeader 负责进入项目后的标题、Notes、Tags、When、Deadline 与生命周期编辑。两者禁止揉成一个组件。

## 官方事实

Project 用于多步骤目标；可属于 Area 或无 Area；Main Lists 把 Projects 嵌套在 Areas 下；Project 有 Progress Pie；Project/Area 可重排，Project 可拖入/拖出 Area；Project 支持 Start、Deadline、Tags、Notes 和 Open/Completed/Canceled 生命周期。

## Presentation Model

```ts
export interface ProjectRowPresentation {
  projectId: string
  title: string
  progress: ProjectProgressPresentation
  placement: ProjectRowPlacement
  lifecycle: ProjectLifecyclePresentation
  deadline?: DeadlinePresentation
  isRepeatingCopy: boolean
  isSelected: boolean
  isDragging: boolean
  enabled: boolean
}
```

Progress 由 Query/Presenter 计算，Row 不遍历 children。`fraction=1` 也不能自动 complete Project。

## Progress Pie

使用自有 `ProjectProgressPie`，不用 TodoCheckbox 或系统 Circular Progress。精确 size/stroke/color 在本轮未重新取得真机源视频逐像素测量，因此全部 `CALIBRATION_REQUIRED`。

## Structure

```
ProjectRow
├─ ProjectProgressPie
├─ MainContent
│  ├─ ProjectTitle
│  └─ OptionalMetadata
└─ OptionalTrailing
```

默认保持 Progress + Title 的克制结构；Deadline/Repeat 等没有 A 级行内证据时只预留 slot，不擅自显示。

## Placement

`MAIN_LISTS_STANDALONE / MAIN_LISTS_INSIDE_AREA / AREA_PAGE / SYSTEM_PROJECTION`。Area indentation 是 Presentation Context，不写入 Project Domain。

## Visual

不做 Card、永久 Shadow、厚 Divider 或 Material Progress。Title/Typography、row height、Area indent、Progress Pie 尺寸全部 Token 化；本轮没有新测量则保持 Pending。

## Tap

整行点击打开 Project Page。Progress Pie 不是完成按钮；Row tap 不进入 Todo 式 inline expansion。

## Drag / Move

Project 可重排并移入/移出 Area。Drop 调用 `moveProject(projectId,targetAreaId?,beforeProjectId?,afterProjectId?)`，Command 统一处理 area relation、rank、inherited tags 和 Projection invalidation。禁止 UI 直接 set areaId/sortIndex。

## Collapse

Area collapsed 只改变可见 Projection，Project Domain/order 不变。

## Tags

Project 移入带 Tag Area 后 effective context 可变化，但 direct project tags / child direct tags 不被复制。Inherited Tags 仍由查询计算。

## Lifecycle / Deadline / Repeat

Project lifecycle 独立于 Progress；Project Deadline/Repeat row presentation 当前均有语义 Model 但视觉保持 Pending，不能照抄 TodoRow badge。

## Accessibility

Row 表达“项目”、title 和进度语义，例如 3/5；Progress Pie 不依赖颜色作为唯一信息。

## Stable IDs

`project_row_<id>.root/progress/title/metadata/trailing/drag_preview`。

## Fixtures

PR01 standalone\_empty；PR02 progress25；PR03 progress75；PR04 progress100\_open；PR05 inside\_area；PR06 long\_title；PR07 system\_projection；PR08 completed；PR09 canceled；PR10 deadline\_pending；PR11 repeat\_pending；PR12 pressed；PR13 selected\_pending；PR14 dragging；PR15 narrow；PR16 accessibility font scale。

## Geometry Gate

Progress/title gap 稳定；Area indent 只由 placement 决定；Title 不覆盖 trailing；整行 hit target；Progress Pie 不因 title 行数横向漂移。

## Pending

row minHeight、title typography、progress pie size/stroke/color、Area indent、pressed、drag preview/gap/haptic/motion、Project selection、Deadline、Repeat、Completed/Canceled exact appearance。

## 低能力模型顺序

ProgressPie gallery → standalone row → progress fractions → insideArea placement → long title/narrow → tap/accessibility → Drag intent / Move Command → into/out Area tests → collapse Projection → internal Golden → 真机素材恢复后视觉校准。

## 禁止事项

不得复用 ProjectHeader；不得 ProgressPie 当 Checkbox；不得 100% 自动完成；不得 Row 自己算 progress；不得 indentation 写 Domain；不得 drag 直接写 parent/rank；不得复制 inherited tags；不得把 Header 字段全塞行内；不得用系统 ProgressBar 或猜像素。

## VERIFIED

Core 可 FUNCTIONAL\_VERIFIED：Progress/Domain 分离、各 placement、navigation、long title、accessibility、reorder/move into/out Area、stable identity、collapse projection、event isolation PASS。

精确 Visual 等源录屏重新测量后再升级。

下一子任务：**H07-B｜AreaRow**。

# H07-B｜AreaRow

> HarmonyOS 26 / ArkUI｜正式开发规格｜可独立验收

## 定位

AreaRow 是 iPhone Main Lists 中 Area 的结构容器行，负责 Area identity、导航和 child Projects 的展开/折叠。它不是进入 Area 页面后的 `AreaHeader`，也不是系统 Projection 中纯分组的 ParentGroupHeader。

## 产品语义

Area 是持续责任范围，不是可完成目标，因此没有 Project 式 lifecycle、Progress Pie 或 CompletionControl。

```
Area
├─ Projects
└─ direct Todos
```

## 官方事实

Area 用于组织长期 responsibility；Projects 在 Main Lists 中嵌套于 Areas；Areas 可折叠；Project/Area 可拖拽重排；拖 Area 时其中 Projects 作为结构随之移动；Project 可拖入/拖出 Area；Area Tags 会被子项用于继承搜索/过滤。

用户既有 A 级录屏还确认 Area 删除有明确确认步骤。

## Domain / Presentation

Area Domain 只维护 id/title/directTags/rank 等组织事实。`isCollapsed` 属于 Presentation Preference，不写入核心 Domain。是否跨设备同步 collapse 偏好保持 Pending。

## Structure

```
AreaRow
├─ CollapseControl
├─ AreaTitle
└─ OptionalContextAction

AreaChildrenContainer
└─ ProjectRow[]
```

AreaRow 与 children container 分离；collapse 只是停止/收起 children presentation，不修改 Project parent/order。

## CollapseControl

使用 Things 自有 disclosure visual，不用 HarmonyOS Accordion 默认样式。CollapseControl 与 Row 主体导航 intent 分离：collapse 只 toggle Presentation；点击 Area title/body 打开 Area Page。

## Layout / Visual

AreaTitle、row height、collapse icon、Project indent、motion 全部 Token 化。本轮没有重新取得源录屏逐像素测量，因此保持 `CALIBRATION_REQUIRED`，禁止用常见 48vp 等冒充最终值。

Area 没有 Progress Pie、完成 Checkbox 或 child completion percentage。

## Direct Todos / Tags

Area 可以直接含 Todo，但 Main Lists AreaRow 不擅自把 direct Todos 渲染成 ProjectRow。进入 Area Page 后由 AreaQuery 展示 direct Todos / Projects。

Area direct Tags 不复制到 child directTags；Move Project into/out Area 后 effective inherited context 重新计算。

## Drag / Reorder

拖 Area 时视觉上是 AreaBlock（AreaRow + visible child ProjectRows）结构移动，但 Domain 只调用一次 `reorderArea`；children areaId 和 relative order 不变。

ProjectRow 可以把 Area 作为 drop target，最终调用 `moveProject` Command，AreaRow 不直接改 parent。

Collapsed Area hover 是否自动展开保持 Pending。

## Magic Plus

Main Lists 中 Area 提供 Project insertion target；Magic Plus drop 只发 `CreateProject(areaId, insertionPoint)` intent，不直接创建实体。

## Delete

Delete Area 是 destructive command，不等价于“把 children detach 然后删空 Area”。必须先 request → impact/confirmation → Domain delete command。精确 iPhone 文案/按钮/动画需从 A 级录屏再校准。

## Accessibility

AreaRow 要表达 Area role、title、expanded/collapsed 状态，并分别暴露 collapse 与 open action，避免重复朗读。

## Stable IDs

`area_row_<id>.root/collapse/title/children/drag_block/drop_target`。

## Fixtures

expanded empty/1/3 projects、collapsed、long title、pressed、drag source、project drop target、Magic Plus target、collapsed target、destructive pending、narrow、accessibility expanded/collapsed。

## Geometry Gate

expanded/collapsed AreaRow 自身 anchors 不跳；child Project indent 与 H07-A 一致；collapse 后 sibling 不 overlap；long title 不覆盖 collapse；child width 不越 Main Lists bounds。

## Pending

collapse sync preference、row geometry/Typography/icon、collapse motion、Area drag preview/haptic、collapsed hover auto-expand、delete dialog visual/text、Area selection、pressed/drop target visual。

## 低能力模型顺序

static AreaRow → Area+ProjectRow composite → functional collapse → Domain separation test → open event isolation → Area reorder → Project into/out Area → Magic Plus target → delete confirm flow → accessibility → internal Golden → A 级素材重测校准。

## 禁止事项

不得 Area completion/progress；不得复用 AreaHeader；不得 collapse 写 Domain；不得 collapse 改 child order/parent；不得拖 Area 时逐个移动 child Domain；不得 Project drop 直接 set areaId；不得复制 inherited tags；不得 delete Area 当 remove parent；不得无确认破坏性删除；不得默认 Accordion visual 或猜最终几何。

## VERIFIED

Core FUNCTIONAL\_VERIFIED：open、collapse/expand、Domain 不变、child order preserved、Area reorder、Project into/out、Tag inheritance side-effect、Magic Plus intent、Delete pre-confirmation、Accessibility、H07-A regression PASS。

精确视觉等 A 级源素材重测后升级。

下一子任务：**H07-C｜HeadingBlock**。

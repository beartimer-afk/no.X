# H05-C1｜NotesEditor Core

> HarmonyOS 26 / ArkUI｜正式开发规格｜可独立验收

## 定位

NotesEditor Core 只实现 Todo 展开态 Notes 的文本编辑内核：多行、长文、placeholder、caret、selection、IME、focus、keyboard、draft/commit、性能，以及 H05-C2 Markdown Presentation 所需的一一文本索引接口。

不负责 Markdown 解析与样式、Format 菜单、Checklist、URL 打开、Find in Text、附件、Domain 保存策略或 ExpandedTodo 外层几何。

## 产品事实

A 级真机已确认：Expanded Todo 标题下存在 Notes 区域，空状态可见“备注”，Notes 与 Title 内容列左对齐，键盘弹出仍保持原列表上下文。

B 级官方确认：Things Notes 支持长文本、Markdown、iPhone 选区 Format；Markdown 特殊字符保持可见；当前不支持图片/附件。URL Scheme 的 notes 参数存在 10,000 未编码字符限制，但这是接口限制，不能推导为 App 内部数据上限。

## ArkUI 选择

最终基础使用 `RichEditor`，但首版只允许 Text Span，不使用 ImageSpan / BuilderSpan / Attachment。

```
NotesEditor
└─ NotesEditorHost
   └─ RichEditor
      └─ Text Spans only
```

之所以不用普通 TextArea：最终需要在保持 Markdown 原字符可见的前提下，对不同文本范围施加 heading / bold / italic / highlight / code 等样式。

## 数据事实源

唯一事实源：

```ts
rawText: string
```

Markdown Parser 只能派生 Presentation spans，不能改变 rawText。

例如 `**重要** 明天处理`，两侧 `**` 必须仍在编辑器中可见、可选、可编辑。

禁止用 RichText JSON 替代 rawText 作为唯一存储。

## Model / Events

```ts
export interface NotesEditorModel {
  todoId: string
  rawText: string
  isEditing: boolean
  enabled: boolean
}
```

对外事件包括 draft change、editing change、commit、selection change、ensure visible、未来 format menu request。

组件不直接访问 Repository。

## Stable ID / Focus Key

* `notes_editor.root`
* `notes_editor.host`
* `notes_editor.rich_editor`
* focus key：`notes_${todoId}`

禁止列表 index 作为 key。

## Layout

Root.width = ExpandedTodo.editorContent.width。

硬约束：

```
notes.textLeft == titleEditor.textLeft ±1vp
```

空 Notes 仍需可点击最小编辑区域。长 Notes 默认随内容增长；何时切换为内部滚动目前保持 `PENDING-NOTES-01`，不得随意固定 120vp 等高度。

## Placeholder

zh-CN Fixture 使用真机已观察到的 `备注`。

只在 rawText 为空时显示；不得自行改成“添加备注…”或加图标。精确获焦后行为保持 Pending。

## Typography / Color

基础正文使用 `ThingsTypography.notesBody`，视觉权重低于 Todo Title。

颜色全部走 Token：notesText、notesPlaceholder、caretPrimary、selectionBackground，以及后续 Markdown 专用 Token。未完成真机校准前保持 PROVISIONAL。

## RichEditor 默认视觉清理

覆盖 background、border、radius、padding、scrollbar、selection、caret 以及不需要的系统插入能力。最终视觉应像文字直接存在 ExpandedTodo Surface 中，而不是嵌套另一个输入卡片。

## Text Index Contract

所有 Caret、Selection、Markdown Span 的 start/end 都以 rawText 索引为准。

Markdown Presentation 不允许隐藏/删除语法字符，因此屏幕文本与 rawText 索引保持一一对应。这是后续 Format、Selection 和 Undo/Redo 的核心约束。

## Caret / Selection

必须支持 caret、选择、Copy/Cut/Paste、Select All。外部 draft 同步不得把 caret 重置到末尾；selection change 只更新 Presentation state，不触发 Domain mutation。

## IME

中文/日韩 composition 期间不能每次中间态都重建 RichEditor。允许延迟 Markdown style reconciliation，但输入候选必须稳定。

覆盖中文拼音、中英混合、emoji、多行粘贴。

## Draft / Commit

推荐：Presentation Draft → lightweight onDraftChange → Presenter debounce/transaction → `updateNotes` Command。

```ts
export enum NotesCommitReason {
  BLUR,
  SWITCH_FIELD,
  COLLAPSE,
  PAGE_CONTEXT_CHANGE,
  EXPLICIT_DONE
}
```

空 Notes 是合法状态，删除全部备注不能删除 Todo。

## Enter

Notes 中 Return 默认插入真实换行。Markdown list continuation 属于 H05-C2，不在 C1 实现。

## 长文本性能

至少覆盖 1 行、10 行、50 行、200 行 DEV stress。禁止每个字符都全量重建所有 spans / editor。

## Gesture Arbitration

editingField == NOTES 时，Tap/Double Tap/Long Press 优先给文本编辑，Todo Drag 禁用或降级。长按必须可做文本选择而不是拖整条 Todo。

## Keyboard / Ensure Visible

获焦及 Caret 下移时可以请求 `onRequestEnsureVisible(todoId, caretRect)`，由 Page / KeyboardCoordinator 完成键盘避让与列表滚动。组件不硬编码键盘高度。

## Markdown Hook

C1 提供 `NotesStyleSpan { start, end, styleType, level? }` 输入接口；C2 负责 rawText → MarkdownPresentationParser → styleSpans。Parser 不改变 rawText 长度。

## 图片/附件边界

Things 当前 Notes 不支持图片/附件，因此首版禁止 ImageSpan、paste image、附件插入等能力泄漏。

## Fixtures

N01 empty\_idle；N02 empty\_focused；N03 one\_line；N04 multiline；N05 caret\_middle；N06 selection；N07 ime\_cn；N08 paste\_multiline；N09 markdown\_raw；N10 stress\_200\_lines。

## Golden / Geometry

冻结 empty、one-line、multiline、selection、markdown\_raw\_core 等。C1 不验最终 Markdown 样式，重点验 placeholder、base typography、caret、selection、高度与 surface cleanliness。

核心 Anchor：root.left、textStartX、firstBaseline、root.right、root.bottom、currentCaretRect。

```
notes.textStartX == titleEditor.textStartX ±1vp
```

## Interaction PASS

* 点“备注”进入编辑且 route 不变；
* 中间插字 caret 不跳末尾；
* 中文 IME 不被 span rebuild 打断；
* 全选删除后 Todo 保留、placeholder 恢复；
* 多行粘贴完整；
* 长文 Caret 下移会请求 ensureVisible。

## Pending

* PENDING-NOTES-01 内部滚动阈值
* PENDING-NOTES-02 精确字号/行高
* PENDING-NOTES-03 Placeholder 获焦后行为
* PENDING-NOTES-04 粘贴图片/附件反馈
* PENDING-NOTES-05 Notes + Checklist 同时很长的高度策略
* PENDING-NOTES-06 selection/caret 精确色
* PENDING-NOTES-07 URL 点击/编辑边界

## 低能力模型编码顺序

RichEditor text-only wrapper → placeholder/basic text → 对齐 H05-B anchor → focus/draft → caret/selection → IME → multiline/paste → stress → styleSpans 接口 → 全量 regression → Golden PASS 后 LOCK。

## 禁止事项

不得把 Notes 做成单行输入；不得 RichText JSON 替代 rawText；不得隐藏 Markdown 符号；不得插图片；不得每键重建 editor；不得直接 DB transaction；不得固定小高度；不得长按触发 Todo Drag；不得删除空 Notes 时删除 Todo；不得误用 URL Scheme 10,000 字符上限。

## VERIFIED 条件

N01–N10 functional PASS；与 Title 左 anchor <=1vp；空/短/长布局正确；caret/selection/IME 正常；200 行无结构性卡顿；无 RichEditor 默认卡片视觉；H02/H03/H04/H05-A/H05-B regression PASS；Golden 达到 H00 阈值。

下一子任务：**H05-C2｜Markdown Presentation & Format**。

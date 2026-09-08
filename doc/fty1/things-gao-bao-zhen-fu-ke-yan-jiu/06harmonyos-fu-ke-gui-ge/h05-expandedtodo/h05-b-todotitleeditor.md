# H05-B｜TodoTitleEditor

> HarmonyOS 26 / ArkUI｜正式开发规格｜可独立验收

## 1. 组件目的

TodoTitleEditor 是 ExpandedTodo 内专门负责 Todo 标题编辑的 Presentation Component。

只负责：标题展示、多行编辑、自动换行、光标、选区、输入法、焦点、文本变化、边界字符以及将结果交给 Presenter / Command 层。

不负责 Notes、Checklist、When、Deadline、Reminder、生命周期、数据库保存、路由、ExpandedTodo 总高度与页面滚动。

## 2. 使用位置

用于 Today / Inbox / Upcoming / Anytime / Someday / Area / Project 等页面中 Todo 原地展开态，不单独作为页面使用。

## 3. Domain / Presentation 边界

```ts
export interface TodoTitleEditorModel {
  todoId: string
  text: string
  isEditing: boolean
  isCompleted: boolean
  enabled: boolean
}
```

对外事件：`onTextChange`、`onEditingChanged`、`onCommit`、`onSelectionChanged`、`onRequestEnsureVisible`。组件不能直接持有 Domain Todo 或写数据库。

## 4. ArkUI 基础组件

标题需要多行，因此使用 `TextArea` wrapper，而不是单行 `TextInput`。必须覆盖系统默认 background / border / radius / padding / underline / selection 等视觉，避免系统输入框样式泄漏。

```
TodoTitleEditor
└─ TitleInputHost
   └─ TextArea
```

Stable IDs：`todo_title_editor.root`、`todo_title_editor.input_host`、`todo_title_editor.text_area`。

## 5. Geometry

Root.width = ExpandedTodo.editorContent.width；TextArea.width = 100%。禁止固定屏幕宽度、设备 px、自算安全区、重复扣 Checkbox 宽度。

核心 Invariant：

```
titleEditor.left == collapsedTodoTitle.left ±1vp
```

## 6. 高度与换行

不设固定总高度。一行保持单行高度，多行随内容自然增长。当前“标题最大编辑行数”缺少 A 级证据：`PENDING-TITLE-01`。开发阶段允许自然增长，工程保护上限只能标记 `DEV_GUARD`，不得称为 Things 行为。

## 7. Typography

复用 `ThingsTypography.todoTitle` / `todoTitleCompleted`。编辑态与折叠态 fontSize、weight、lineHeight、baseline 必须连续；首行 baseline 和左边缘误差均 <=1vp。

## 8. Color

只使用 Token：`textPrimary`、`textCompleted`、`caretPrimary`、`selectionBackground`、`inputPlaceholder`。当前证据不支持完成态删除线，因此禁止擅自增加 strike-through。

## 9. Padding

TextArea 默认内边距必须显式清除或统一 Token 化。默认 outer margin=0、InputHost padding=0、TextArea padding=0 或唯一补偿 Token。禁止页面局部负 margin 修补。

## 10. 状态

* DISPLAY\_IDLE
* EDITING\_FOCUSED
* EDITING\_SELECTION
* EMPTY\_EDITING
* LONG\_MULTILINE
* COMPLETED\_EDITING
* DISABLED

## 11. Focus 与键盘

点击标题区域 → TextArea 获焦 → editing=true → `onRequestEnsureVisible(todoId)` → 页面协调滚动 → 系统键盘出现。Stable focus key 使用 `todo_title_${todoId}`，禁止列表 index。

键盘避让由 Page / KeyboardCoordinator 处理，TitleEditor 不硬编码键盘高度。

## 12. Enter / Return

当前缺少 A 级证据冻结 Return 行为：`PENDING-TITLE-02`。组件内部禁止把 Enter 私自定义为 createTodo、collapse 或 commit。由 TodoEditorPresenter / Page 统一决定。

## 13. Caret 与 Selection

必须监听 selection change，保存 selectionStart / selectionEnd。禁止每次 onChange 后把光标强制移动到末尾。外部 Model 重新赋值若导致 caret reset，需保存后恢复位置。

## 14. 输入事件

至少处理 `onChange`、`onFocus`、`onBlur`、`onTextSelectionChange`。只有确有产品规则时才使用 `onWillInsert/onWillDelete`。禁止编辑中自动 trim、格式化空格、大小写或 emoji。

## 15. IME Composition

中文拼音/日韩输入存在 composition。Presentation 可实时显示，但不能每次中间态都触发昂贵 Domain Command，也不能因为外部 state 重建 TextArea 打断候选词。

必须覆盖中文拼音、候选词、中间编辑、emoji、中英混合。

## 16. Commit

```ts
export enum TitleCommitReason {
  BLUR,
  EXPLICIT_DONE,
  COLLAPSE,
  SWITCH_FIELD,
  PAGE_CONTEXT_CHANGE
}
```

推荐 Presenter 管理 draft，离开字段时 commit，对高频 onChange 做合理 debounce/draft update，Domain 接收 `updateTitle` command。

## 17. 空标题

组件允许空 draft。最终 commit 空字符串如何处理属于产品规则：`PENDING-TITLE-03`。TitleEditor 不得自行 deleteTodo。

## 18. Gesture Arbitration

Tap 放 Caret、Double Tap 选择、Long Press 使用文本编辑菜单。父级 ExpandedTodo 不应截获导致 collapse。`editingField == TITLE` 时 Todo Drag 默认降低优先级/禁用。

## 19. Motion

TitleEditor 不做独立 entrance animation，只跟随 H05-A expand motion。禁止获得焦点时放大、Card 高亮、Material underline 或 floating label。

## 20. Accessibility

可聚焦、label 表达“任务标题”、文本可读、disabled 正确暴露，不把整个 ExpandedTodo 与输入框合并为不可编辑 node。

## 21. Responsive

窄屏自然换行，不水平滚动标题，不覆盖 Checkbox / ActionBar。Canonical Golden 固定 fontScale=1.0；Accessibility fontScale 单独做 Functional Fixture。

## 22. Fixture

* F01 short\_idle：`买一本测试用的书`
* F02 short\_editing
* F03 caret\_middle：`今天晚上健身半小时`
* F04 selection：选择 `晚上健身`
* F05 chinese\_2\_lines
* F06 english\_3\_lines
* F07 mixed\_unicode：`读 Chapter 3｜ArkUI ✅ 明天继续`
* F08 empty
* F09 completed
* F10 extremely\_long\_dev\_guard

## 23. Golden

冻结 short\_idle、short\_editing、caret\_middle、selection、chinese\_2\_lines、english\_3\_lines、completed。文本抗锯齿使用 Typography Mask 容差，但 root bbox / left / first baseline / lineHeight / total height / collapsed-expanded anchor 为硬 Gate。

## 24. Geometry Anchors

A1 root.left；A2 textGlyphStartX；A3 firstBaselineY；A4 caretX/Y；A5 root.right；A6 root.bottom。

```
A2(expanded) - A2(collapsed) <= 1vp
firstBaseline(expanded) - firstBaseline(collapsed) <= 1vp
```

## 25. Interaction PASS

* collapsed → expand → focus：route/todoId 不变，anchor 不变，keyboard 出现，ensureVisible 调用；
* caret\_middle 中间插入字符后光标不跳末尾；
* selection 不触发父级 Drag；
* 中文 IME composition 不被重建打断；
* blur 仅触发一次 commit；
* H02/H03/H04/H05-A regression 全 PASS。

## 26. Evidence

A：现有真机录屏确认原地展开、标题与 Checkbox 关系、键盘仍在原页面上下文。

B：HarmonyOS 官方 ArkUI TextArea、焦点、键盘、selection/caret 能力。

C：H05-A 512×1108 录屏几何测量，如 Checkbox 左缘约 18 epx、Title 左缘约 53 epx。

D/E：TextArea wrapper、Presenter draft、IME-safe commit、Stable Focus Key、Geometry Regression。

## 27. Pending

* PENDING-TITLE-01 最大行数/内部滚动
* PENDING-TITLE-02 Return/Enter 行为
* PENDING-TITLE-03 空标题 commit
* PENDING-TITLE-04 Caret 精确色
* PENDING-TITLE-05 Selection 精确色/alpha
* PENDING-TITLE-06 长标题与 ActionBar 同屏最大高度
* PENDING-TITLE-07 字体 metric 最终补偿

## 28. 低能力模型编码顺序

1. TextArea wrapper，关闭系统默认视觉；
2. F01 对齐 H03 anchor；
3. focus key + focus/blur；
4. onChange + draft；
5. selection + caret\_middle；
6. 多行自适应；
7. 中文 IME；
8. completed visual；
9. H02/H03/H04/H05-A regression；
10. Golden PASS 后 LOCK H05-B。

## 29. 禁止事项

不得做独立页面、Domain editing state、直接存 DB、空标题直接删除、私自 Enter 语义、抢 Checkbox、文字选择与 Drag 冲突、caret 跳末尾、固定高度裁切、系统默认 TextArea 外观。

## 30. VERIFIED 条件

F01–F10 结构/交互通过；collapsed→expanded anchor <=1vp；一行/多行自适应；中间插入不跳 caret；selection/IME/focus 正确；无系统默认视觉；所有下游 regression PASS；Golden 达到 H00 阈值。

下一子任务：**H05-C｜NotesEditor**。

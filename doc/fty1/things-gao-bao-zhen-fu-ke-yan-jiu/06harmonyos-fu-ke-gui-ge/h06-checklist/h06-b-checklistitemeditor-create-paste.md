# H06-B｜ChecklistItemEditor / Create / Paste

> HarmonyOS 26 / ArkUI｜正式开发规格｜可独立验收

## 目标

在 H06-A 的 ChecklistContainer / ChecklistItemRow 上增加 ChecklistItemEditor、创建 Draft、编辑已有 Item、Focus/Caret/Selection/IME、Return Policy、多行 Paste、单次 100 行防误操作、inline Markdown hook 与完整验收。

## 官方事实

Things iPhone/iPad 可在 Checklist 字段粘贴多行文本，每一行转换成一个 ChecklistItem；单次最多 100 行用于防误操作。URL Scheme / Shortcuts 也支持换行分隔的 checklist items。ChecklistItem 数据接口具有 title / completed / canceled。Things Markdown 说明明确提到 Markdown syntax 可以形成 styled notes or checklists，syntax 保持可见。

## 100 行不是总容量

```
MAX_ITEMS_PER_PASTE_OPERATION = 100
```

这是操作级保护，不是数据库 Checklist 总容量。禁止写成 `MAX_CHECKLIST_ITEMS = 100`。

## Editor 架构

```
ChecklistItemRow
├─ CompletionControl
├─ InlineMarkdownTextEditor
└─ DragGrip
```

普通文本输入可由 TextArea 支撑，但为了复用 Things 的 Markdown Presentation，最终使用轻量 text-only RichEditor / 可替换 abstraction；只复用 H05-C2 inline parser，不把 ChecklistItem 做成小型 Notes。

## Draft 与 Entity 分离

新 item 先进入 Presentation Draft：

```
ChecklistDraft {
  draftId
  todoId
  text
  insertionIndex
  focusState
}
```

空 Draft 不等于已持久化 Item。有内容/明确 commit 后才执行 createChecklistItem Command。已有 item 编辑必须保留 stable itemId，禁止 delete + recreate。

## Stable Focus Key

已有：`checklist_item_${itemId}`；Draft：`checklist_draft_${draftId}`。禁止 array index；reorder 后焦点仍跟随同一 Item。

## Layout / Height

Editor 位于 completion 与 grip 之间，长标题自然换行、Grip 固定右侧。编辑态与展示态第一 baseline 尽量一致。

Return 是关键 Pending：它可能创建下一 Item、插入换行或结束编辑，因此用 `ChecklistReturnPolicy` 表达，不允许 TextEditor 内硬编码。首轮可以 `CREATE_NEXT_ITEM` 作为 PROVISIONAL，但不能冒充 Things VERIFIED。

## Placeholder

Things 历史 release notes 证明 checklist item 有 placeholder 概念，但当前中文文案/出现时机缺 A 级证据。Visual Lab 可用 DEV placeholder，产品 Golden 不冻结自创文案。

## Caret / Selection / IME

中间插字不能跳末尾；selection 不触发 Domain；外部 state update 不重置 caret。中文/日韩 composition 期间不得重建整个 Checklist/Row 导致候选或键盘消失。

## Inline Markdown

ChecklistItem raw title 仍是唯一事实源。复用 H05-C2 的 inline parser / span reconciliation。Markdown syntax 保持可见，不另写分叉 parser。Block-level Markdown 在 ChecklistItem 中的精确支持范围保持 Pending。

## 创建第一项

H05-D ChecklistAction → Coordinator 聚焦 Checklist。若 count==0：创建 Presentation Draft → focus → keyboard → 输入 → commit → Domain Item。空 Draft 离开时可丢弃，不残留空 Entity（E 级实现建议）。

## 创建下一项

若 ReturnPolicy=CREATE\_NEXT\_ITEM：commit A → 在当前 insertion point 建 Draft B → focus B → keyboard 保持 → ensureVisible。精确插入位置仍待真机。

## 空行 / Backspace

空行 Return、空 Draft Backspace、已有 Item 清空后 blur 的精确 Things 行为均保留 Policy/Pending。ChecklistItemEditor 不能自行 delete Entity。

## 多行 Paste

```
clipboard plain text
→ normalize CRLF/LF
→ split lines
→ max 100 rows per operation
→ batch create command
→ preserve source order
→ insert at target position
```

推荐 `createChecklistItems(todoId, insertionPoint, titles[])`，禁止 100 次独立 Repository transaction。

空白行、trim、bullet 前缀清理、富文本 bullet 转换仍待证据。官方说明 bulleted list 可转换为 Checklist，最终会有 normalization，但不能凭猜测实现细则。

## >100 行

必须防护且不能静默截断。Presenter 返回 TooManyPasteRows / 等价错误并展示提示；原生文案和交互待 A 级证据。

## Keyboard / Focus Coordinator

Title / Notes → Checklist 切换统一由 TodoEditorCoordinator：commit current draft → update editing target → focus stable key → ensureVisible。连续创建 Item 时键盘 session 尽量保持，不重建 ExpandedTodo。

## 编辑态 Gesture

建议 editing item 暂时禁用/降低 Grip 与 Swipe 优先级，CompletionControl 可保留但需事件隔离；Parent Todo Drag 禁用。精确原生策略待补证。

## Fixtures

E01 edit\_existing\_short；E02 long；E03 caret\_middle；E04 selection；E05 ime\_cn；E06 create\_first；E07 create\_next；E08 empty\_blur；E09 empty\_return；E10 empty\_backspace；E11 paste\_3；E12 paste\_100；E13 paste\_101；E14 blank\_lines；E15 bulleted；E16 inline\_markdown；E17 reorder\_after\_edit；E18 narrow。

## Geometry / Interaction Gate

编辑/展示 textLeft 与 first baseline <=1vp；Grip/Completion anchor 不移动；Draft 与正常 Row rhythm 一致；focused row 在键盘上方可见。

Create first、Edit middle、Paste 3/100/101、Parent isolation 都必须自动化测试。

## Pending

* CHECKEDIT-01 Markdown subset
* CHECKEDIT-02 Return
* CHECKEDIT-03 Placeholder
* CHECKEDIT-04 新项插入位置
* CHECKEDIT-05 空行 Return
* CHECKEDIT-06 空行 Backspace/blur
* CHECKEDIT-07 Paste blank/whitespace/bullet normalization
* CHECKEDIT-08 >100 提示
* CHECKEDIT-09 Undo 粒度
* CHECKEDIT-10 编辑态 Swipe/Grip

## 低能力模型顺序

existing editor → stable ID → caret/selection/IME → Presentation Draft → create first → provisional Return policy → Paste 3 → Paste 100 guard → 101 error → inline Markdown hook → focus continuity → regression → Golden。

## 禁止事项

不得空 Draft 立即变永久 Entity；不得 edit delete+recreate；不得 index focus key；不得把 provisional Return 写成最终规则；不得把 100 行当总容量；不得 >100 静默截断；不得逐条 DB transaction；不得无证据自动 trim/去 bullet；不得分叉 Markdown parser；不得输入时重建整个 Checklist。

## VERIFIED

Core Functional：existing edit / create / draft / caret / selection / IME / Paste 3/100 / >100 guard / stable ID / focus continuity / parent isolation / H05+H06-A regression PASS。

Return/empty/backspace/bullet-normalization 等在缺 A 级证据时只能 `POLICY_PROVISIONAL`。

下一子任务：**H06-C｜Drag Reorder / Swipe Motion / Checklist Final Regression**。

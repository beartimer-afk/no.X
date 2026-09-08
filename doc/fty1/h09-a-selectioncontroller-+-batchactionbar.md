# H09-A｜SelectionController + BatchActionBar

Selection 是 UI Interaction State，不是 Todo 字段。

A 级真机已经确认：Today 中进入多选后右侧出现选择控制，selected row 使用浅蓝背景，底部浮动 batch toolbar；主动作包含时间、Move、Delete、More；More 中已观察到 Complete、Tags、Deadline、Duplicate、Share；Delete 有数量确认。

## Architecture

`SelectionController { mode, selectedIds, anchorContext }` 独立于 Domain。Row 只接 `selectionPresentation`，点击选择控制只修改 selectedIds。退出 mode 后恢复 TodoCheckbox presentation。

## Batch commands

所有批量业务走单一 Command，不允许 UI `forEach repository.write`。MIXED 状态由 Presenter 计算。Delete、Complete、Tags、Deadline、When、Move 等分别委托各自 Domain command/Overlay。

## Coexistence

Selection 与 ExpandedTodo、Drag、Overlay 的互斥/共存必须由 Global Interaction Coordinator 管理。进入 Drag 前 Selection policy 明确；打开 Batch Overlay 时 selectedIds 保持。

## Fixtures / Gate

0/1/N selected、mixed metadata、delete confirmation、batch When/Deadline/Tags/Move、exit restore、scroll retained。A 级视觉作为目标；精确 toolbar geometry/motion 继续 simulator Golden。

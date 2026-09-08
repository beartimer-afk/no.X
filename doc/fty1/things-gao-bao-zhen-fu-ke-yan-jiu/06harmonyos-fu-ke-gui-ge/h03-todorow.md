# H03｜TodoRow

TodoRow 是 Things 页面最核心的重复构件。H03 先冻结最普通 Row，再增加长标题、完成态和 Metadata；不能一次把所有状态写进一个巨型组件。

## H03-A｜Normal / Open 基础骨架

C 级真机样本显示普通 Row 的纵向节奏约 **57–58 epx**。鸿蒙首轮最小高度 Seed 约 **45vp**，但仍为 PROVISIONAL。

结构：

```
TodoRow
├─ CheckboxSlot       [H02 VERIFIED]
└─ ContentColumn
   └─ TitleLine
```

TodoRow 的视觉语言：

* 无 Card；
* 无 Shadow；
* 无 Divider；
* 依靠留白和一致的行节奏形成信息层级。

Checkbox 与 Title 的 x anchor 从真机样本测量并进入 Geometry Contract。

## 交互边界

* 点 Checkbox：完成/取消完成；
* 点 Row 内容区：原地展开 Todo；
* 两者事件互斥；
* Row 不负责路由到独立 Todo Detail Page。

## H03-B｜长标题、自适应高度、Completed

Row 不能固定 45vp 后把两行/三行标题裁掉。高度由内容测量决定，基础值只是 `minHeight`。

Completed 真机证据：

* Checkbox 变成完成态；
* Title 明显弱化为灰色；
* 当前证据**不支持增加删除线**，首版禁止擅自加 strike-through。

特别重要：`checked=true` 后 TodoRow **不能自己把自己 remove**。是否从 Today、Project 等列表退出由 Domain Command + Projection 更新决定。

## Title 规则

在没有足够长标题 A 级样本之前：collapsed 状态“最多显示几行”保持 PENDING，模型不能自行决定 1 行或 2 行作为最终规则。

## Golden

至少：

* normal short title
* normal long title 2 lines
* very long title
* completed short
* completed multi-line
* checkbox click isolation

每次修改 H03 必须回归 H02。

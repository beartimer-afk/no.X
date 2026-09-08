# H04｜TodoMetadata

TodoMetadata 负责 TodoRow 上的弱化辅助信息与右侧高优先级状态。它不是“一堆随便排的图标”，而是一套明确的槽位、优先级和压缩规则。

## H04-A｜容器模型

冻结三类槽：

```
TodoContent
├─ TitleLine
│  ├─ Title
│  └─ InlineSuffix       ← RepeatIndicator 等
├─ SecondaryLine         ← Parent / Reminder 等弱信息
└─ Trailing              ← Deadline 等右侧高优先级信息
```

核心原则：Deadline 等 Trailing 信息不能被长标题挤出屏幕；TitleLine 获取剩余可用宽度。

## H04-B1｜DeadlineMeta

真机 C 级样本：

* Deadline 整体约 **57×16 epx**；
* 旗帜约 **15×14 epx**；
* 视频编码后的粉红 Seed 约 `#F6164A`；
* 鸿蒙旗帜首轮 Seed 约 **12vp**。

DeadlineMeta：

* 固定于 trailing；
* 展示 Presenter 已计算好的文案（今天 / 日期 / overdue 等）；
* 组件本身不能计算日期语义；
* 不能在内部直接打开 Deadline Picker。

Overdue 精确颜色与文案未获得完整 A 级证据时保持 Pending。

## H04-B2｜RepeatIndicator

真机视觉约 **18–20 epx**，鸿蒙 Seed 约 **14vp**。

它是 TitleLine 的 `InlineSuffix`：紧跟标题，而不是固定靠右。

必须使用项目自有 repeat vector；禁止 Unicode `↻`、禁止拿系统 Refresh 图标替代。

它只展示 Presenter 给出的“此 Todo 与 Repeat Series/Copy 有关”的视觉状态，不承担 recurrence engine、template/copy 查询或规则编辑。

## H04-B3｜SecondaryMetadata

真机已经确认 Parent / Project 名称、Reminder 提示以**纯文字第二行**出现。

首版禁止为了“信息更清晰”添加文件夹、铃铛、Chip 或 Capsule。

规则：

* 与 Title 内容列左对齐；
* 视觉权重明显低于 Title；
* 首轮字号 Seed 约 **12fp**；
* Parent + Reminder 同时存在时谁优先，因为缺 A 级证据，保持 Pending，由 Presenter 策略层后续冻结。

## H04-C｜组合回归

单组件 PASS 不代表组合 PASS。需要建立组合 Fixture，例如：

```
标题：健身半小时
Repeat：有
Secondary：每日健身
Deadline：今天
```

组合后硬验收：

* Checkbox anchor 不变；
* Title anchor 不变；
* Repeat 与 Title gap 正确；
* Secondary 左边缘与 Title 对齐；
* Deadline 右边缘稳定；
* 长标题只能压缩自己的内容宽度，不能覆盖 trailing。

H04 只有在组合 Golden PASS 后才算 VERIFIED。

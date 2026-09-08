# H02｜TodoCheckbox

TodoCheckbox 是全 App 最重要的视觉锚点之一，会被 Today、Inbox、Upcoming、Anytime、Project、Area、Logbook 等大量场景复用。

## 真机 C 级测量

来源：用户原始 512×1108、30fps iPhone Things Light Mode 录屏。

测量 Seed：

* 未完成外轮廓约 **21×21 epx**；
* 完成态蓝色外框/填充约 **23×23 epx**；
* 白色勾主体约 **11×8 epx**；
* 编码后蓝色主样本约 `RGB(0,85,184)` / `#0055B8`；
* 灰色边框主样本约 `RGB(183,187,191)` / `#B7BBBF`。

这些是 C 级测量，不是设备原始 RGB，颜色必须继续通过 H00 Golden 校准。

## 鸿蒙首轮 Seed

视觉直径约 **18vp**，点击区域单独扩大到约 **48vp**。

视觉圆圈与 Hit Target 必须分离：不能为了 48vp 点击要求把圆圈本身画成 48vp。

## 实现约束

禁止直接使用 ArkUI 默认 Checkbox，因为系统组件样式可能随 HarmonyOS 版本变化，而且形态很难做到 Things 高保真。

应实现自有 `TodoCheckbox`：

```
TodoCheckbox
├─ HitTarget
└─ VisualCircle
   ├─ Border / Fill
   └─ Checkmark
```

Checkmark 使用项目自有 Vector/Path，不使用 Unicode 字符。

## 状态

首轮至少：

* open / unchecked
* completed / checked
* pressed
* selected interaction context 下仍由 Selection 组件单独表达，不能把“选中”混进 Todo 完成状态

## 事件边界

Checkbox 点击只发出 `onToggleCompletion(todoId)`；不允许同时触发 TodoRow 展开。

事件传播必须有测试：点击 Checkbox 时 Row 的 `onOpen` 次数必须为 0。

## Visual Lab

至少：

* normal unchecked
* completed checked
* pressed
* 放在白背景 / 轻灰背景中
* 与 TodoTitle baseline 联合展示

## Geometry Gate

检查视觉圆直径、中心位置、stroke、checkmark bbox、hit target bbox；视觉节点和点击节点分别设置 Stable ID。

## 禁止事项

* 禁止系统 Checkbox 默认样式；
* 禁止 Unicode `✓`；
* 禁止 literal 蓝色/灰色；
* 禁止 Checkbox 自己修改数据库；
* 禁止为了扩大点击区域放大视觉圆。

# H00｜Visual Acceptance Harness

H00 是鸿蒙复刻工程的第一任务。它先解决“**做出来以后如何证明做对了**”，再开始真正 UI 开发。

## 目标

禁止使用“AI 看截图感觉像”“人肉觉得差不多”作为验收标准。每一个组件和页面必须形成：

**Evidence → Token → Spec → Fixture → Implement → Simulator Screenshot → Inspector Geometry → Diff → Fix → VERIFIED**

## 固定验收环境

首版冻结一台 Canonical HarmonyOS Phone 模拟器：

* 设备类型、HarmonyOS / API 版本固定；
* Portrait；
* Light Mode；
* 系统语言与字体缩放固定；
* App Fixture 固定；
* Scroll offset / keyboard / overlay / selection state 固定。

Golden Screenshot 只能在这套环境生成。

## 为什么不能直接 iPhone 整屏 Pixel Diff

iPhone 与 HarmonyOS 存在系统栏、屏幕比例、系统字体栅格化与抗锯齿差异，因此不能用整屏像素完全相等作为目标。

验收拆成四层：

1. **Geometry Gate**：组件 x/y/width/height、baseline、anchor、间距硬验收；
2. **Token Gate**：颜色、字号、圆角、尺寸只能来自冻结 Token；
3. **Typography Gate**：字号、字重、行高、baseline 独立校验；
4. **Screenshot Gate**：内容区域归一化后做 Diff，文本区域与阴影使用合理容差，非文本几何区域要求更高精度。

## Stable ID

关键节点必须具备稳定 ID，例如：

```
todo_row.standard.root
todo_row.standard.checkbox
todo_row.standard.title
expanded_todo.standard.action_bar
```

ArkUI Inspector / 自动化测试以这些 ID 获取边界与属性。

## Visual Lab

工程必须包含仅开发环境可访问的 `DevVisualLab`：

```
Colors
Typography
Spacing
TodoCheckbox Gallery
TodoRow Gallery
TodoMetadata Gallery
ExpandedTodo Gallery
Checklist Gallery
Project / Area / Heading Gallery
Overlay Gallery
Selection Gallery
Drag Gallery
```

每个状态都由固定 Fixture 直接打开，不依赖人工先创建任务再点一长串设置。

## 页面 Golden

页面不能只有一张截图。例如 Today 至少应覆盖：

* empty
* standard
* long-content
* this-evening
* grouped
* todo-expanded
* selection
* scrolled

最终形成 100～200 张左右 Golden 是合理规模。

## 自动失败报告

理想报告格式：

```
TodoRow.normal
Geometry       PASS
row.height     FAIL +4vp
checkbox.x     PASS
title.baseline FAIL -1vp
Screenshot     96.4% / threshold 98%

Allowed modification:
TodoRow horizontal layout only

Do not modify:
Verified Checkbox / Colors / Typography
```

低能力模型收到的是约束明确的修复任务，不承担开放式审美判断。

## 组件锁定原则

组件通过 Fixture + Geometry + Screenshot + Interaction 后进入 `VERIFIED`。上层页面组装时禁止为了“页面好看一点”任意改 VERIFIED Primitive；如必须修改，需重新跑其全部 Regression。

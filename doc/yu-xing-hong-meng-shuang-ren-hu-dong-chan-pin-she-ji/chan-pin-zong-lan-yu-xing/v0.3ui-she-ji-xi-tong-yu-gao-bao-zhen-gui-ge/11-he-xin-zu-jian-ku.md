# 11｜核心组件库

## 原则

组件库不是为了复用而复用，而是为了保证整个 App 的气质不会在不同页面漂移。

## 1. Primary Button

用途：START、开始挑战、保存。

* 高 60vp；
* 最小宽 120vp；
* 页面主按钮通常占安全区全宽；
* Pill 圆角；
* 默认暖白背景 / 近黑文字；
* Press：scale 0.985 + opacity 0.92，120ms；
* Disabled：Surface 2 + Text Disabled。

一个页面最多一个 Primary Button。

## 2. Secondary Text Action

用途：换一个、Secret Match、Packs、取消。

* 15fp / 500；
* Text 2；
* 点击区最小 48vp；
* Press 只做亮度变化，不加底色矩形。

## 3. Core

Core 是品牌组件，不属于普通 Loading。

状态：

* Rest
* Breathing
* Hold
* Charged
* Fake Signal
* Tense
* Explosion
* Match Merge
* Transition Seed

每个状态必须在一个组件内部完成，不允许不同页面复制多个“差不多的圆”。

## 4. Challenge Card

* 宽：100% 安全区；
* 最大 430vp；
* 圆角 32vp；
* 背景 Surface 1 / 2；
* 1vp 以内微亮边；
* 内边距 24vp；
* 不默认阴影；
* 状态：Hidden / Reveal / Active / Completing / Leaving。

## 5. Choice Pill

Secret Match 使用。

* 高 52vp；
* 三个选择可纵向或横向，最小屏优先纵向；
* 未选：Surface 1；
* 选中：Accent Dim + 1vp Accent 边；
* 不用实心彩色背景。

## 6. Handoff Cover

全屏隐私组件：

* 内容彻底不可读；
* 背景使用黑 + 模糊；
* 中央小 Core；
* 长按进入；
* App 切后台时也可复用同一遮挡组件。

## 7. Bottom Sheet

只用于设置高级选项和确认危险操作。

* 顶部圆角 28vp；
* 最大高度 78% 屏幕；
* 背景 Surface 1；
* 支持拖拽关闭，但危险确认时拖拽不直接执行操作。

## 8. Toggle / Slider

遵循系统熟悉行为，不做赛博定制。只替换颜色 Token 和间距。

## 9. Toast

极少使用。游戏核心链路不允许 Toast 告知关键结果。

适用：

* 已保存；
* 已复制；
* 权限说明后的非阻断反馈。

## 10. Empty State

不用插画。小 Core / 图标 + 一句文案 + 一个动作即可。

## 组件验收

把组件单独摆在白纸上，如果仍显得“设计过度”，说明太花；把它放进游戏页，如果又消失得毫无个性，说明品牌性不足。目标是在两者之间。

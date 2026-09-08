# 19｜设计到开发交付约束

## 目标

禁止出现“逻辑都对，但开发自己顺手做了一个差不多的 UI”。

## 开发前必须具备

每个关键页面至少有：

1. 静态设计稿 / 目标截图；
2. Token 标注；
3. 组件引用；
4. 状态列表；
5. 进入 / 离开转场；
6. 触觉事件；
7. 声音事件；
8. 异常状态；
9. 验收截图名称。

缺一项，视为设计尚未完成，不应由开发者补猜。

## 代码约束

* 所有颜色从 Semantic Token 获取；
* 所有 spacing 从统一常量获取；
* Core 只能有一个正式实现；
* Primary Button / Challenge Card / Handoff Cover 必须组件化；
* 动效 duration / spring 参数集中管理；
* Haptic event 使用语义枚举 Tap / Lock / Fake / Burst；
* 禁止页面内散落 magic number。

## 截图文件命名

建议：

`UI_Home_Idle_390.png` `UI_Bomb_Calm_390.png` `UI_Bomb_Tense_390.png` `UI_Lose_Default_390.png` `UI_Challenge_Hidden_390.png` `UI_Challenge_Revealed_390.png` `UI_Secret_Handoff_390.png`

模拟器 / 真机自动截图统一按该命名与设计基线对比。

## 偏差处理

### 可接受

* 不同设备字体栅格导致 1vp 左右差异；
* 系统 Safe Area 变化；
* 由动态字体引起的可预期换行。

### 不可接受

* 开发自行换颜色；
* 自行减掉 Emotional Pause；
* 把自定义 Core 换成 Spinner；
* 把上推揭晓改成普通页面 Push；
* 为了开发方便显示倒计时 / Progress；
* 用系统默认 Alert 替代设计中的关键现场交互。

## UI Bug 优先级

### P0

隐私泄露、关键按钮不可点、Challenge 被遮挡、Explosion 严重掉帧。

### P1

主视觉尺寸错误、Core 状态不对、字体层级错误、触觉错位、关键转场缺失。

### P2

间距 2–4vp 偏差、低频图标不一致、非核心 Tool Page 小细节。

## Definition of Ready

一个页面只有在“静态 + 状态 + 动效 + 触觉 + 异常 + 验收”六类信息都存在后，才可以进入开发。

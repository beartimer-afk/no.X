# 05｜TASK-002：Home → Round Ready

## 目标

增加 Round Ready 静态页，并由 Index 根据 GameSession.state 在 Home/Ready 间切换。本任务不进入 Bomb。

## 依赖

TASK-001 PASS。

## 允许修改

pages/Index.ets pages/HomePage.ets pages/RoundReadyPage.ets components/NoxPrimaryButton.ets components/NoxBombCore.ets constants/CopyConstants.ets model/GameState.ets model/GameSession.ets

## Home → Ready

开始游戏点击：发送 HOME\_START\_TAP；记录 HAPTIC\_TAP 占位日志；state=ROUND\_READY。Index 做 220ms cross-fade：Home opacity 1→0，Ready 0→1。禁止系统 Router 和横向滑动。

## Round Ready 390×844

BackButton：X16；安全区底+8；命中44×44；点击 state=HOME。

ROUND 01：top88；居中；13fp COLOR\_TEXT\_MUTED。

准备开始：top142；居中；32fp。

真话会更热，当它伴随着风险。：标题下10；15fp COLOR\_TEXT\_SUB。

Core：中心(195,370)；视觉外径156vp；不可点击；静态。

### Ready Core 每层必须一致

从内到外：

* Center Dot：10×10vp 实心圆，COLOR\_TEXT\_MAIN，opacity0.92；
* Inner Disk：92×92vp 圆，填充 COLOR\_SURFACE opacity0.62；
* Primary Ring：132×132vp，stroke3vp，COLOR\_PURPLE opacity0.78；
* Pressure Ring：156×156vp，stroke4vp，COLOR\_PINK opacity0.74；
* Glow：视觉最大 206vp，COLOR\_PURPLE opacity0.18 + COLOR\_PINK opacity0.16；
* 禁止旋转、粒子、心形、HOLD 字样。

RuleLines top492：一个问题。 / 一个计时。 / 一个结果。；17fp；行间净距10；居中。

CTA：X32 Y692 W326 H56 Radius28；我准备好了；粉→紫渐变。

## Ready CTA

本任务只打印 ROUND\_READY\_CONFIRMED\_TASK002，不改 state，不生成 Bomb target。

## 禁止

系统 Router、3/2/1、玩家名、头像、进度条、Core 动画、进入 Bomb。

## 必交

TASK002\_round\_ready.png；TASK002\_home\_to\_ready.mp4。

## 验收

A01 Index 只按 state 切页面；A02 cross-fade；A03 文案一致；A04 Core 中心(195,370)、五层结构尺寸正确；A05 CTA(32,692,326,56)；A06 Ready 点击不进 Bomb；A07 Back 回 Home。

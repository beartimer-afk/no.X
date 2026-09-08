# 03｜Round Ready 回合准备实现合同

## 页面目标

只做一件事：**把两个人的注意力收回来，让当前回合有仪式感。** 不设置玩家、不解释规则、不选模式。

## 页面固定文案

顶部小字：`ROUND 01`

主标题：`准备开始`

副标题：`真话会更热，当它伴随着风险。`

说明三行：

`一个问题。`

`一个计时。`

`一个结果。`

CTA：`我准备好了`

## 布局

* 全屏 `bg.base`；
* 背后允许一层很弱的紫色雾化纹理，Opacity ≤ 0.18；
* `ROUND 01`：Y=88vp，水平居中，12sp / 500 / letter spacing +1.8vp；
* H1：Y=142vp，28sp；
* 副标题：H1 下 10vp；
* Core Ready：直径 138vp，中心点 Y=370vp；
* Core 中心只放一个 22vp 的小心形轮廓或 NO.X 点阵标记，禁止写 `HOLD`；
* 三行说明区域顶部 Y=492vp，行间 10vp；
* CTA：X=32，Y=692，326×56vp。

## Core Ready 动效

循环 2.4s：

* 0%：scale 0.985，Glow 0.68；
* 50%：scale 1.015，Glow 1.00；
* 100%：scale 0.985，Glow 0.68；
* easing：sine in-out；
* 禁止旋转；
* 禁止粒子持续喷射。

## CTA 行为

点击后 180ms 进入 Bomb Pressing 页面。

进入 Bomb 后，必须先展示 Prompt 和未按压 Core；不会自动开始计时，直到第一次有效 `pointerDown`。

## 返回

左上返回按钮 44×44vp；Tap 返回 Home；若本回合尚未开始，不弹确认框。

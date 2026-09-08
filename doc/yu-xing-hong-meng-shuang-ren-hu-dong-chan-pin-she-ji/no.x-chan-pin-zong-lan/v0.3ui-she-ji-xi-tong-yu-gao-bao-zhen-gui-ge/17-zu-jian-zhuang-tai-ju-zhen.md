# 17｜组件状态矩阵

## Primary Button

| 状态       | 背景          | 文字             | 动效                              |
| -------- | ----------- | -------------- | ------------------------------- |
| Default  | TEXT\_1     | BG\_0          | 无                               |
| Pressed  | TEXT\_1 92% | BG\_0          | scale .985                      |
| Disabled | SURFACE\_2  | TEXT\_DISABLED | 无                               |
| Loading  | TEXT\_1     | BG\_0          | 不用 Spinner，文案淡出 + 极小 Core pulse |

## Core

| 状态        | 呼吸       | Accent        | 漂移   | 触觉         |
| --------- | -------- | ------------- | ---- | ---------- |
| Rest      | 极慢       | 低             | 否    | 否          |
| Calm      | 2.6–3.2s | 低             | 极弱   | 启动一次       |
| Uneasy    | 1.8–2.4s | 中低            | 有    | 可选         |
| Tense     | .9–1.5s  | 中             | 有    | Fake       |
| Holding   | 暂停漂移     | 收束            | 指向触点 | Tap → Lock |
| Charged   | 慢回弹      | 中             | 否    | Lock       |
| Explosion | 停止循环     | 高 + EXPLOSION | 破裂   | Burst      |
| Match     | 慢        | 低             | 双环靠近 | Lock       |

## Challenge Card

| 状态         | 内容   | 高度        | 可操作    |
| ---------- | ---- | --------- | ------ |
| Hidden     | 无正文  | 180–220   | 上推揭晓   |
| Revealing  | 延迟显示 | 动态        | 禁止其它操作 |
| Active     | 完整   | 320+      | 主/次操作  |
| Recording  | 正文弱化 | 320+      | 停止/重录  |
| Completing | 正文保持 | scale .96 | 禁止重复点击 |
| Leaving    | 淡出   | 收缩        | 否      |

## Choice Pill

* Default：Surface 1；
* Press：亮度 +4%；
* Selected：Accent Dim + Accent Border；
* Focus / Accessibility：额外 2vp 外轮廓；
* Disabled：通常不出现。

## Text Input

* Rest：Surface 1；
* Focus：Accent 弱边 + 光标；
* Error：不大面积红边，只在下方显示短错误文案 + Explosion 色小标记；
* Filled：保持 Focus / Rest 规则。

## Handoff Cover

* Entering：内容 180ms Blur → Black；
* Locked：100% 不可读；
* Holding：小 Core 收束；
* Unlock：Lock haptic + 黑幕向 Core 收缩；
* Background Return：强制重新进入 Locked。

## Bottom Sheet

* Closed；
* Opening 320ms；
* Open；
* Dragging；
* Confirming；
* Closing 220ms。

任何组件新增状态必须先补此矩阵，再开发。

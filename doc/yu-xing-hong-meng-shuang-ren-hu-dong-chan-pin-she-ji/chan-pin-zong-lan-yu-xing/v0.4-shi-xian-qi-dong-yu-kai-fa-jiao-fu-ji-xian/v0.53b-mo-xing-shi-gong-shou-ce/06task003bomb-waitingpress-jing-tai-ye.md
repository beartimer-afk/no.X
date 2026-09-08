# 06｜TASK-003：Bomb WAITING\_PRESS 静态页

## 目标

实现 `BOMB_WAITING_PRESS` 静态状态。**不实现按压、爆炸、随机。**

## 依赖

TASK-002 PASS。

## 页面结构

1. 左上返回；
2. 顶中 `NO.X`；
3. 右上 `ROUND 1`；
4. 主标题 `按住并回答`；
5. Prompt Card；
6. Bomb Core；
7. 提示 `按住它，说出来，别松手。`；
8. 无底部主按钮。

## Prompt 固定测试文案

`说出你最想被亲吻的地方。`

禁止随机。

## Core

* 视觉直径 156vp；
* 触控命中 188vp；
* 两者同中心；
* 两层外环；
* 不旋转；
* 无百分比；
* 无真实倒计时数字。

## Prompt Card

* 宽=内容区 100%；
* 最小高 88vp；
* 圆角 18vp；
* 背景 `COLOR_SURFACE`；
* 边框 1vp `COLOR_BORDER`；
* 文案 17fp；
* 内边距 20vp；
* 无额外图标。

## 禁止

长按事件、Explosion、PASS、进度、Fake Signal、真实时间。

## 必交

`TASK003_bomb_waiting.png`

## 验收

A01 Core 是唯一主要交互对象；A02 156vp；A03 Prompt 不抢焦点；A04 无倒计时；A05 无传统炸弹图标。

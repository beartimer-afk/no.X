# 05｜TASK-002：Home → Round Ready

## 依赖

TASK-001 全 PASS。

## 目标

只增加导航和 Round Ready 静态页。

## Home 行为

点击 `开始游戏`：轻触觉一次，直接进入 `ROUND_READY`。禁止配置页、人数、姓名、加载页。

## Round Ready 固定结构

1. 返回按钮；
2. `ROUND 1`；
3. 标题 `准备开始`；
4. 副标题 `真话会更热，当它伴随着风险。`；
5. 中央静态 Bomb Core；
6. 三行：`一个问题。` / `一个计时。` / `一个结果。`；
7. 主按钮 `我准备好了`。

## Core

* 视觉直径 156vp；
* 不可点击；
* 粉紫双环；
* 无动画；
* 无卡通炸弹。

## 布局

返回触控 44vp；ROUND 1=13fp；标题 32fp；副标题 15fp；副标题到 Core 32vp；Core 到三行说明 24vp；按钮 56vp 高，左右 24vp。

## 按钮行为

本任务点击只打印 `ROUND_READY_CONFIRMED`，不进 Bomb。

## 禁止

3/2/1 倒计时、A/B、头像、进度条、启动炸弹。

## 必交

* `TASK002_round_ready.png`
* `TASK002_home_to_ready.mp4`

## 验收

A01 单次导航；A02 无多余功能；A03 Core 为视觉中心；A04 文案完全一致；A05 返回 Home 无状态残留。

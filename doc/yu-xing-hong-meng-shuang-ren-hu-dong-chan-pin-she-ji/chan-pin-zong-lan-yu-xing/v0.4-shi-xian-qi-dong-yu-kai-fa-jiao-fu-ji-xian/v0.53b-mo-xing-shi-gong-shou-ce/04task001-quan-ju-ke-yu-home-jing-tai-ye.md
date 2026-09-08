# 04｜TASK-001：全局壳与 Home 静态页

## 目标

只实现 Home 静态页面。本任务**不得进入游戏**。

## 允许修改文件

```
pages/HomePage.ets
components/NoxBrand.ets
components/NoxPrimaryButton.ets
components/NoxSecondaryButton.ets
constants/UiTokens.ets
constants/CopyConstants.ets
```

## 页面固定结构

从上到下：

1. 系统状态栏；
2. 顶部右 Settings 图标（本任务点击无动作）；
3. `NO.X`；
4. `两个人，一个未知。`；
5. 暗紫/黑成人暧昧背景图层或占位资源；
6. 主按钮 `开始游戏`；
7. 说明 `更大胆的问题，更真实的后果。`；
8. 底部弱入口：`玩法说明`、`设置`。

## 精确值

* 基准宽 390vp；
* 左右内容边距 24vp；
* Settings 触控 44×44vp，图标 20vp；
* 品牌距安全区底部 72vp；
* 品牌 44fp；
* 品牌与副标题 8vp；
* 主按钮高 56vp；
* 主按钮宽 = 内容区 100%；
* 主按钮圆角 28vp。

## 主按钮

背景：`COLOR_PINK → COLOR_PURPLE` 水平渐变。文字 17fp，`COLOR_TEXT_MAIN`。

## 禁止

TabBar、宫格、Secret Match、Pack、记录、动画、开始后导航、额外文案。

## 必交

* `TASK001_home_390.png`
* `TASK001_home_small.png`

## 验收

A01 无系统蓝；A02 无宫格；A03 NO.X 为第一焦点；A04 开始游戏为唯一强 CTA；A05 左右 24vp；A06 无额外入口；A07 小屏无截断。

# 04｜TASK-001：全局壳与 Home 静态页

## 目标

只实现 390×844vp 基准 Home 静态页，并保证冷启动直接看到 Home。本任务不进入游戏、不做响应式、不加载网络资源。

## 依赖

`TASK-000B PASS`。如果 App Icon 仍是默认图标或字体/资源冻结未通过，本任务不得开始。

## 本任务允许读取的上下文

只允许：`00 + 01 + 02 + 03 + 03A + 本 TASK`。禁止读取旧版本视觉规格自行折中。

## 允许修改

```
pages/Index.ets
pages/HomePage.ets
components/NoxBrand.ets
components/NoxPrimaryButton.ets
constants/UiTokens.ets
constants/CopyConstants.ets
```

禁止修改 AppScope 图标资源、字体、SVG 资源、app.json5。

## Index.ets

保持薄 Root：本任务只渲染 HomePage。禁止在 Index 复制 Home UI。开始按钮本任务只打印 `HOME_START_TAPPED_TASK001`。

## 背景精确结构

1. BackgroundBase：全屏 `COLOR_BG`。
2. PurpleBloom：280×280vp；left=220vp；top=-80vp；`COLOR_PURPLE_DEEP`；opacity=0.22；blur=72vp；不响应触控。
3. PinkBloom：240×240vp；left=-100vp；top=620vp；`COLOR_PINK`；opacity=0.12；blur=64vp；不响应触控。
4. 禁止网络图、AI 临时人物图、视觉探索截图切片、彩虹渐变。

## 固定结构

BackgroundBase / PurpleBloom / PinkBloom / BrandHero(`NO.X` + `两个人，一个未知。`) / PrimaryCTA / Subline / BottomSignature。

不显示 Settings、玩法说明、Tab、Pack。

## 390×844 坐标

BrandHero top=248vp；宽342vp；left24vp；内容居中。`NO.X` 44fp weight700 `COLOR_TEXT_MAIN`；副标题15fp `COLOR_TEXT_SUB`；间距8vp。

PrimaryCTA X32 Y548 W326 H56 Radius28；文案`开始游戏`；17fp weight600；`COLOR_PINK→COLOR_PURPLE` 水平渐变；无图标。按钮外发光一层：`COLOR_PINK` opacity0.26，blur18vp。

Subline top620：`更大胆的问题，更真实的后果。`；13fp `COLOR_TEXT_SUB`；单行居中。

BottomSignature top758：`靠近一点。`；13fp `COLOR_PINK_HOT` opacity0.70；不可点击。

## 字体

必须使用系统无衬线字体。禁止设置第三方 fontFamily、下载字体或引用字体文件。

## 禁止

TabBar、宫格、Secret Match、Pack、Settings、网络图片、动画、导航、额外文案、修改任何静态资源包文件。

## 必交

`TASK001_home_390.png`；Debug build PASS；FILES CHANGED。

## 验收

A01 冷启动直接 Home；A02 Index 无业务 UI；A03 坐标准确；A04 Bloom 一致；A05 无系统蓝；A06 NO.X 第一焦点；A07 开始游戏唯一强 CTA；A08 无额外入口；A09 文案逐字一致；A10 未新增字体/图片/第三方资源。

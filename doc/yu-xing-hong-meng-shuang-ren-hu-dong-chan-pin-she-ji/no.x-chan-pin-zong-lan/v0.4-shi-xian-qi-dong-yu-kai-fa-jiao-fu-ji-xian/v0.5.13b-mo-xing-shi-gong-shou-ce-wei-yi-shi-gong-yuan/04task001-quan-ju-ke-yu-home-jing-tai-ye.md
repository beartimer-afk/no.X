# 04｜TASK-001：全局壳与 Home 静态页

## 目标

只实现 390×844vp 基准 Home 静态页，并保证 App 冷启动后直接看到该 Home。本任务不进入游戏、不做响应式、不加载网络资源。

## 依赖

PRECHECK PASS。

## 允许修改

pages/Index.ets pages/HomePage.ets components/NoxBrand.ets components/NoxPrimaryButton.ets constants/UiTokens.ets constants/CopyConstants.ets

## Index.ets

保持薄 Root：本任务只渲染 HomePage。禁止在 Index 复制 Home UI。开始按钮回调本任务只打印 HOME\_START\_TAPPED\_TASK001。

## 背景精确结构

1. BackgroundBase：全屏 COLOR\_BG。
2. PurpleBloom：椭圆 280×280vp；left=220vp；top=-80vp；填充 COLOR\_PURPLE\_DEEP；opacity=0.22；blur radius=72vp；不响应触控。
3. PinkBloom：椭圆 240×240vp；left=-100vp；top=620vp；填充 COLOR\_PINK；opacity=0.12；blur radius=64vp；不响应触控。
4. 禁止网络图、AI 临时人物图、系统蓝、彩虹渐变。

## 固定结构

BackgroundBase / PurpleBloom / PinkBloom / BrandHero(NO.X + 两个人，一个未知。) / PrimaryCTA / Subline / BottomSignature。

不显示 Settings、玩法说明、Tab、Pack。

## 390×844 坐标

BrandHero top=248vp；宽=342vp；left=24vp；水平内容居中。NO.X 44fp weight700 COLOR\_TEXT\_MAIN；副标题 15fp COLOR\_TEXT\_SUB；间距8vp。

PrimaryCTA X32 Y548 W326 H56 Radius28；文案 开始游戏；17fp weight600；COLOR\_PINK→COLOR\_PURPLE 水平渐变；无图标。按钮外发光只允许一层：COLOR\_PINK opacity0.26，blur radius18vp，Y offset 0。

Subline top620：更大胆的问题，更真实的后果。；13fp COLOR\_TEXT\_SUB；单行居中。

BottomSignature top758：靠近一点。；13fp COLOR\_PINK\_HOT opacity0.70；不可点击。

## 字体

v0.1 不引入外部字体文件。全部使用系统无衬线字体；只按本文 weight/size 区分。禁止 3B 下载字体或自行选择装饰字体。

## 禁止

TabBar、宫格、Secret Match、Pack、Settings、网络图片、动画、导航、额外文案。

## 必交

TASK001\_home\_390.png；Debug build PASS；FILES CHANGED。

## 验收

A01 冷启动直接 Home；A02 Index 无业务 UI；A03 固定坐标准确；A04 两个 Bloom 尺寸/位置一致；A05 无系统蓝；A06 NO.X 第一焦点；A07 开始游戏唯一强 CTA；A08 无额外入口；A09 文案逐字一致。

# 25｜视觉资源清单：没有资源时禁止模型自创低质替代

## 目标

区分“必须代码绘制的 UI”与“需要外部视觉资产的部分”，避免 3B 模型自行找图、画嘴唇、画低质人物。

## 代码必须绘制

以下不得使用整张 PNG 截图冒充 UI：

* Primary/Secondary Button；
* Prompt Card；
* Challenge Card；
* Bomb Core 基础圆环；
* Core glow；
* Explosion 几何光粒；
* 所有文本；
* TopBar 图标；
* 页面背景色/暗紫雾层。

## 外部资产可选

```
assets/home_hero.webp
assets/lose_hero.webp
assets/challenge_bg.webp
```

用途：仅作为低亮度成人暧昧氛围背景。

## 资产缺失时唯一降级策略

如果上述外部图片不存在：

* Home 使用 `COLOR_BG` + 深紫径向雾层；
* Lose 使用 `COLOR_BG` + 深紫残光；
* Challenge 使用纯暗背景；
* **不得自己生成临时 AI 人物图塞进去。**

## 图片显示规则

* `objectFit=cover`；
* 全屏图必须有深色 Overlay；
* 背景人物不得导致主文案对比不足；
* 不做随机背景；
* 不下载网络图片。

## 品牌标识

NO.X 首版使用文字排版，不需要复杂 Logo 图文件。圆形核心可在 O 内部视觉中呼应，但不得因为 Logo 未定自行做图标系统。

## 图标

只使用统一线性图标风格：Back / Close / Settings / Heart / Flame。

如果系统图标风格与视觉冲突，允许用简单矢量 Path；不得混用 3 种不同图标库。

## 禁止

* 从互联网抓成人图片；
* 使用水印图；
* 使用视觉附录整张截图切片进 App；
* 用图片替代按钮文字；
* 模型自行设计新 Logo。

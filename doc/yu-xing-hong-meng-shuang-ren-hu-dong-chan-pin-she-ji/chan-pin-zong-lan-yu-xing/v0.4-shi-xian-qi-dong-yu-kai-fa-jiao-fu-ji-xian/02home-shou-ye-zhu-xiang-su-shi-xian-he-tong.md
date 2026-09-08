# 02｜Home 首页逐像素实现合同

基准：390×844vp。

## 层级结构（从后向前）

1. `BackgroundPhoto`：全屏裁切 Cover；
2. `BlackVeil`：全屏 `rgba(4,0,6,0.58)`；
3. `PurpleBloom`：右上 / 左下弱紫光；
4. `Header`；
5. `BrandLockup`；
6. `PrimaryCTA`；
7. `Subline`。

{% hint style="warning" %}
如果正式摄影资源尚未提供，必须使用 `bg.base + bg.deepPurple` 固定渐变占位。**禁止开发 AI 自行找图、生成图或替换成人摄影。**
{% endhint %}

## 坐标与尺寸

### Header

* 顶部：安全区后 16vp，即 Y≈60；
* 左侧 24vp；
* `NO.X` logo 宽 84vp，高度按资产比例；
* 右侧仅允许一个 Settings icon，24×24vp，触控区 44×44vp；
* 首个试玩版本如果 Settings 未实现，右侧图标直接隐藏，不保留空按钮。

### Brand Hero

* 品牌 Hero 容器顶部：Y=248vp；
* Logo 大尺寸：宽 176vp；
* 水平居中；
* Logo 下间距：10vp；
* 副标题固定：`两个人，一个未知。`；
* 16sp / 500 / text.primary；
* 居中。

### Primary CTA

* X=32vp；
* 宽=326vp；
* 高=56vp；
* Y=548vp；
* 圆角=28vp；
* 文案：`开始游戏`；
* 18sp / 600；
* 不显示右箭头；
* Tap scale：1.00 → 0.975 → 1.00。

### CTA 下方文案

Y=620vp；

`更大胆的问题，更真实的后果。`

* 13sp；
* text.secondary；
* 居中；
* 单行；
* 不允许换行。

### 底部署名

Y≈758vp：

`更胜一点，靠近一点。 ♡`

* 14sp；
* 允许使用斜体效果；
* 颜色 `accent.hotPink`，透明度 0.78；
* 仅装饰，不可点击。

## 交互

点击 `开始游戏`：

* 立即触觉 `Tap`；
* 0–90ms：按钮缩小到 0.975；
* 90–220ms：Home 内容整体 opacity 1→0；
* 同时 Ready 页面 opacity 0→1、Y 10→0；
* 总过渡 220ms；
* 禁止系统默认路由滑动动画。

## 验收零容忍

* 首页不能出现底部 Tab Bar；
* 不能出现功能宫格；
* 不能出现“秘密匹配 / Pack / 记录”等入口；
* 不允许把 CTA 改成普通实色矩形；
* Logo 不能使用普通文本近似。

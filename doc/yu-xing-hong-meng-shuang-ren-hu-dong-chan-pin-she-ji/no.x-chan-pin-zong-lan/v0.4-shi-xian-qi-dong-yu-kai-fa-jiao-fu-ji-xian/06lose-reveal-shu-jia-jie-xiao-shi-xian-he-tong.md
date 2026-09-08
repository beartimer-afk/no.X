# 06｜Lose Reveal 输家揭晓实现合同

## 固定文案

主标题：`是你。`

副文案：`你没能按住它。`

辅助文案：`有些风险，值得冒。`

按钮：`继续`

## 页面布局

* 页面继承 Explosion 后的暗紫背景；
* 如有指定 Lose 摄影资源，全屏 Cover；
* 摄影资源上叠 `veil.black 0.62`；
* 主标题中心 Y=390vp；
* 副文案距主标题 18vp；
* 辅助文案距副文案 36vp；
* CTA X=44，W=302，H=52，Y=650vp；
* CTA 使用 Secondary 样式：透明暗紫底 + 粉色 1vp 描边，不使用 Primary Gradient。

## 节奏

* `是你。` 出现后 350ms 才允许 CTA 接收点击；
* CTA 允许立即显示但前 350ms disabled；
* disabled 时 opacity=0.45；
* 350ms 后恢复 1.0；
* 不显示倒计时；
* 不自动跳 Challenge。

## 点击继续

* Tap haptic；
* 180ms fade；
* Challenge Card 从下方 Y=24→0 + opacity 0→1；
* 总时长 260ms。

## 禁止

* 禁止 `你输了！` 红色大字；
* 禁止奖杯 / emoji / 彩纸；
* 禁止“胆小鬼”等羞辱文案；
* 禁止同时展示 Challenge 内容。

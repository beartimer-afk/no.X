# 08｜组件状态、按钮、图标与触控合同

## Primary Button

尺寸：

* 默认宽：父容器可用宽；
* 高：56vp；
* Radius：28vp。

状态：

| 状态       | Scale | Opacity | Glow |
| -------- | ----: | ------: | ---: |
| Idle     | 1.000 |    1.00 | 1.00 |
| Pressed  | 0.975 |    1.00 | 1.18 |
| Disabled | 1.000 |    0.42 | 0.30 |

Pressed 动画：90ms in，110ms out。

禁止 spinner 占据整个按钮；如未来需要 Loading，保留文案并在右侧放 16vp spinner。

## Secondary Button

* H=52vp；
* 背景 `rgba(27,10,33,0.72)`；
* Border 1vp `line.strong`；
* 文字 `text.primary`；
* Pressed 背景亮度 +8%。

## Icon Button

* 视觉图标 22–24vp；
* 命中区固定 44×44vp；
* 不画圆形底，除非在 Bottom Sheet；
* 所有 icon 必须是线性风格，Stroke 1.8–2.0vp；
* 禁止混用 Emoji。

## Core Press Area

* 视觉直径 156vp；
* 命中直径 188vp；
* `pointerDown` 必须发生在命中区；
* 按下后允许手指漂移至视觉圆心外 22vp 仍保持有效；
* 超出有效区域连续 120ms，视为松手；
* 系统滑动手势不得抢占 Core 按压。

## Touch Slop

* Buttons：8vp；
* Core：22vp；
* Back：10vp。

## Bottom Sheet

仅允许用于“结束这一轮 / 结束 Session”确认。

* 顶部圆角 26vp；
* 背景 `#16071B`；
* 不使用系统默认白底；
* 高度按内容，首版约 252vp；
* 遮罩 `rgba(0,0,0,0.64)`；
* 出现：260ms cubic-bezier(0.2,0.8,0.2,1)。

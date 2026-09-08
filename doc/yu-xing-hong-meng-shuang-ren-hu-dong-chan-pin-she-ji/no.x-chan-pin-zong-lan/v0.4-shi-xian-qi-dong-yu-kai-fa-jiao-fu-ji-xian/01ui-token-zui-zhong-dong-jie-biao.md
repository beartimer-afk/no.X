# 01｜UI Token 最终冻结表

{% hint style="warning" %}
所有页面必须引用统一 Token。禁止页面内写临时颜色、临时圆角、临时字号。
{% endhint %}

## Color Tokens

| Token            | 值                       | 用途                |
| ---------------- | ----------------------- | ----------------- |
| `bg.base`        | `#080309`               | 全局背景              |
| `bg.deepPurple`  | `#15051A`               | 暗紫环境层             |
| `surface.1`      | `#1B0A21`               | 卡片 / 输入块          |
| `surface.2`      | `#26102D`               | 强调卡片              |
| `text.primary`   | `#FFF5FC`               | 主文字               |
| `text.secondary` | `#D6B9D2`               | 次级文字              |
| `text.tertiary`  | `#9E7899`               | 说明文字              |
| `accent.pink`    | `#FF2FAE`               | 主霓虹粉              |
| `accent.hotPink` | `#FF5BCB`               | 高亮粉               |
| `accent.purple`  | `#A855F7`               | 主紫                |
| `accent.violet`  | `#7C3AED`               | 深紫                |
| `accent.danger`  | `#FF3F78`               | Tense / Explosion |
| `line.soft`      | `rgba(255,91,203,0.24)` | 细描边               |
| `line.strong`    | `rgba(255,91,203,0.62)` | 强描边               |
| `veil.black`     | `rgba(4,0,6,0.72)`      | 背景压暗              |

### 禁止颜色

* `#007DFF` Harmony 默认蓝；
* `#00C853` / 绿色成功态；
* Cyan / Electric Blue 作为主交互色；
* 金色 / 琥珀色作为主主题色。

## Gradient Tokens

### Primary Button

`linear-gradient(100deg, #FF2FAE 0%, #C146FF 58%, #FF4F88 100%)`

### Core Calm Glow

`radial-gradient(circle, rgba(255,91,203,0.92) 0%, rgba(168,85,247,0.54) 36%, rgba(168,85,247,0.00) 72%)`

### Core Tense Glow

`radial-gradient(circle, rgba(255,245,252,1.0) 0%, rgba(255,47,174,0.96) 20%, rgba(168,85,247,0.76) 48%, rgba(255,63,120,0.00) 78%)`

## Typography

品牌 `NO.X`：**必须使用设计资产（SVG / Path）**，禁止开发者自行挑字体重打 Logo。

中文正文统一 HarmonyOS Sans SC：

| 名称        |   字号 | Weight |   行高 |
| --------- | ---: | -----: | ---: |
| Hero      | 38sp |    700 | 46sp |
| H1        | 28sp |    700 | 36sp |
| H2        | 22sp |    700 | 30sp |
| Title     | 18sp |    600 | 26sp |
| Body      | 16sp |    500 | 24sp |
| Secondary | 14sp |    400 | 21sp |
| Caption   | 12sp |    400 | 18sp |
| Micro     | 10sp |    500 | 14sp |

禁止：全页使用细体、艺术字体写正文、英文 Serif 替代中文系统字体。

## Radius

* Primary Button：`26vp`
* Secondary Button：`24vp`
* Challenge Card：`22vp`
* Prompt Card：`18vp`
* Small Chip：`14vp`
* Core：圆形，`50%`

## Border

* 常规卡片：1vp `line.soft`
* 高亮卡片：1vp `line.strong`
* Primary Button 不画硬边框，靠 Glow 分离。

## Shadow / Glow

Primary Button：

* 外发光：`0 0 18vp rgba(255,47,174,0.48)`
* 第二层：`0 0 36vp rgba(168,85,247,0.20)`

Core Calm：

* `0 0 26vp rgba(255,47,174,0.40)`
* `0 0 52vp rgba(168,85,247,0.22)`

Core Tense：

* `0 0 32vp rgba(255,47,174,0.68)`
* `0 0 64vp rgba(168,85,247,0.38)`

禁止使用黑色 Material Elevation 阴影模拟层级。

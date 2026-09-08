# 04｜Bomb 按压核心：规则、布局与手感合同

这是 NO.X 首版最重要的页面。**任何技术取舍都必须优先保证按压手感。**

## 1. 页面固定结构

顶部：

* 左：返回；
* 中：`ROUND 01`；
* 右：无内容。

主标题：`按住并回答`

Prompt Card：示例固定文本：`说出你最想被亲吻的地方。`

中部：可按压圆形 Core。

底部说明：`说出来，按住它，别松手。`

不显示：

* 真实剩余时间；
* 爆炸概率；
* 倒计时数字；
* 百分比；
* 玩家名。

## 2. 坐标

基准 390×844vp：

* 顶栏 Y=52–96；
* 标题 top=118vp；
* Prompt Card：X=42，Y=174，W=306，H=96；
* Core 中心：X=195，Y=424；
* Core 视觉直径：156vp；
* Core 实际命中区：188×188vp；
* 底部说明 Baseline Y≈674；
* Handoff 提示区域 Y≈716。

## 3. Core 结构

从内到外必须是：

1. 中心发光点 / 心形轮廓：26vp；
2. Inner Disk：直径 92vp；
3. Primary Ring：直径 132vp，Stroke 3vp；
4. Pressure Ring：直径 156vp，Stroke 4vp；
5. 外部 Glow：最大视觉直径约 206vp。

不得增加实体炸弹保险丝、炸药、卡通 Bomb body。

## 4. pointerDown

手指进入 Core 命中区并按下：

* 0ms：记录当前 Hold Session；
* 0–80ms：Core scale `1.0 → 0.965`；
* 0–120ms：Primary Ring 亮度 +25%；
* 80ms：触觉 `Lock`；
* 120–300ms：Pressure Ring 从 0.72 opacity → 1.0；
* Prompt Card 不动；
* 页面标题不变。

## 5. 最短合法按压

* `MIN_HOLD = 1100ms`；
* 1100ms 前松手视为 Early Release；
* 1100ms 后松手视为 Pass Intent。

### Early Release

如果 `<1100ms` 松手：

* 不切页面；
* Core 100ms 弹回 scale=1；
* 触觉：`Error` 一次；
* 底部说明临时替换为 `别急，还不能放。`；
* 900ms 后恢复原文；
* 不重置全局 Bomb 计时。

## 6. 全局隐藏爆炸时间

每个 Round 在第一次按下前生成一次：

`deadline = Uniform(14.0s, 30.0s)`

只累计**有效按压时长**，不累计页面过渡和 Handoff 时间。

* 每次合法松手后暂停累计；
* 下一位再次按下后继续累计；
* Round 内不得重新生成 deadline；
* 用户不可见；
* Debug Build 可通过日志输出，Release 禁止显示。

## 7. 每次接手保护

每次新 `pointerDown` 的前 400ms，爆炸事件如到期则延迟到 `pointerDown + 400ms` 触发，避免刚碰屏幕瞬间爆炸导致体验像 Bug。

## 8. 合法松手 / Handoff

`holdDuration >=1100ms` 且未爆炸时松手：

* 0ms：触觉 `Tap`；
* 0–100ms：Core Glow 降至 55%；
* 100–180ms：屏幕中央出现覆盖文案 `传给对方`；
* Handoff Overlay 持续 1300ms；
* Overlay 全屏背景 `rgba(8,3,9,0.92)`；
* 中间只显示 `传给对方`，22sp；
* 下方 14sp：`别让 TA 等太久。`；
* 1300ms 后回到同一 Bomb 页面等待下一次按下；
* Bomb 累计时间在 Overlay 期间暂停。

禁止在 Handoff Overlay 上加按钮。

## 9. 回答内容

首个实现版本**不做语音识别、不做麦克风检测**。Prompt 是现场互动规则，由两个人自己判断是否回答完。App 只负责按压和风险。

## 10. 返回 / 中断

Round 开始后点击返回：

必须弹出自定义确认 Bottom Sheet：

`结束这一轮？`

`当前进度不会保留。`

按钮：`继续玩` / `结束`

禁止使用系统 AlertDialog 默认样式。

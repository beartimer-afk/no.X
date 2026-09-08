# 16｜关键页面逐屏布局标注

> 基准画板：390vp 宽手机竖屏。高度动态，以约 844vp 作为设计检查基准。所有顶部 / 底部安全区使用系统 Insets，不写死状态栏高度。

## Home

### Horizontal

* 页面左右边距：20vp；
* 内容最大宽：350vp；
* Core 中心 X：195vp。

### Vertical

安全区后：

* Top bar 高 56vp；
* Logo baseline：Top bar 中心；
* Core 视觉中心：可用内容区约 42% 高度；
* Core 玻璃壳：196vp；
* 外光晕：276vp；
* Core 到主按钮顶部最小距离：72vp；
* START：高 60vp；
* START 到 Secondary actions：16vp；
* Secondary actions 到底部安全区：24vp。

视觉原则：底部操作不能把 Core 推到上半屏像“Banner + CTA”。

## Round Ready

* Top bar 保持 56vp；
* Round Label 距 safe top + 20vp；
* Core 中心与 Bomb 页面完全相同，保证连续性；
* Ready 文案 / 脉冲直接围绕 Core，不新增居中弹窗。

## Bomb

* Top bar：56vp；
* Core 玻璃壳：196vp；
* Touch target：220vp；
* Core 中心 Y：可用区 43–45%；
* 指令文字顶部距离 Core 壳：32vp；
* 指令文字最大宽：280vp；
* 底部辅助区：高 72vp，仅弱提示。

## Lose Reveal

Explosion 后：

* 主文案中心约位于可用区 46%；
* `是你。` 36–42fp；
* 辅助文案距离主文案 16vp；
* 不显示 Card 边界；
* 屏幕至少 60% 区域保持纯背景留白。

## Hidden Challenge

* Card 宽：350vp；
* 初始高度：180–220vp；
* Center X：195vp；
* Bottom 距安全区：可根据屏高动态，但不低于 40vp；
* `你的挑战` 置于卡片上方 20vp 或卡片内部顶部，二选一后全局统一。

## Revealed Challenge

* Card 宽：350vp；
* 最小高：320vp；
* 最大高：可用高度 - 120vp；
* 内边距：24vp；
* Tag 行高：18vp；
* Tag 到正文：20vp；
* 正文到操作区：至少 32vp；
* Primary Action 高：56vp；
* Secondary Action 点击区：48vp。

短正文时不要让按钮紧贴文字；空白属于设计。

## Secret Match Handoff

* 小 Core：96vp；
* 中心 Y：可用区 42%；
* 主文案距 Core 32vp；
* 辅助说明距主文案 12vp；
* 长按区域在屏幕底部 72–96vp，但视觉可仅为文字 / 薄 Pill。

## Secret Match Choice

* Progress：safe top + 20vp；
* 题目容器中心约 38–42%；
* 题目最大宽 320vp；
* 三选择区底部对齐 safe bottom + 20vp；
* Horizontal 模式按钮间距 8vp；若任意标签拥挤立即切 Vertical。

## Match Result

* 重合双 Core：128–160vp；
* 数字：48fp / 650；
* 文案距数字 8–12vp；
* START GAME 主按钮沿用 Home 尺寸。

## Editor

* 导航栏：56vp；
* 正文编辑 Surface：页面边距 20vp，最小高 144vp；
* Advanced rows：每行 56vp；
* 保存按钮优先底部固定，键盘出现时上移，不能被键盘遮挡。

## Settings

* 顶部标题区 64vp；
* Group 间距 28–32vp；
* Row 最小高 56vp；
* 左右边距 20vp；
* 使用系统安全区与滚动行为。

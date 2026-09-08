# 02｜全局 Token：开发只能读取，禁止自行设值

## 使用优先级

**当前 TASK 写了精确尺寸/坐标/间距时，必须使用 TASK 的值。** 本页 Token 只用于当前 TASK 未单独定义的属性。

因此例如 TASK 明确写 `X=32vp`、`间距=10vp` 时，不与本页默认 24vp 页面边距或基础间距表冲突，也不需要报告 SPEC\_GAP。

禁止反过来用通用 Token 覆盖 TASK 精确坐标。

## 固定颜色

COLOR\_BG = #080309 COLOR\_BG\_ELEVATED = #120716 COLOR\_SURFACE = #1A0B20 COLOR\_PINK = #FF2FAE COLOR\_PINK\_HOT = #FF4FBE COLOR\_PURPLE = #A855F7 COLOR\_PURPLE\_DEEP = #7C3AED COLOR\_TEXT\_MAIN = #FFF6FC COLOR\_TEXT\_SUB = #CDB7C9 COLOR\_TEXT\_MUTED = #8D7489 COLOR\_BORDER = #4B244B COLOR\_DANGER = #FF2F68 COLOR\_WHITE\_FLASH = #FFFFFF

## 基础间距

当 TASK 没有写精确间距时，只能从 `4 / 8 / 12 / 16 / 20 / 24 / 32 / 40vp` 中选择。

当 TASK 已明确写 10vp、18vp 等具体值时，严格执行 TASK 值，不得改成最近的基础间距。

## 固定圆角

RADIUS\_SM = 12vp RADIUS\_MD = 18vp RADIUS\_LG = 24vp RADIUS\_BUTTON = 28vp RADIUS\_CARD = 24vp RADIUS\_CORE = 999vp

## 固定字号

TYPE\_BRAND = 44fp TYPE\_H1 = 32fp TYPE\_H2 = 26fp TYPE\_BODY = 17fp TYPE\_BODY\_SM = 15fp TYPE\_CAPTION = 13fp TYPE\_MICRO = 11fp TYPE\_BUTTON = 17fp

当前 TASK 如果为某个元素明确给出字号，使用 TASK 值。

## 默认页面边距

只有 TASK 没给出 X/W 时，默认左右内容边距 24vp。TASK 明确给出 `X=32vp` 等坐标时，使用 TASK 坐标。

顶部默认：系统安全区底部 + 16vp；底部默认：安全区顶部向上 16vp。若 TASK 有明确 Y/top/bottom，则 TASK 优先。

## 霓虹规则

Primary Button：粉色主发光 + 紫色次晕染。 Bomb Core：粉紫双层。 普通文字禁止发光。 一屏最多两个强发光焦点。

## 禁止

系统蓝、彩虹渐变、橙色主按钮、白底、自动浅色模式、页面内重新定义品牌色字面量。

## 验收

品牌色字面量只允许定义在 `UiTokens.ets`；页面只能引用 Token。TASK 精确布局值允许直接作为本任务页面常量，但不得建立第二套全局 Token。

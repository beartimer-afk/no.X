# 02｜全局 Token：开发只能读取，禁止自行设值

## 固定颜色

```
COLOR_BG          = #080309
COLOR_BG_ELEVATED = #120716
COLOR_SURFACE     = #1A0B20
COLOR_PINK        = #FF2FAE
COLOR_PINK_HOT    = #FF4FBE
COLOR_PURPLE      = #A855F7
COLOR_PURPLE_DEEP = #7C3AED
COLOR_TEXT_MAIN   = #FFF6FC
COLOR_TEXT_SUB    = #CDB7C9
COLOR_TEXT_MUTED  = #8D7489
COLOR_BORDER      = #4B244B
COLOR_DANGER      = #FF2F68
COLOR_WHITE_FLASH = #FFFFFF
```

## 固定间距

`4 / 8 / 12 / 16 / 20 / 24 / 32 / 40vp`。除系统安全区计算外禁止出现任意新间距值。

## 固定圆角

```
RADIUS_SM = 12vp
RADIUS_MD = 18vp
RADIUS_LG = 24vp
RADIUS_BUTTON = 28vp
RADIUS_CARD = 24vp
RADIUS_CORE = 999vp
```

## 固定字号

```
TYPE_BRAND = 44fp
TYPE_H1 = 32fp
TYPE_H2 = 26fp
TYPE_BODY = 17fp
TYPE_BODY_SM = 15fp
TYPE_CAPTION = 13fp
TYPE_MICRO = 11fp
TYPE_BUTTON = 17fp
```

## 固定页面边距

* 左右内容边距：24vp；
* 顶部：系统安全区底部 + 16vp；
* 底部按钮：安全区顶部向上 16vp。

## 霓虹规则

* Primary Button：粉色主发光 + 紫色次晕染；
* Bomb Core：粉紫双层；
* 普通文字禁止发光；
* 一屏最多两个强发光焦点。

## 禁止

系统蓝、彩虹渐变、橙色主按钮、白底、自动浅色模式。

## 验收

品牌色字面量只允许出现在 `UiTokens.ets`。

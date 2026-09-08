# 23｜页面组件树：ArkUI 层级禁止自行重排

本页把主要页面拆成固定组件树。开发模型不得自行把组件层级合并成“更简洁”的结构，也不得将页面改成 List/Grid。

## HomePage

```
Stack(full screen)
├── BackgroundLayer
│   ├── SolidColor(COLOR_BG)
│   ├── OptionalVisualAsset(full bleed, low opacity)
│   └── DarkOverlay
└── Column(full screen, padding L/R 24vp)
    ├── TopBar(height 44vp)
    │   └── SettingsIconButton(44x44, align right)
    ├── Spacer(flexible)
    ├── NoxBrand
    │   ├── Text("NO.X", 44fp)
    │   └── Text("两个人，一个未知。", 15fp)
    ├── Spacer(flexible, smaller than upper spacer)
    ├── NoxPrimaryButton("开始游戏", height 56vp)
    ├── Text("更大胆的问题，更真实的后果。", 13fp)
    ├── Row weak links
    │   ├── TextButton("玩法说明")
    │   └── TextButton("设置")
    └── BottomSafeSpacer(16vp)
```

## RoundReadyPage

```
Stack
├── BackgroundLayer
└── Column(padding 24vp)
    ├── TopBar
    │   ├── BackButton(44x44)
    │   └── Text("ROUND N", 13fp)
    ├── Text("准备开始", 32fp)
    ├── Text("真话会更热，当它伴随着风险。", 15fp)
    ├── Spacer(32vp)
    ├── NoxBombCore(mode=static, visual=156vp)
    ├── Spacer(24vp)
    ├── Column info lines
    │   ├── Text("一个问题。")
    │   ├── Text("一个计时。")
    │   └── Text("一个结果。")
    ├── Spacer(flexible)
    ├── NoxPrimaryButton("我准备好了", 56vp)
    └── BottomSafeSpacer
```

## BombPage

```
Stack
├── BackgroundLayer
│   ├── COLOR_BG
│   └── TensionAtmosphereLayer (visual only)
└── Column(padding 24vp)
    ├── TopBar
    │   ├── BackButton(44x44)
    │   ├── Text("NO.X", 15fp)
    │   └── Text("ROUND N", 13fp)
    ├── Text(dynamicTitle, 32fp)
    ├── NoxPromptCard
    ├── Spacer(flexible)
    ├── Stack(width=188vp,height=188vp)
    │   ├── InvisibleHitCircle(188vp)
    │   └── NoxBombCore(156vp)
    ├── Spacer(20vp)
    ├── Text(dynamicInstruction, 15fp)
    └── BottomSafeSpacer
```

**不得把 Prompt 放到 Core 下方。不得把 Core 变成全屏按钮。**

## LoseRevealPage

```
Stack
├── BackgroundLayer
└── Column(padding 24vp)
    ├── Text("NO.X",15fp)
    ├── Spacer(flexible)
    ├── Text("是你。",44fp)
    ├── Text("你没能按住它。",17fp)
    ├── Text("有些风险，值得冒。",15fp)
    ├── Spacer(flexible)
    ├── NoxPrimaryButton("查看挑战",56vp)
    └── BottomSafeSpacer
```

## ChallengeRevealPage

```
Stack
├── BackgroundLayer
└── Column(padding 24vp)
    ├── TopBar
    │   ├── CloseButton(44x44)
    │   ├── Text("NO.X",15fp)
    │   └── Text("i / total",13fp)
    ├── Text("你的挑战",32fp)
    ├── Spacer(20vp)
    ├── NoxChallengeCard(minHeight=280vp)
    ├── Spacer(flexible)
    ├── NoxPrimaryButton(dynamic,56vp)
    ├── NoxSecondaryButton("下一张",56vp)
    ├── Text("更大胆的问题，更亲密的连接。",13fp)
    └── BottomSafeSpacer
```

## 禁止

* 禁止用 Grid；
* 禁止用底部 Tab；
* 禁止把 TopBar 做成系统 NavigationBar 导致风格不可控；
* 禁止自动压缩 Core 以适配文本；小屏优先压缩垂直 Spacer；
* 禁止把 Challenge Card 放入可横滑 Carousel。

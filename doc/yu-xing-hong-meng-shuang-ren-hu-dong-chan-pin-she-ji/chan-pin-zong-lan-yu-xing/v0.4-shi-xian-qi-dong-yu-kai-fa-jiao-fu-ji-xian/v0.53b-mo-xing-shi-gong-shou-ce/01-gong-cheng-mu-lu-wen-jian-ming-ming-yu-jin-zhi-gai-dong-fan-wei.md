# 01｜工程目录、文件命名与禁止改动范围

## 必须建立目录

```
entry/src/main/ets/
├── entryability/
├── pages/
│   ├── HomePage.ets
│   ├── RoundReadyPage.ets
│   ├── BombPage.ets
│   ├── LoseRevealPage.ets
│   └── ChallengeRevealPage.ets
├── components/
│   ├── NoxPrimaryButton.ets
│   ├── NoxSecondaryButton.ets
│   ├── NoxBombCore.ets
│   ├── NoxPromptCard.ets
│   ├── NoxChallengeCard.ets
│   └── NoxBrand.ets
├── model/
│   ├── GameState.ets
│   ├── Challenge.ets
│   └── GameSession.ets
├── data/
│   └── challenges.ts
├── constants/
│   ├── UiTokens.ets
│   ├── GameConstants.ets
│   └── CopyConstants.ets
├── services/
│   ├── RandomService.ets
│   ├── HapticService.ets
│   └── SoundService.ets
└── utils/
    └── TimeUtils.ets
```

## 职责固定

* `UiTokens.ets`：UI 常量；
* `GameConstants.ets`：时间/随机常量；
* `CopyConstants.ets`：固定文案；
* `GameSession.ets`：当前 Session 状态；
* `BombPage.ets`：组合 UI，不写随机算法；
* `RandomService.ets`：只产生随机数；
* `HapticService.ets` / `SoundService.ets`：只由状态事件调用。

## 禁止

* 禁止第二套颜色常量；
* 禁止页面硬编码品牌色；
* 禁止所有逻辑塞进 `Index.ets`；
* 禁止任意重命名上述文件；
* 禁止一个文件同时承担 UI、随机、持久化。

## 验收

提交完整目录树，与本页逐项一致。

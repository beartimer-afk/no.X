# 01｜工程目录、文件命名与禁止改动范围

## 固定目录

entry/src/main/ets/ ├── entryability/ ├── pages/ │ ├── Index.ets │ ├── HomePage.ets │ ├── RoundReadyPage.ets │ ├── BombPage.ets │ ├── LoseRevealPage.ets │ └── ChallengeRevealPage.ets ├── components/ │ ├── NoxPrimaryButton.ets │ ├── NoxSecondaryButton.ets │ ├── NoxBombCore.ets │ ├── NoxPromptCard.ets │ ├── NoxChallengeCard.ets │ └── NoxBrand.ets ├── model/ │ ├── GameState.ets │ ├── Challenge.ets │ └── GameSession.ets ├── data/ │ └── challenges.ts ├── constants/ │ ├── UiTokens.ets │ ├── GameConstants.ets │ └── CopyConstants.ets ├── services/ │ ├── RandomService.ets │ ├── HapticService.ets │ └── SoundService.ets └── utils/ └── TimeUtils.ets

## Index.ets 唯一职责

Index.ets 是 App Root。允许：持有/取得 GameSession；根据 state 选择业务页面；把页面事件回调交给 Session；做页面级淡入淡出。

Index.ets 禁止：画 Home/Bomb 具体 UI；写随机；写 Bomb 时间算法；写 Prompt/Challenge 数据；写音频/触觉 API。

## 其他职责

UiTokens=UI 常量；GameConstants=时间/随机常量；CopyConstants=固定文案；GameSession=Session 状态；BombPage=组合 UI；RandomService=随机；Haptic/Sound 只在 TASK-014 接系统能力。

## 禁止

第二套颜色常量；页面硬编码品牌色；把所有逻辑塞 Index；任意重命名；一个文件同时承担 UI+随机+持久化。

## 验收

提交完整目录树；Index 必须是薄 Root，不得成为巨型页面。

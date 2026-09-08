# 01｜Session 与状态机

## 1. Session 定义

一次从点击 START 到主动结束游戏，称为一个 Session。Session 内可包含任意多轮 Round。

Session 不要求登录、不要求命名玩家、不要求先选场景。

### Session 必须保存的运行时状态

* sessionId：本地临时 UUID；
* startedAt；
* currentRoundIndex；
* activePackIds；
* activeChallengePool；
* currentGameType；
* currentRulePreset；
* secretMatchResult（如有）；
* completedChallengeIds；
* skippedChallengeIds；
* temporaryMediaRefs；
* pendingNextMove；
* pausedState。

默认只需要在 App 意外退到后台时短暂恢复，不要求形成永久历史。

## 2. 顶层状态机

### S0 Idle

首页静止状态。

可转移到：QuickStart / SecretMatch / Packs / Create / Settings。

### S1 SessionPrepare

加载 Pack、规则和挑战池。目标时间应尽量短，正常情况下用户不应感知到“加载页”。

失败时：回首页，并明确告诉用户当前挑战池不可用。

### S2 RoundReady

显示 ROUND N 和当前小游戏名称。持续约 1.8–2.4 秒，自动进入游戏。

用户可在这里暂停/退出，但不提供大量设置。

### S3 MiniGameActive

运行当前小游戏。第一版即 Bomb。

结束条件：产生 loser / executor。

### S4 ResultReveal

只揭晓“谁输了/轮到谁”，不显示挑战。

默认停留 0.8–1.3 秒。

### S5 ChallengeHidden

挑战已确定，但卡片保持背面状态。

等待用户主动揭晓。这里允许营造第二层悬念。

### S6 ChallengeRevealed

显示挑战正文、必要的时间/媒体类型/难度。

用户选择：开始 / 完成 / 跳过，取决于 Challenge Type。

### S7 ChallengeExecuting

限时、录音、拍照、录像等任务的执行态。普通任务可以跳过此状态。

### S8 ChallengeComplete

任务完成后的短反馈态。默认 0.5–1 秒。

### S9 NextMove

决定是否给本轮输家下一轮控制权。

默认规则：每 2–3 轮出现一次，而不是每轮强制出现，以避免拖慢节奏。

### S10 RoundTransition

Round +1，清空本轮临时状态，保留 Session 状态，进入下一次 RoundReady。

### S11 Pause

用户主动暂停或系统中断。恢复后返回中断前稳定状态；不会在恢复瞬间直接爆炸。

### S12 SessionEnd

生成 Session Summary，并处理临时媒体。

## 3. 状态转移原则

* 任意涉及秘密内容的状态进入后台时立刻遮挡；
* MiniGameActive 进入后台必须暂停随机时钟，恢复后给予至少 1.5 秒安全期；
* ChallengeExecuting 的录音/录像被系统中断时，不自动标记完成；
* 任何 Challenge 都允许 Skip；
* Skip 只更换挑战，不改变本轮输家；
* 连续 Skip 3 次后，提供“降低本轮挑战强度”快捷入口；
* App 崩溃恢复时优先恢复到最近一个稳定节点，而不是恢复半完成动画。

## 4. Round 数据结构

每轮运行时至少包含：

* roundId；
* index；
* gameType；
* startedAt；
* loserToken（不一定是账号，只表示当前持有方）；
* selectedChallengeId；
* challengeRevealAt；
* challengeStatus：hidden / revealed / executing / completed / skipped；
* nextMoveType；
* endedAt。

## 5. 节奏预算

一轮理想总时长：20–75 秒。

推荐分配：

* RoundReady：2 秒；
* MiniGame：8–40 秒；
* 输家揭晓：1 秒；
* 挑战翻牌：1–4 秒；
* 执行：5–30 秒（由挑战决定）；
* Complete / NextMove / Transition：2–8 秒。

如果一轮平均超过 90 秒，应检查是不是流程、文字或任务本身过重。

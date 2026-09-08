# 21｜3B 失败排查手册：禁止边猜边改

出现 FAIL 时，只按类型检查，不允许顺手重构。

## 类型 1：构建失败

只修当前 TASK 涉及的构建错误。不得升级 SDK、换工程结构、引入新依赖。

## 类型 2：UI 偏移

先核对当前 TASK 固定坐标与 UiTokens。禁止因为一个页面偏 4vp 就全局改 Token。

## 类型 3：状态错乱

只检查 GameState、Event 日志、事件真值表。禁止增加第二套 boolean 状态机。

## 类型 4：Bomb 爆炸时机不对

正式实现没有 bombStartAt / bombExplodeAt。只检查：bombTargetHoldMs、bombAccumulatedHoldMs、pressStartAt、pressSafeUntil、monotonicNowMs()。

Pressing 中：effective = bombAccumulatedHoldMs + (now - pressStartAt)。Explosion 只在 effective >= target 且已过 400ms safety 时触发。

重点排查：Waiting/Handoff 是否错误累计；ACTION\_UP/CANCEL 是否重复累计；Pressing 切后台是否漏结算或重复结算；每轮是否错误重抽 target。

禁止改 14–30s 正式范围让测试好过。

## 类型 5：Fake 影响按压

检查 Fake 是否修改 pressStartAt、state、target、accumulated。Fake 只能是视觉/触觉/声音瞬态。

## 类型 6：Explosion 卡顿

优先减少粒子到 8；禁止删除 Vacuum、缩短总 1180ms。

## 修复输出

ROOT\_CAUSE: FIXED\_FILES: WHY\_NO\_OTHER\_FILES\_CHANGED: BUILD: RETEST:

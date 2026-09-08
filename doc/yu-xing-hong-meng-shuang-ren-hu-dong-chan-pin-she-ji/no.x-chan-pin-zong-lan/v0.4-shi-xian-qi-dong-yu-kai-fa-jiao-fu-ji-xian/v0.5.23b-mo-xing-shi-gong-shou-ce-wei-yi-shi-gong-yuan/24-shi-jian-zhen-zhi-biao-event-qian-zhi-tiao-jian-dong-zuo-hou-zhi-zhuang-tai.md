# 24｜事件真值表：Event → 前置条件 → 动作 → 后置状态

开发时只允许实现下表事件。visualPhase 不产生业务导航事件。

| Event                  | 前置状态                                              | 必须动作                                        | 后置状态                 |
| ---------------------- | ------------------------------------------------- | ------------------------------------------- | -------------------- |
| HOME\_START\_TAP       | HOME                                              | Tap；导航 Ready                                | ROUND\_READY         |
| READY\_CONFIRM\_TAP    | ROUND\_READY                                      | 生成一次 bombTargetHoldMs；accumulated=0；进入 Bomb | BOMB\_WAITING\_PRESS |
| CORE\_DOWN             | BOMB\_WAITING\_PRESS                              | pressStartAt=now；safeUntil=now+400；Tap      | BOMB\_PRESSING       |
| PRESS\_REACH\_VALID    | BOMB\_PRESSING 且 held>=1100                       | isPressValid=true；Lock 一次                   | BOMB\_PRESSING       |
| REAL\_EXPLOSION        | BOMB\_PRESSING 且 effectiveHold>=target 且已过 safety | loser=holder；锁触控                            | EXPLOSION            |
| CORE\_UP\_SHORT        | BOMB\_PRESSING 且 held<1100 且未爆                    | 先把 held 加入 accumulated；清 press；提示           | BOMB\_WAITING\_PRESS |
| CORE\_CANCEL           | BOMB\_PRESSING 且未爆                                | 先把 held 加入 accumulated；清 press              | BOMB\_WAITING\_PRESS |
| CORE\_UP\_VALID        | BOMB\_PRESSING 且 held>=1100 且未爆                   | 先把 held 加入 accumulated；Tap；进入 Handoff       | BOMB\_HANDOFF        |
| HANDOFF\_TIMEOUT       | BOMB\_HANDOFF 1300ms                              | 切 holder；换 Prompt                           | BOMB\_WAITING\_PRESS |
| FAKE\_SIGNAL           | BOMB\_PRESSING 且跨过未触发 fake threshold 且未真实爆        | 260ms 假信号，不改 press/target/accumulated       | BOMB\_PRESSING       |
| EXPLOSION\_END         | EXPLOSION 1180ms                                  | 导航 Lose                                     | LOSE\_REVEAL         |
| LOSE\_REVEAL\_READY    | LOSE\_REVEAL 1100ms                               | CTA 可点                                      | LOSE\_REVEAL         |
| LOSE\_CHALLENGE\_TAP   | LOSE\_REVEAL ready                                | Tap；抽 Challenge                             | CHALLENGE\_REVEAL    |
| CHALLENGE\_DO\_TAP     | CHALLENGE\_REVEAL 未完成                             | 标记完成态                                       | CHALLENGE\_REVEAL    |
| CHALLENGE\_NEXT\_TAP   | CHALLENGE\_REVEAL 未完成                             | 抽另一 Challenge                               | CHALLENGE\_REVEAL    |
| NEXT\_ROUND\_TAP       | CHALLENGE\_REVEAL 已完成                             | round+1；清本轮 Bomb 状态                         | ROUND\_TRANSITION    |
| ROUND\_TRANSITION\_END | ROUND\_TRANSITION 280ms                           | 显示 Ready                                    | ROUND\_READY         |
| APP\_BACKGROUND        | 任意                                                | 按 TASK-015；Pressing 必须先结算 held              | 视规则                  |
| APP\_FOREGROUND        | 任意                                                | 按 TASK-015                                  | 视规则                  |

## 事件优先级

同一次 UI/timer 检查中：REAL\_EXPLOSION 最高；其次 ACTION\_UP/CANCEL；FAKE\_SIGNAL 最低。若真实 Explosion 与 Fake 同时满足，只执行 REAL\_EXPLOSION。

## 日志

\[NOX]\[STATE] ---->

非法 Event：不改状态，记录 \[NOX]\[IGNORED]，不 Toast、不崩溃。

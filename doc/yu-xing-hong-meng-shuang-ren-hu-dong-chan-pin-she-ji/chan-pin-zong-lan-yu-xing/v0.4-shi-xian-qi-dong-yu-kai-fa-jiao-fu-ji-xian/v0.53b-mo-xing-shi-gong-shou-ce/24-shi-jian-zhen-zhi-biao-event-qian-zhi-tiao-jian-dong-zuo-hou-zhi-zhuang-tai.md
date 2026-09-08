# 24｜事件真值表：Event → 前置条件 → 动作 → 后置状态

开发时只允许实现下表列出的事件。页面不得自行添加隐藏事件。

| Event                  | 前置状态                          | 必须动作                           | 后置状态                 |
| ---------------------- | ----------------------------- | ------------------------------ | -------------------- |
| HOME\_START\_TAP       | HOME                          | Tap haptic；导航 Ready            | ROUND\_READY         |
| READY\_CONFIRM\_TAP    | ROUND\_READY                  | round 初始化；生成 explodeAt；进入 Bomb | BOMB\_WAITING\_PRESS |
| CORE\_DOWN             | BOMB\_WAITING\_PRESS          | 记录 pressStartAt；Tap haptic     | BOMB\_PRESSING       |
| CORE\_UP\_SHORT        | BOMB\_PRESSING 且 held<1100    | 清 press；提示再按久一点                | BOMB\_WAITING\_PRESS |
| CORE\_CANCEL           | BOMB\_PRESSING                | 清 press                        | BOMB\_WAITING\_PRESS |
| PRESS\_REACH\_VALID    | BOMB\_PRESSING 且 held>=1100   | isPressValid=true；Lock haptic  | BOMB\_PRESSING       |
| CORE\_UP\_VALID        | BOMB\_PRESSING 且 held>=1100   | 进入 Handoff；禁触控                 | BOMB\_HANDOFF        |
| HANDOFF\_TIMEOUT       | BOMB\_HANDOFF after 1300ms    | 切 holder；换 Prompt              | BOMB\_WAITING\_PRESS |
| FAKE\_SIGNAL           | Bomb active                   | 260ms 假信号，不改游戏时间               | 原 Bomb 状态            |
| REAL\_EXPLOSION        | Bomb active 且 now>=explodeAt  | 记录 loser；锁触控；Explosion         | EXPLOSION            |
| EXPLOSION\_END         | EXPLOSION after 1180ms        | 导航 Lose                        | LOSE\_REVEAL         |
| LOSE\_REVEAL\_READY    | LOSE\_REVEAL after 1100ms     | 启用 CTA                         | LOSE\_REVEAL         |
| LOSE\_CHALLENGE\_TAP   | LOSE\_REVEAL ready            | 轻触觉；抽挑战                        | CHALLENGE\_REVEAL    |
| CHALLENGE\_DO\_TAP     | CHALLENGE\_REVEAL 未完成         | 标记完成态                          | CHALLENGE\_REVEAL    |
| CHALLENGE\_NEXT\_TAP   | CHALLENGE\_REVEAL 未完成         | 抽另一 Challenge                  | CHALLENGE\_REVEAL    |
| NEXT\_ROUND\_TAP       | CHALLENGE\_REVEAL 已完成         | round+1；清本轮状态                  | ROUND\_TRANSITION    |
| ROUND\_TRANSITION\_END | ROUND\_TRANSITION after 280ms | 显示 Ready                       | ROUND\_READY         |
| APP\_BACKGROUND        | 任意                            | 按生命周期规则处理                      | 视原状态                 |
| APP\_FOREGROUND        | 任意                            | 按恢复规则处理                        | 视恢复规则                |

## 事件实施规则

* 一个用户动作只能发送一个业务 Event；
* UI 组件不得直接导航两个页面；
* 所有状态变更必须能在日志中找到 Event 名称；
* 不允许通过多个 boolean 组合暗中形成第二状态机。

## 日志格式固定

```
[NOX][STATE] <old> --<event>--> <new>
```

示例：

```
[NOX][STATE] BOMB_WAITING_PRESS --CORE_DOWN--> BOMB_PRESSING
```

## 非法 Event

如果状态不满足前置条件：

* 不改变状态；
* 打日志 `[NOX][IGNORED] event=<...> state=<...>`；
* 不崩溃；
* 不弹 Toast。

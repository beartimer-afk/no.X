# 26｜测试专用 Debug 开关：只能测试构建使用

## 目标

稳定复现 Tense / Fake / Explosion，不修改正式规则。

## 固定 Debug 配置

DEBUG\_FORCE\_BOMB\_TARGET\_HOLD\_MS: number? = null DEBUG\_FORCE\_FAKE\_AT\_HOLD\_MS: number? = null DEBUG\_FORCE\_TENSION\_RATIO: number? = null DEBUG\_DISABLE\_SOUND: boolean = false

只允许 Debug 构建存在，不得放 Settings UI。

## 测 Explosion

DEBUG\_FORCE\_BOMB\_TARGET\_HOLD\_MS = 5000。

含义：本测试轮的累计按压目标为 5000ms。仍只有按住 Core 时累计；Waiting/Handoff 仍暂停。不得实现成进入页面 5 秒后自动爆。

## 测 Fake

DEBUG\_FORCE\_FAKE\_AT\_HOLD\_MS = 2500。含义：effectiveHoldMs 首次跨过 2500ms 且仍 Pressing 时触发正式 Fake 路径。

## 测 Tense

DEBUG\_FORCE\_TENSION\_RATIO = 0.80。只强制 visualPhase 的计算输入，不触发真实 Explosion。

## Release 要求

所有 Debug 值必须为 null/false 或不包含；不得存在隐藏手势打开 Debug。

## 禁止

改正式 14–30s；Debug 使用 wall-clock 自动爆；Debug 另写 Explosion 分支；暴露到用户 Settings。

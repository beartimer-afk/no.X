# 00｜3B 执行协议：每次只做一个任务

## 唯一施工源

3B 施工时只允许使用本 v0.5.1 手册。不得同时提供 v0.4、v0.3、v0.2。每次只提供：00 执行协议、01 工程目录、02 Token、03 状态机、当前一个 TASK。

## 开工前 PRECHECK

TASK-001 前只检查不写代码：HarmonyOS 工程存在、Debug 构建通过、存在默认 EntryAbility/ArkUI 入口。不允许 3B 自行升级 SDK、修改 bundleName/签名或引入依赖。

若工程不存在或当前不能构建：STATUS=BLOCKED；SPEC\_GAP=PROJECT\_NOT\_INITIALIZED；然后 STOP。

## 根页面规则

现有 HarmonyOS 默认启动页保持 `pages/Index.ets`。`Index.ets` 是唯一 App Root：只负责根据 GameSession.state 选择显示 HomePage / RoundReadyPage / BombPage / LoseRevealPage / ChallengeRevealPage，并负责页面级淡入淡出。业务页面禁止自己调用系统 Router 导航。

Index.ets 禁止写随机算法、Bomb 计时、Prompt 抽取、Challenge 抽取、触觉/声音实现。

## 触觉/声音分阶段规则

TASK-014 之前，文档中出现的 HAPTIC\_\* / SOUND\_\* 只表示“逻辑事件节点”。实现方式只允许日志，例如 `[NOX][HAPTIC_PLACEHOLDER] HAPTIC_TAP`。禁止提前调用系统触觉/音频 API。

TASK-014 才把这些既有事件节点接到 HapticService / SoundService。不得因为前期是占位日志而改变事件出现位置。

## 每个任务固定 8 步

1. 复述任务编号与目标；2. 列修改文件；3. 检查依赖；4. 只实现本任务；5. 构建运行；6. 生成规定截图/录屏；7. 逐条 PASS/FAIL；8. STOP。

## 固定回复

TASK / STATUS / FILES TO CHANGE / FILES CHANGED / SPEC\_GAP / BUILD / ACCEPTANCE / ARTIFACTS / NEXT ACTION: STOP。

## 禁止

禁止继续下一个任务；禁止未构建声称 PASS；禁止改任务外文件；禁止自行换色/字号/圆角；禁止加功能；禁止把精确值改成“大约”；禁止从旧文档选方案；禁止冲突后自行折中。

没有定义的具体决策必须标记 SPEC\_GAP，不猜。

# 开工 Gate｜TASK-001 前必须全部通过

本页不是开发任务。**任何一项 FAIL，禁止开始 TASK-001。**

## Gate G01｜工程存在

必须确认当前仓库已经存在可识别的 HarmonyOS / ArkUI 工程，不允许 3B 根据经验新建工程。

PASS 条件：能看到既有工程配置、`entry` 模块、`entry/src/main/ets/`。

FAIL 输出：`SPEC_GAP: PROJECT_NOT_INITIALIZED`。

## Gate G02｜基线 Debug 构建通过

在修改任何 NO.X 代码之前，对当前基线执行一次 Debug 构建。

PASS：构建退出码为 0。

FAIL：保存原始错误日志，输出 `SPEC_GAP: BASELINE_BUILD_FAILED`，STOP。禁止升级 SDK、改依赖版本、改签名来尝试修复。

## Gate G03｜启动入口确认

必须确认冷启动最终进入 `entry/src/main/ets/pages/Index.ets`（或现有配置明确把 Index 作为默认页面）。

PASS：可从工程配置与现有代码确认。

FAIL：输出 `SPEC_GAP: STARTUP_PAGE_NOT_INDEX`，STOP。禁止 3B 自行修改 `main_pages.json`、模块配置或 EntryAbility 来重定向入口。

## Gate G04｜工作区干净

开始 TASK-001 前执行 Git 状态检查。

PASS：没有来源不明的未提交修改；若有用户明确保留的修改，必须由人工先处理/提交，再开始。

FAIL：输出 `STATUS: BLOCKED`，列出未提交文件，STOP。禁止 3B 自动丢弃、stash、commit 用户已有改动。

## Gate G05｜施工上下文正确

3B 本次只能收到以下 5 份文档：

1. `00｜3B 执行协议`；
2. `01｜工程目录、文件命名与禁止改动范围`；
3. `02｜全局 Token`；
4. `03｜状态机与数据模型`；
5. `04｜TASK-001：全局壳与 Home 静态页`。

不得提供 v0.4、v0.3、v0.2，不得提供 TASK-002 及后续任务。

## Gate G06｜禁止基础设施自由发挥

TASK-001 前和 TASK-001 中都禁止：

* 升级 HarmonyOS SDK/API；
* 修改 bundleName；
* 修改签名；
* 添加第三方 UI 库；
* 添加状态管理框架；
* 添加路由框架；
* 添加网络权限；
* 添加相机/麦克风权限；
* 重构用户已有工程目录。

任何一项被认为“必须”时，先 `SPEC_GAP`，人工决定。

## Gate PASS 固定输出

只有六项全部通过时才允许输出：

```
NOX_PRECHECK: PASS
G01 PROJECT_EXISTS: PASS
G02 BASELINE_BUILD: PASS
G03 STARTUP_INDEX: PASS
G04 GIT_CLEAN: PASS
G05 CONTEXT_SCOPE: PASS
G06 INFRA_FREEZE: PASS
NEXT: TASK-001 ONLY
```

然后才进入 TASK-001。

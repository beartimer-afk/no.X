# 开工 Gate｜TASK-000A 前必须全部通过

本页不是开发任务。**任何一项 FAIL，禁止开始 TASK-000A。Gate 阶段不得修改文件。**

## G01｜工程存在

确认当前仓库已有 HarmonyOS / ArkUI 工程、entry 模块、entry/src/main/ets/。不存在：`SPEC_GAP: PROJECT_NOT_INITIALIZED`，STOP。

## G02｜基线 Debug 构建通过

修改前执行 Debug 构建。失败：保存原始错误，`SPEC_GAP: BASELINE_BUILD_FAILED`，STOP。禁止升级 SDK/依赖/签名。

## G03｜启动入口确认

确认冷启动最终进入现有 `entry/src/main/ets/pages/Index.ets`。不明确：`SPEC_GAP: STARTUP_PAGE_NOT_INDEX`，STOP。

## G04｜Git 工作区干净

来源不明未提交修改：`STATUS: BLOCKED`，列出文件，STOP。禁止自动 stash/commit/discard 用户改动。

## G05｜施工上下文正确

Gate 阶段只给 3B：00执行协议、01工程目录、03A静态资源合同、本 Gate。禁止旧版本和任何业务 TASK。

## G06｜基础设施冻结

禁止升级 SDK/API、bundleName、签名、路由框架、状态框架、第三方 UI 库、网络/相机/麦克风权限。

## G07｜设计侧静态资源包存在且完整

必须存在：`design_delivery/NOX_static_assets_v0.1/`

至少包含：resource\_manifest.json、SHA256SUMS.json、App Icon 前景/背景/json、5个 `nox_ic_*.svg`。

若编排者仍持有 ZIP，则 ZIP SHA-256 必须为：

`3bc4622f8a5039ea6693120cdeeac6b17eb716f2d3ecc81db5d902d2396d943c`

缺失/不一致：`BLOCK_ASSET_MISSING`，STOP。禁止联网补齐。

## PASS 固定输出

```
NOX_PRECHECK: PASS
G01 PROJECT_EXISTS: PASS
G02 BASELINE_BUILD: PASS
G03 STARTUP_INDEX: PASS
G04 GIT_CLEAN: PASS
G05 CONTEXT_SCOPE: PASS
G06 INFRA_FREEZE: PASS
G07 ASSET_PACK: PASS
NEXT: TASK-000A ONLY
```

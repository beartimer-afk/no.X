# 32｜Diff 审查红线：每个任务必须过代码变更检查

## P0：出现即拒绝整个 TASK

* 修改任务允许文件列表之外文件且未说明；
* 新增网络权限；
* 新增相机/麦克风权限；
* 新增广告/分析/账号 SDK；
* 新增外部 UI 框架；
* 修改品牌 Token 以适配当前页面；
* 修改 GameConstants 让测试容易通过；
* 删除/绕过状态机；
* 将真实随机逻辑换成固定测试值但忘记 Debug 隔离；
* 使用设计展示图作为整页背景截图冒充 UI；
* 把敏感/成人外部网络图片硬链接进代码。

## P1：要求修复后重新验收

* 硬编码颜色出现在页面文件；
* 文案散落硬编码而不是 CopyConstants / data；
* 重复实现 Button 样式而不复用组件；
* 出现 1100、1300、14000 等魔法数字而不是 GameConstants；
* 页面新增未定义 boolean 控制业务状态；
* 触摸事件和业务逻辑混成无法追踪的多个回调；
* Fake / Explosion 使用两套不同 Core 组件。

## P2：可记录技术债但不阻塞

* 命名略显冗长；
* 小范围重复但不影响行为；
* 平台特定 API 包装还可优化。

## 搜索关键字检查

每个 TASK 后建议全局搜索：

```
http
https
camera
microphone
permission
analytics
ads
#FF2FAE
#A855F7
14000
30000
1100
1300
```

预期：

* URL 不应来自业务代码；
* 权限相关不应出现；
* 品牌颜色只在 UiTokens；
* 游戏数字只在 GameConstants/测试配置。

## Diff 通过格式

```
DIFF REVIEW: PASS
OUT-OF-SCOPE FILES: NONE
NEW DEPENDENCIES: NONE
NEW PERMISSIONS: NONE
TOKEN DRIFT: NONE
STATE MACHINE DRIFT: NONE
```

没有这段检查，不进入下一任务。

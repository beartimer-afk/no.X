# 30｜最终 Definition of Done：3B 模型不得自行宣布项目完成

开发模型只能报告任务完成，**不能自行宣布 NO.X v0.1 完成**。

## 最终必须全部满足

### 功能

* Home 可启动；
* Round Ready 正确；
* Bomb 按压 1100ms 判定正确；
* Handoff 1300ms；
* 随机 Explosion 14–30s；
* 新 holder 400ms 安全；
* ≤2 Fake；
* Explosion 1180ms 五段；
* Lose Reveal 1100ms 后可点击；
* Challenge 可完成/下一张；
* 下一轮正确；
* 连续 5 轮无错误；
* 前后台规则正确。

### UI

* 15 张固定截图全 PASS；
* 无系统蓝；
* 无卡通炸弹；
* Core 为圆核；
* 粉 + 紫 + 暗黑视觉成立；
* 页面文案逐字正确；
* 没有未定义功能入口。

### 工程

* Release 构建 PASS；
* Debug 开关未暴露；
* 无网络权限；
* 无相机/麦克风权限；
* 无账号/分析 SDK；
* Git TASK 提交链完整；
* 无未解释 SPEC\_GAP。

### 体验

需要真人两人试玩至少一次，检查：

* 按住是否容易理解；
* 传手机是否自然；
* Fake 是否会吓到但不混乱；
* Explosion 是否有冲击；
* Lose→Challenge 是否有第二次悬念；
* 是否愿意自然继续第二轮。

这里的试玩反馈只能产生新版本规格，开发模型**不得直接根据试玩意见修改代码**。

## 最终发布决定

只能由人工产品验收明确给出：

```
NO.X v0.1 ACCEPTED
```

在此之前项目状态只能是 `IMPLEMENTATION / QA`。

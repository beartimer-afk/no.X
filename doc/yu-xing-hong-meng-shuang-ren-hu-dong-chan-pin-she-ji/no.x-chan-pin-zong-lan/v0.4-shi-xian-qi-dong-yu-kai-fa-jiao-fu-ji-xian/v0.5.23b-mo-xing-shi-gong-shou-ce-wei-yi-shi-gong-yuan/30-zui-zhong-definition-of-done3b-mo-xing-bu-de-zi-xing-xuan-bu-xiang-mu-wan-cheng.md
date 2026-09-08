# 30｜最终 Definition of Done：3B 模型不得自行宣布项目完成

开发模型只能报告任务完成，不能自行宣布 NO.X v0.1 完成。

## 最终必须全部满足

### 静态资源

* HarmonyOS App Icon 为前景/背景双层 1024×1024 PNG；
* 背景层无透明像素；
* `app.icon=$media:nox_app_icon`；
* 桌面不再显示默认 DevEco/HarmonyOS 图标；
* 运行时 Back/Close/Heart 等图标来自冻结 `nox_ic_*.svg`；
* 没有开发模型下载的图片/icon/音频；
* NO.X v0.1 新增字体文件数量必须为 0；
* 不存在 3B 自建 `fonts/` 目录。

### 功能

* Home 可启动；
* Round Ready 正确；
* Bomb 按压1100ms判定正确；
* Handoff1300ms；
* 每轮随机 `bombTargetHoldMs` 为14000–30000ms累计按压预算；
* Waiting/Handoff/后台不累计 Bomb 预算；
* Explosion 只在 Pressing 发生；
* 新按压400ms安全窗；
* ≤2 Fake；
* Explosion1180ms五段；
* Lose Reveal1100ms后可点击；
* Challenge可完成/下一张；
* 下一轮正确；
* 连续5轮无错误；
* 前后台规则正确。

### UI

* 15张固定截图全PASS；
* 无系统蓝；无卡通炸弹；Core为圆核；粉+紫+暗黑视觉成立；
* 页面文案逐字正确；
* 没有未定义功能入口。

### 工程

* Release构建PASS；Debug开关未暴露；
* 无网络权限；无相机/麦克风权限；无账号/分析SDK；
* Git提交链包含 TASK-000A、000B、001–017；
* 无未解释 SPEC\_GAP / BLOCK\_ASSET\_MISSING。

### 声音

v0.1 若未发布正式设计侧音频包：SoundService 静默/no-op 是正确状态，不能因此判 FAIL；但不得存在开发模型自行下载的音频素材。

### 体验

真人两人试玩至少一次：按住是否容易理解；传手机是否自然；Fake是否吓到但不混乱；Explosion是否有冲击；Lose→Challenge是否有第二次悬念；是否自然继续第二轮。

试玩反馈只能形成新版本规格，开发模型不得直接根据反馈改代码。

## 最终发布决定

只有人工产品验收可以给出：

`NO.X v0.1 ACCEPTED`

此前状态只能是 `IMPLEMENTATION / QA`。

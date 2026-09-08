# 12｜【T10 完成｜Development Package + Acceptance Tests｜V1 开发冻结包】

**邮件主题：** RE: 【项目立项】Personal Storage Layer — Mobile Photos V1｜T01–T10 主线程

**收到时间：** 2026-09-05T18:10:21Z

## 邮件正文原文

```
【T10 完成｜Development Package + Acceptance Tests｜V1 开发冻结包】  
  
验收结论：PASS WITH CONSTRAINTS  
  
一、目标  
把 T01–T09 压缩为一份无上下文低成本 AI 可以直接执行的最终开发规格。T10
不重新发明产品，而是冻结开发顺序、模块边界、不可更改规则、端到端验收用例和 Done Definition。若 T10 与前序任务冲突，以最新冻结约束及
T10 中明确引用的前序规则为准。  
  
二、产品一句话  
一个 Local-only 的照片保护层：用户直接浏览本地照片，只对例外照片表达 Important / Private 两个正交意图；Core 根据本地
Policy 自动决定是否需要远端副本，Private 在进入任何 Storage Provider 前必须由 Core 本地加密；Storage
通过插件扩展，V1 只真实实现 GitHub Reference Plugin；我们的后台只提供 Plugin Marketplace
Catalog/版本信息，不接触用户数据。  
  
三、V1 硬边界｜开发 AI 不得修改  
1\. 当前产品规格平台中立，不加入 iOS/Android/Desktop 专有产品逻辑。  
2\. 唯一一级页面 Home；不得新增底部 Tab。  
3\. 不做我们的账号、登录、注册、云端用户数据库。  
4\. 不做 Albums、Search、People、Memories、Editor、Social、完整 File Manager。  
5\. Protection
只有两个正交维度：importance=standard|important；privacy=standard|private。UI 可显示
Standard / Important / Private / Private+Important 四组合，但不能包装成四档安全等级。  
6\. Private = Core 客户端本地加密后才可进入 Plugin；加密失败绝不允许明文降级上传。  
7\. Plugin 不决定 ProtectionPolicy、不持有 LocalKeyMaterial、不默认读取完整 Photo Library。  
8\. PluginInstalled != StorageInstanceReady。  
9\. Upload Success != Verified != Protected。  
10\. Protected 只能由 ProtectionEvaluation 根据 Policy 与实际 verified RemoteCopy 计算。  
11\. 删除 SourceCopy 不自动删除 RemoteCopy；移除 StorageInstance 也不等于删除 Provider 数据。  
12\. Marketplace 不得获得
PhotoAsset、Protection、Credential、Key、StorageInstance、RemoteCopy、Transfer/Verification
历史。  
13\. GitHub 的 repo/commit/branch 等概念只能存在 GitHub Plugin 内，禁止污染 Core Domain。  
  
四、开发模块边界  
A. Local Library  
职责：发现/索引 PhotoAsset 与 SourceCopy，向 Home 提供可见资产；区分 access
unknown/partial/denied/empty/loading/error。  
  
B. Protection Core  
职责：ProtectionAssignment、ProtectionPolicy、ProtectionEvaluation、ProtectionState；唯一拥有
Desired State 判断权。  
  
C. Planner / Reconciler  
输入：Assignment + Policy + Storage eligibility + Actual
RemoteCopies/Verification。  
输出：缺失 DesiredCopy 与可执行 Job。必须差额执行、幂等、可重复 reconcile。  
  
D. Encryption Core  
职责：Private 明文→EncryptionArtifact 密文；Key 仅本地；失败即停止远端路径。插件不能实现或绕过此层。  
  
E. Transfer Engine  
职责：持久化 Job、retry、resume、cancel、provider uncertain response
reconciliation、upload→verify。崩溃后从本地事实恢复。  
  
F. Plugin Runtime  
职责：Manifest、configuration
schema、capabilities、authenticate、resolveTarget、put/stat/get/verify/delete/healthCheck
等 Provider 适配。  
  
G. GitHub Reference Plugin  
职责：Authorization/Credential、Repository、optional base path、Instance display
name；实现 Core 所需 capability，并返回标准化错误。它只是 Reference，不是 Core 特例。  
  
H. Marketplace Client  
职责：获取 PluginCatalogEntry/版本/撤回或 blocked 信息。Marketplace 断网不得影响 Home、已安装 Plugin
或现有 StorageInstance 的本地工作。  
  
I. UI  
职责：Home、Selection、Protection Picker、Storage Required、Storage Center、Plugin
Store、Plugin Detail、Setup/Test、Problems、Protection/Storage Detail；UI 只消费
Domain 状态，不自行推断 Protected。  
  
五、核心持久化事实  
必须能持久化并恢复：  
PhotoAsset  
SourceCopy  
ProtectionAssignment  
ProtectionPolicy / version  
InstalledPlugin  
StorageInstance  
CredentialRef（secret 独立安全存储）  
CapabilitySnapshot  
RemoteCopy  
VerificationRecord  
TransferJob / operationId  
必要的 EncryptionArtifact 引用/加密版本信息  
LocalKeyMaterial（独立安全域）  
ResumeContext 可持久化，但不能成为唯一事实源。  
  
ProtectionState、Problem 聚合允许缓存，但必须能从事实重算。禁止只存 isProtected=true。  
  
六、V1 默认 Policy  
Standard：默认不强制远端副本；无 Storage 不报错。  
Important：minimumVerifiedRemoteCopies=1；verificationRequired=true；clientSideEncryptionRequired=false，除非同时
Private。  
Private：minimumVerifiedRemoteCopies=1；verificationRequired=true；clientSideEncryptionRequired=true。  
Private+Important：同时满足 Important 与 Private；V1 最低仍为 1 个 verified remote copy，但
Domain/Policy 不得把 1 写死，未来可提升为多个独立 StorageInstance。  
  
七、ProtectionState 唯一语义  
UNASSIGNED：无 Assignment。  
PENDING：已有自动执行路径，正在加密/传输/验证/重试。  
PROTECTED：Policy 全条件满足。  
DEGRADED：仍有部分有效保护事实，但当前不足以满足 Policy。  
NEEDS_ATTENTION：无法自动继续，需要用户动作。  
FAILED：执行问题事实；不得覆盖已有 RemoteCopy 的真实状态，最终用户状态仍由 Evaluation 决定。  
  
八、唯一主交互  
Launch→Home→允许/管理照片访问→照片流→长按进入 Selection→继续多选→“设置保护方式”→Protection Picker→写入
Assignment→Re-evaluate。  
  
若已有 eligible READY Storage：Home 显示 Protecting，后台执行。  
若缺 Storage：Assignment 先保存→Storage Required Prompt→Configure Storage 或 Not Now。  
  
Configure：Plugin Store→Plugin Detail→Install→Set Up→Test Connection→Storage
ready→自动恢复原 Assignment→Re-evaluate→Private 时
Encrypt→Transfer→Verify→Protected。  
  
用户绝不需要因为配置 Storage 而重新选择照片或重新打标。  
  
九、Plugin Capability Test  
必须验证而不是只 ping：  
Authentication→Target Resolve→Write test payload→Stat→Verify→Cleanup（若支持
Delete）。  
返回 CapabilitySnapshot：testedAt、pluginVersion、configVersion、capability
status、limits、verification method、warnings。  
只有满足当前 Policy 所需 capabilities 的 Instance 才 eligible。  
  
十、Private Trust Boundary  
明文允许：Core 读取源、Encryption Core 必要内存/受控本地处理。  
明文禁止：Marketplace、Provider、Private 路径中的 Storage Plugin 输入。  
Private Plugin 输入必须是 EncryptionArtifact ciphertext + opaque object identifier
+ 最少技术 metadata。  
  
Provider 不应看到 ProtectionMark、语义文件名、人物/相册标签、LocalKeyMaterial、其他 Storage
信息。Private 默认不把原始语义 metadata 暴露成 Provider-side metadata。  
  
十一、Recovery 产品约束  
由于 Core 永久 Local-only，我们的服务器不保存 Private Key。  
因此必须明确：设备与本地密钥同时丢失、且用户没有自己保存恢复材料时，Private 密文可能永久不可恢复。  
  
V1 开发必须至少预留并实现一个用户控制的 Recovery Kit / Recovery Key 产品流程，要求：  
\- 恢复材料由用户持有，不上传我们的服务端。  
\- 可以验证恢复材料是否可用。  
\- UI 明确提醒用户保存恢复材料。  
\- 不把 Provider Credential 当作 Private 解密密钥。  
\- 不承诺“忘记/丢失密钥后客服可找回”。  
  
具体密码算法、KDF、密钥封装格式属于安全工程实现阶段，必须经专门安全评审；低成本 AI 不得自行发明密码学方案。这是 T10 的实施约束之一。  
  
十二、错误分类  
插件/Transfer
至少统一：AUTH_REQUIRED、AUTH_DENIED、AUTH_EXPIRED、TARGET_NOT_FOUND、TARGET_NOT_WRITABLE、RATE_LIMITED、QUOTA_EXCEEDED、OBJECT_TOO_LARGE、NETWORK_UNAVAILABLE、PROVIDER_UNAVAILABLE、WRITE_FAILED、READ_FAILED、VERIFY_FAILED、OBJECT_NOT_FOUND、CONFIG_INVALID、CAPABILITY_UNSUPPORTED、PLUGIN_ERROR、UNKNOWN_PROVIDER_ERROR。  
每项必须附 retryable|user_action|terminal，UI 映射成可执行修复动作，不直接展示 Provider 原始错误。  
  
十三、Problems 聚合  
只在需要用户动作时打扰。  
按修复动作聚合：Configure storage、Reconnect storage、Fix storage
configuration、Protection incomplete、Restore encryption access、Retry
protection。  
临时网络错误仍自动 retry 时不进入强提醒。  
  
十四、开发顺序｜必须按依赖执行  
M1 Local Domain + Persistence：先实现事实模型与可重算 Evaluation。  
M2 Home + Selection + Protection Picker：用本地假数据完成 UI/Assignment。  
M3 Plugin Protocol + Mock Storage Plugin：先用 Mock 验证 Core 不依赖 GitHub。  
M4 Planner/Reconciler + Transfer/Verification：跑通普通 Important。  
M5 Encryption Boundary + Recovery：跑通 Private，密码学实现需安全评审。  
M6 GitHub Reference Plugin：替换 Mock 跑真实 Provider。  
M7 Storage/Marketplace UI：Store→Install→Configure→Test→Resume Intent。  
M8 Problems + Recovery Paths：断网、Auth expired、partial success、crash resume。  
M9 Full Acceptance Suite。  
  
关键：先做 Mock Plugin 再做 GitHub，是为了验证 Core 没有 GitHub 锁定；不是增加第二个正式 Provider。  
  
十五、Acceptance Tests｜任何一项失败都不能称 V1 Done  
  
AT-01 无账号首次进入  
Given 全新本地状态  
When 启动  
Then 直接 Home；不存在 Login/Register/Onboarding 强制页；未授权时只显示照片访问空状态。  
PASS 条件：不配置 Storage 也能进入产品。  
  
AT-02 部分照片访问  
Given Core 只能看到部分照片  
Then Home 显示可见内容并明确“部分内容”；不得声称完整图库。  
  
AT-03 Standard 无 Storage  
Given 无 StorageInstance  
When 用户不打特殊标记  
Then 照片正常存在 Home，不出现强制 Storage 错误。  
  
AT-04 Selection  
Given Home 有照片  
When 选择第一张并继续增减  
Then selectedAssetIds 精确变化；0 项自动退出 Selection；Cancel 清空选择。  
  
AT-05 四组合批量打标  
Given 多张不同旧 Mark 照片  
When 批量选择 Private+Important  
Then 所有 Assignment 最终表达 importance=important + privacy=private；重复提交幂等，不产生重复任务。  
  
AT-06 Picker Cancel  
When 打开 Picker 后取消  
Then 返回 Selection，选择仍保留；Assignment 不改变。  
  
AT-07 缺 Storage 仍保存意图  
Given 无 eligible Storage  
When 标 Important/Private  
Then Assignment 已持久化；状态 NEEDS_ATTENTION；Not Now 后回 Home仍可恢复。  
  
AT-08 Storage 配置后恢复原意图  
Given AT-07  
When 完成 Plugin Install→Configure→Test PASS  
Then 不重新选照片；Core 自动 Re-evaluate 原 Assignment 并开始执行。  
  
AT-09 PluginInstalled != Ready  
Given GitHub Plugin 安装成功但未配置/未 Test  
Then 不能作为 Policy target，不能开始上传。  
  
AT-10 Capability Test 真验证  
Given 配置 Credential/Target  
When Test  
Then 至少执行 Auth、Target、Write、Stat、Verify；仅认证成功但不可写时 Instance 不 Ready。  
  
AT-11 Important 普通闭环  
Given Important + eligible Storage  
When transfer+verification 成功  
Then RemoteCopy VERIFIED，Evaluation 满足
minimumVerifiedRemoteCopies，ProtectionState=PROTECTED。  
  
AT-12 Upload Success 但 Verify 未完成  
Then RemoteCopy 不能计入 Policy，ProtectionState != PROTECTED。  
  
AT-13 Verify Fail  
Given Provider 已接收对象但内容不匹配/验证失败  
Then RemoteCopy INVALID/UNVERIFIED，不计数；进入 retry 或 Needs Attention。  
  
AT-14 Private 明文边界  
Given Private  
When执行  
Then Plugin.putObject 输入必须是 ciphertext；测试桩捕获 Plugin 输入并证明与源明文不相同且不能获得
LocalKeyMaterial。  
  
AT-15 Private 加密失败  
Then 不调用 Plugin.putObject；Provider 收到 0 bytes；状态 PENDING/NEEDS_ATTENTION；绝无明文
fallback。  
  
AT-16 Private 已有旧明文副本  
Given Standard 曾上传明文 RemoteCopy  
When 改为 Private  
Then旧明文 RemoteCopy 不计入 Private Policy；必须生成合规密文 DesiredCopy。  
  
AT-17 改回 Standard 不自动删远端  
Given Private RemoteCopy 存在  
When改 Standard  
Then只改变 Desired State；远端密文不被自动删除。  
  
AT-18 Provider 临时断网  
Given Transfer 中网络失败且 retryable  
Then Job进入 retry wait；Assignment 保留；已有 verified copies 不被删除；网络恢复可继续。  
  
AT-19 不确定写入响应  
Given Provider 可能已写成功但客户端未收到确认  
Then重试前先 reconcile/stat/verify，不能盲目制造重复对象。  
  
AT-20 Crash Resume  
Given App/Core 在 Encrypt/Transfer/Verify 任一非终态中断  
When重新启动  
Then从持久化 Job/Remote facts恢复或 reconcile；无需用户重新打标。  
  
AT-21 Credential Expired  
Then StorageInstance=AUTH_EXPIRED；新任务暂停；Problems 提供
Reconnect；重认证+Test→READY→自动 Re-evaluate。  
  
AT-22 Partial Batch Success  
Given 2 个 PhotoAsset，一成功一失败  
Then各自独立 ProtectionState；UI 不得显示“全部成功”。  
  
AT-23 已有部分副本的 Policy 不满足  
Given Policy 需要 N、实际 verified<N 且至少有一个有效副本  
Then不能 PROTECTED；根据自动路径进入 PENDING/DEGRADED，不抹掉有效副本事实。  
  
AT-24 Marketplace Offline  
Then Home、InstalledPlugin、已有 StorageInstance、现有 Transfer 不因 Catalog
无法加载而失效；只影响浏览/安装/更新。  
  
AT-25 Marketplace 数据泄露测试  
检查所有 Marketplace request/telemetry payload：不得出现 PhotoAsset
ID/内容、ProtectionMark、StorageInstance config、Credential、Key、RemoteCopy、Transfer
history。  
  
AT-26 Plugin 最小权限  
使用测试插件尝试请求完整 Library/其他 Credential/LocalKeyMaterial；Core 必须拒绝。Private
putObject 只提供当前密文 payload 和当前目标。  
  
AT-27 Opaque metadata  
Given Private 原文件名/metadata 含敏感语义  
Then Provider object key/side metadata 不直接暴露这些语义；原始 metadata 如需保存应位于加密 payload
内。  
  
AT-28 Remove Storage impact preview  
Given 某 StorageInstance 正被 Protected assets 依赖  
When用户 Remove  
Then确认前先计算受影响数量；明确 Remove connection 不等于 Delete remote data；确认后重新 Evaluation。  
  
AT-29 Plugin Update  
Given Plugin 更新  
Then Credential/config 不被静默覆盖；CapabilitySnapshot needsRetest；若 capability
变化则重新 Evaluation。  
  
AT-30 Plugin Withdraw/Block  
Then不远程删除用户本地数据/RemoteCopy记录；严重 blocked 可阻止新 Transfer，并明确受影响 Protection。  
  
AT-31 Recovery Kit  
Given 新建/启用 Private 能力  
Then用户能生成/保存自己控制的恢复材料；恢复材料不上传我们的服务器；可在隔离测试状态验证能恢复必要 Key；失败时给出明确风险提示。  
  
AT-32 无 Recovery 材料风险  
Given用户拒绝/丢失恢复材料  
Then产品不得显示“永远可恢复”；必须明确本地 Key 丢失可能导致 Private 密文不可恢复。  
  
AT-33 GitHub Core 解耦  
静态/Domain 测试：Core 模型和 Protection UI 中不得出现 branch/commit/repo 等 GitHub
专有字段；它们只能在 GitHub Plugin config/implementation。  
  
AT-34 Mock Plugin 替换性  
同一 Important 流程分别使用 Mock Plugin 与 GitHub Plugin，Core Planner/Evaluation/UI
不改代码路径，仅插件实现和 config schema 不同。  
  
AT-35 Protected 计算唯一来源  
尝试让 UI、TransferJob 或 Plugin直接写 Protected；架构必须禁止/忽略。只有 Evaluation 可生成该状态。  
  
十六、UI Acceptance  
Home 是唯一一级入口；Standard 照片视觉安静；Selection 只有“设置保护方式”核心动作；Picker 不出现 Storage
名；Storage Required 在 Assignment 保存之后出现；Plugin Detail 有 Publisher/Data
Access/Privacy/Pricing/Capabilities；Setup Secret 默认遮蔽；Test 显示逐项状态；Problems
按修复动作聚合；成功尽量轻量反馈，临时 retry 不骚扰用户。  
  
十七、性能/体验产品验收（平台中立）  
当前不冻结毫秒级指标，但冻结体验要求：已有照片时增量加载不得全屏阻断；选择/打标的本地 Assignment 写入应给即时反馈，不等待远端；远端执行不阻断
Home 浏览；批量操作逐 Asset 独立事实化；大量任务不能让 UI 只剩一个不可解释的总 spinner。具体性能数字在平台实施阶段建立基线。  
  
十八、市场/成熟机制依据  
最终交互继续采用前序已冻结的成熟范式：照片浏览/多选采用主流照片图库的“内容优先+原位多选”；无账号本地入口借鉴 Local/Gallery-first
产品；Provider 配置采用成熟同步工具的“配置实例→Test→复用”；插件发现采用 Extension Marketplace 的
Browse/Search→Detail→Install→Configure；保护结果借鉴备份产品的状态反馈，但升级为 Policy
satisfaction 而不是“上传完成”。本任务不再扩张竞品范围，避免引入与已确认 UI 冲突的新范式。  
  
十九、外部实施约束  
GitHub 的认证方式、文件/仓库/流量限制、API 速率、服务条款和 Provider 行为属于实施时动态事实，必须在编码 GitHub Plugin
前按当时 GitHub 官方文档验证，不能把易变化数字写死进 Core。  
  
密码算法、KDF、nonce、key wrapping、Recovery Kit
格式属于高风险安全工程；必须采用成熟标准并做专门安全评审。当前产品规格只冻结“真实客户端加密、Key
不上我们的服务端、可由用户控制恢复材料恢复”的行为契约，不授权低成本 AI 自创密码学。  
  
二十、Definition of Done  
V1 只有同时满足以下条件才 Done：  
\- AT-01～AT-35 全部通过；  
\- 无账号/无后台用户数据依赖；  
\- GitHub Reference Plugin 真正跑通 Install→Configure→Test→Transfer→Verify；  
\- Private 真正以 ciphertext 到达 Provider；  
\- Recovery 风险有真实产品闭环；  
\- Crash/断网/Auth expired/Verify fail 可恢复；  
\- Marketplace 离线不破坏 Core；  
\- Core 可用 Mock Plugin 证明 Provider 抽象；  
\- UI 没有自行把 Upload success 当 Protected；  
\- 不出现本规格禁止的范围扩张。  
  
二十一、验收结论  
从 T01 到 T10，V1 已达到“平台中立、可进入实施准备”的产品规格程度。无上下文低成本 AI 可以依据邮件完成
Domain、UI、Plugin、执行链与验收测试，不需要重新决定核心产品逻辑。  
  
本任务标记 PASS WITH CONSTRAINTS，而不是纯 PASS，只有两个原因：  
1\. GitHub Provider 的动态 API/服务限制必须在真正实现前以当时官方资料验证；  
2\. Private 的具体密码学与 Recovery Kit 格式必须由安全工程阶段选择成熟方案并评审，不能由产品规格或低成本 AI 随意决定。  
  
这两项不属于未定义产品需求，而是实施安全/外部依赖门槛。  
  
T10：PASS WITH CONSTRAINTS。  
  
【项目阶段结论】  
T01–T10 产品定义阶段全部完成。下一阶段若开始编码，应进入“平台选择 + 技术架构 + 安全实现评审 + GitHub Provider
实时约束验证”，不要继续修改本期已冻结的核心产品交互，除非 Acceptance Test 发现规格矛盾。


```

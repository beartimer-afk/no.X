# 08｜【T06 完成｜Storage Plugin Spec + GitHub Reference Plugin】

**邮件主题：** RE: 【项目立项】Personal Storage Layer — Mobile Photos V1｜T01–T10 主线程

**收到时间：** 2026-09-05T16:28:26Z

## 邮件正文原文

```
【T06 完成｜Storage Plugin Spec + GitHub Reference Plugin】  
  
验收结论：PASS WITH CONSTRAINTS  
  
一、任务目标  
冻结 Core 与 Storage Provider 之间的产品协议边界，使 V1 可以只实现 GitHub，但未来增加
S3、WebDAV/NAS、对象存储、云盘时不需要重写 Protection / Transfer / UI 核心。  
  
插件的职责只有：  
“把 Core 给出的 payload 可靠地放进/读出/验证某个外部 Storage Provider。”  
  
插件不负责：  
\- 判断照片是否 Important/Private  
\- 决定要几个副本  
\- 生成 Private 密钥  
\- 决定什么时候 Protected  
\- 保存完整照片 Library  
\- 向 Marketplace 上报用户数据  
  
二、插件体系四层必须分离  
  
1\. PluginCatalogEntry  
Marketplace 服务端下发的“插件商品/目录信息”。  
  
2\. InstalledPlugin  
本地已安装某 pluginId + version 的事实。  
  
3\. StoragePlugin Runtime  
插件代码提供的能力实现。  
  
4\. StorageInstance  
用户配置并通过 Capability Test 的实际 Provider 目标。  
  
禁止：  
CatalogEntry = InstalledPlugin  
InstalledPlugin = StorageInstance  
StoragePlugin = Provider Account  
  
三、插件 Manifest 最小规范  
  
每个插件必须声明：  
  
identity  
\- pluginId：全局稳定 ID，不随版本变化  
\- name  
\- publisherId  
\- publisherDisplayName  
\- version  
\- protocolVersion  
\- minimumCoreProtocolVersion  
  
presentation  
\- icon reference  
\- shortDescription  
\- longDescription  
\- goodFor  
\- providerName  
  
pricing  
\- pluginPricingType: free | paid | subscription | unknown  
\- pluginPricingDescription  
\- providerChargesMayApply: boolean  
\- providerPricingDescription  
\- providerPricingUrl/reference（Catalog 可下发）  
  
trust  
\- declaredDataAccess  
\- declaredNetworkDomains / provider endpoints（技术执行方式后置）  
\- requiresCredential: boolean  
  
configuration  
\- configurationSchemaVersion  
\- fields[]  
  
capabilities  
\- capability declarations  
  
lifecycle  
\- status: available | deprecated | withdrawn | blocked  
\- update information  
  
四、Configuration Schema  
  
Core 负责渲染统一配置 UI，插件提供 Schema。  
  
Field 最小属性：  
\- fieldId  
\- label  
\- type  
\- required  
\- secret  
\- placeholder  
\- helperText  
\- validationRules  
\- defaultValue（secret 禁止从 Marketplace 提供用户值）  
\- optionsSource：static | pluginDynamic  
\- visibilityCondition（仅简单条件，避免插件自建任意 UI）  
  
V1 支持的产品字段类型：  
text  
secret  
boolean  
singleSelect  
dynamicSelect  
path/string  
authorizationAction  
  
禁止 V1：  
插件提供任意 HTML/Web UI 作为主配置页。  
原因：  
\- 权限不可控  
\- UX 不一致  
\- 低成本 AI 实施复杂  
\- 容易把 Credential/用户数据带出 Core 边界  
  
如 Provider 授权必须跳第三方授权页面，视为 authorizationAction；返回 Core 后仍由 Core 页面完成配置和 Test。  
  
五、Credential 边界  
  
Credential：  
\- Core 的本地安全秘密存储持有。  
\- StorageInstance 只保存 credentialRef，不保存可直接导出的 secret。  
\- 插件执行某个授权操作时由 Core 以受控方式提供所需 Credential。  
\- 插件不得将 Credential 写普通日志。  
\- Marketplace 永远不可读取。  
\- PluginCatalogEntry 不包含用户 Credential。  
  
用户删除 StorageInstance：  
CredentialRef 对应秘密在确认不再被其他 Instance 使用后可清理。  
删除 Credential ≠ 删除 Provider 远端数据。  
  
六、Capability Model  
  
V1 Core 认识以下能力：  
  
AUTHENTICATE  
能验证 Credential/身份是否可用。  
  
TARGET_RESOLVE  
能确定配置目标存在/可访问，例如 repository/bucket/folder。  
  
WRITE  
能创建远端 payload。  
  
READ  
能读取已写入 payload；用于强验证/恢复场景。  
  
STAT  
能查询远端对象存在性、size、provider metadata/identifier。  
  
VERIFY_CONTENT  
能以 Core 可接受的方法确认远端内容与本地预期一致。  
实现可以是 read-back hash、Provider 可信 content digest 等，但必须把 verificationMethod 返回给
Core。  
  
DELETE  
能显式删除远端对象。  
V1 可为 optional；Core 不依赖自动删除实现保护闭环。  
  
LIST  
能枚举某目标路径对象。  
V1 optional。  
  
MOVE/RENAME  
optional。  
  
SERVER_SIDE_COPY  
optional。  
  
ATOMIC_WRITE  
声明是否能避免部分对象被当成完成对象。  
若不支持，插件必须提供可识别 incomplete/temporary 写入语义。  
  
ARBITRARY_BINARY  
是否可原样存储 Core 提供的任意密文 bytes。  
Private Policy 必须要求该能力或等价能力。  
  
MAX_OBJECT_SIZE  
不是 boolean；插件/Instance 返回约束值。  
  
七、Core 最小插件操作协议  
  
1\. describe()  
返回 Manifest + capability declarations。  
  
2\. validateConfiguration(config)  
只做结构/Provider 特定静态校验，不代表连接成功。  
  
3\. authenticate(context)  
验证 Credential。  
  
4\. resolveTarget(config)  
确认目标可解析，并返回非 secret 的 target descriptor。  
  
5\. testCapabilities(instanceDraft)  
执行 Capability Test，返回逐项结果。  
  
6\. putObject(request)  
输入：  
\- operationId（幂等关联）  
\- target logical path/key  
\- payload stream/reference  
\- expectedSize  
\- expectedContentFingerprint（若适用）  
\- contentType 可选  
\- minimal metadata  
输出：  
\- providerObjectId/path  
\- providerVersion/etag/digest（如有）  
\- bytesAccepted  
\- provider response summary（非 secret）  
  
7\. statObject(remoteLocator)  
输出：  
exists / size / version / digest / observable metadata。  
  
8\. getObject(remoteLocator)  
仅在插件支持 READ 时。  
  
9\. verifyObject(request)  
插件根据自身能力执行验证，返回：  
PASS | FAIL | INCONCLUSIVE  
\+ verificationMethod  
\+ observed facts  
  
重要：  
INCONCLUSIVE 不能计为 VERIFIED。  
  
10\. deleteObject(remoteLocator)  
仅插件声明 DELETE 时可用；必须由 Core 显式调用，不因 Source 删除自动触发。  
  
11\. healthCheck(instance)  
轻量判断 Provider/credential/target 是否仍可使用；不能用一次网络失败直接推断远端数据丢失。  
  
八、插件返回错误必须标准化  
  
Core 不允许 UI 直接显示 Provider 原始错误。  
  
标准错误类别：  
AUTH_REQUIRED  
AUTH_DENIED  
AUTH_EXPIRED  
TARGET_NOT_FOUND  
TARGET_NOT_WRITABLE  
RATE_LIMITED  
QUOTA_EXCEEDED  
OBJECT_TOO_LARGE  
NETWORK_UNAVAILABLE  
PROVIDER_UNAVAILABLE  
WRITE_FAILED  
READ_FAILED  
VERIFY_FAILED  
OBJECT_NOT_FOUND  
CONFIG_INVALID  
CAPABILITY_UNSUPPORTED  
PLUGIN_ERROR  
UNKNOWN_PROVIDER_ERROR  
  
每个错误同时包含：  
\- retryability: retryable | user_action | terminal  
\- safeUserMessageKey  
\- providerDiagnostic（本地诊断用，必须脱敏）  
  
九、Capability Test 精确定义  
  
TEST 不是 ping。  
  
必须依次/按依赖验证：  
  
1\. Authentication  
Credential 可用。  
  
2\. Target Resolve  
目标存在或插件能够按配置使用。  
  
3\. Write  
向专用 test namespace 写入随机测试 payload。  
  
4\. Stat  
确认远端对象可观察。  
  
5\. Verify  
验证远端对象与测试 payload 一致。  
若需要 read-back，可读取后比较。  
  
6\. Cleanup  
若 DELETE capability 存在，删除测试对象。  
若不能 DELETE，插件必须使用不会污染用户正式命名空间的测试策略，并明确可能残留。  
  
Test 输出 CapabilitySnapshot：  
\- testedAt  
\- pluginVersion  
\- instanceConfigVersion  
\- 每项 capability status  
\- limits  
\- verification methods  
\- warnings  
  
只有满足当前目标 Policy 的所有 required capabilities 才可作为 eligible StorageInstance。  
  
十、StorageInstance 状态  
  
DRAFT  
CONFIGURED  
TESTING  
READY  
READY_WITH_LIMITATIONS  
AUTH_EXPIRED  
MISCONFIGURED  
UNAVAILABLE  
DISABLED  
  
READY：  
当前 CapabilitySnapshot 满足至少 Core 基线。  
  
READY_WITH_LIMITATIONS：  
可工作但缺某些 optional capability；是否可用于某 Policy 由 Eligibility 判断。  
  
十一、Plugin Update 规则  
  
安装新版本不得自动修改 StorageInstance 的 Credential/config。  
  
更新后：  
\- 若 protocol/config schema 完全兼容，可保留 Instance，但 CapabilitySnapshot 标记
needsRetest。  
\- 若配置 schema 需要 migration，插件必须声明 migration path。  
\- Migration 失败：旧 config 保留，不覆盖。  
\- 若新版本 capability 下降，Re-evaluate 受影响 Policy。  
  
Marketplace 不可通过 Catalog Update 静默改变用户 ProtectionPolicy。  
  
十二、Plugin Withdraw / Block  
  
withdrawn：  
不再推荐新安装，但本地已安装插件不自动删除。  
UI 提示状态。  
  
blocked：  
用于明确的严重安全/协议风险。  
Core 可以禁止新 Transfer，但必须：  
\- 保留 StorageInstance 配置记录  
\- 保留 Credential（除非用户主动删除或安全策略明确要求）  
\- 保留 RemoteCopy/Verification 历史  
\- 告知用户哪些 ProtectionState 受影响  
  
Marketplace 发一个 blocked 标记不能远程删除用户数据。  
  
十三、插件权限原则  
  
默认拒绝，按能力授予。  
  
插件只能得到当前操作必要的数据：  
putObject 时：  
\- 得到当前 payload  
\- 当前目标  
\- 最少 metadata  
  
不能默认得到：  
\- 完整 Photo Library  
\- 所有 ProtectionMark  
\- 其他 StorageInstance  
\- 其他 Provider Credential  
\- LocalKeyMaterial  
\- Marketplace identity/profile（产品无用户账号）  
  
Private：  
插件得到的 payload 必须已经是 EncryptionArtifact 密文。  
插件不参与 Private 明文→密文的加密核心。  
  
十四、Metadata 最小化  
  
Core 给插件的 remote key 默认不得直接使用：  
\- 原照片标题  
\- 用户备注  
\- “private”  
\- “wife”  
\- album/person 等敏感语义  
  
推荐由 Core 生成 opaque object identifier/path。  
  
Provider 必要 metadata：  
\- object identifier  
\- payload bytes  
\- size  
\- 协议工作所需最小技术字段  
  
原始 EXIF 是否保留在加密 payload 内与是否暴露为 Provider metadata 必须分离。  
Private 默认不把原始语义 metadata 暴露为 Provider-side metadata。  
  
十五、GitHub Reference Plugin  
  
目的：  
只用于跑通真实插件闭环，不把 GitHub 当成长期照片存储推荐。  
  
产品层配置：  
1\. GitHub Credential / Authorization  
2\. Repository  
3\. Base path（optional）  
4\. Instance display name  
  
目标路径：  
Core 提供 opaque logical key。  
插件映射到 repository/basePath 下的对象路径。  
  
必须验证：  
AUTHENTICATE  
TARGET_RESOLVE  
WRITE  
STAT  
VERIFY_CONTENT  
ARBITRARY_BINARY  
MAX_OBJECT_SIZE / Provider constraints  
  
READ：  
若验证方案需要 read-back，则支持。  
  
DELETE：  
可以实现，但 V1 Protection 不依赖自动删除。  
  
十六、GitHub Provider 约束必须显式暴露  
  
GitHub 是代码托管/协作平台，不应在产品文案中宣传为专业照片备份服务。  
插件详情必须有：  
“此插件用于连接 GitHub 作为可配置存储目标。GitHub 的账户、仓库、文件大小、流量、使用政策及其他限制由 GitHub 决定。”  
  
Core 必须能够接收插件返回的：  
OBJECT_TOO_LARGE  
RATE_LIMITED  
QUOTA/PROVIDER LIMIT  
POLICY/PROVIDER REJECTION 类错误。  
  
不能因为 GitHub V1 能工作，就把 Core 设计成：  
\- commit  
\- branch  
\- repo  
\- Git LFS  
等 GitHub 专有 Domain。  
这些只能存在 GitHub Plugin 内部/配置层。  
  
十七、Private + GitHub 路径  
  
PhotoAsset  
→ Core Private Policy  
→ Core Local Encryption  
→ opaque object key  
→ GitHub Plugin.putObject(ciphertext)  
→ GitHub receives ciphertext  
→ plugin stat/read/verify  
→ VerificationRecord  
→ RemoteCopy verified  
→ ProtectionEvaluation  
→ Protected  
  
GitHub Plugin 永远不接收 LocalKeyMaterial。  
若插件请求明文 PhotoAsset 作为 Private 上传输入，协议实现不合格。  
  
十八、非 GitHub 抽象验证  
  
用三个未来 Provider 思维测试：  
  
A. S3-compatible  
bucket + prefix  
put/stat/get/delete  
ETag/digest 语义不同  
可映射。  
  
B. WebDAV/NAS  
endpoint + path  
put/head/get/delete  
认证方式不同  
可映射。  
  
C. Consumer cloud drive  
OAuth + folder ID  
可能有 upload session、限流、文件重名  
仍可通过 put/stat/verify 抽象；Provider 特性留在插件内部。  
  
结论：  
Core API 没有依赖 repo/commit/branch，抽象未被 GitHub 锁死。  
  
十九、验证闭环  
  
Plugin 生命周期：  
Catalog→Install→Installed→Configure→Test→CapabilitySnapshot→Ready→Transfer→Verify。  
PASS。  
  
Credential 失效：  
Ready→AUTH_EXPIRED→Reconnect→Test→Ready。  
PASS。  
  
Update：  
新版本→needsRetest→Test→更新 Snapshot→Policy Re-evaluate。  
PASS。  
  
Private：  
Core 加密→Plugin 只收密文→Provider 密文→Verify。  
PASS。  
  
Marketplace 泄露：  
Catalog 不包含 Credential/Photo/Policy。  
PASS。  
  
Provider 差异：  
S3/WebDAV/Cloud Drive 可映射。  
PASS。  
  
二十、PASS WITH CONSTRAINTS 的原因  
产品协议已足够冻结，但 GitHub 的具体 Provider 限制、API 认证方式、对象大小/仓库限制、使用政策等属于“真实实施前必须根据当时
GitHub 官方规则验证”的外部约束。  
  
当前阶段用户明确要求只讲产品、不绑定技术平台，因此本任务不把会变化的 GitHub API 数字写死进 Core Spec。  
  
这不阻塞 T07/T08/T09/T10。  
  
二十一、对后续任务  
T07：  
需要保存 pluginId/version、config schema
version、credentialRef、CapabilitySnapshot、remoteLocator、providerVersion/digest。  
  
T08：  
必须围绕 operationId 做幂等；VERIFY INCONCLUSIVE 不得计数。  
  
T09：  
必须审查插件包供应链、声明权限与实际权限、blocked 机制、metadata leakage。  
  
T10：  
GitHub Reference Acceptance Test 必须从安装一直测到密文 RemoteCopy Verified。  
  
闭环结论：  
V1 可以只实现 GitHub，同时 Core 的产品协议没有 GitHub 锁定。插件只能执行 Provider 适配，不拥有 Protection
决策权和 Private 密钥。  
  
T06：PASS WITH CONSTRAINTS。


```

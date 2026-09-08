# 04｜【T02 完成｜核心 Domain Model 与术语冻结】

**邮件主题：** RE: 【项目立项】Personal Storage Layer — Mobile Photos V1｜T01–T10 主线程

**收到时间：** 2026-09-05T16:08:58Z

## 邮件正文原文

```
【T02 完成｜核心 Domain Model 与术语冻结】  
  
验收结论：PASS  
  
1\. 任务目标  
冻结后续所有任务使用的唯一领域语言，避免“照片=文件”“插件=存储”“上传成功=已保护”“Private=隐藏”等混淆。  
  
2\. 硬边界  
\- 当前只定义产品，不绑定平台。  
\- V1 唯一真实 Reference Plugin：GitHub。  
\- Private 必须在进入 Provider 前完成本地客户端加密。  
\- Core 永久 Local-only；Marketplace 仅下发插件目录、版本、描述等。  
  
3\. 核心实体  
PhotoAsset：用户眼中的一项逻辑照片资产。  
SourceCopy：当前本地可读取的实际来源副本。  
ProtectionMark：用户表达的保护意图，例如 Normal / Important / Private。  
ProtectionPolicy：把意图翻译成机器可判断的 Desired State。  
ProtectionAssignment：某 PhotoAsset 当前绑定的保护意图/策略。  
StoragePlugin：Provider 适配扩展，例如 GitHub Plugin。  
InstalledPlugin：某插件版本已安装在本地的事实。  
StorageInstance：用户基于插件完成授权/配置/测试后形成的真实可用存储目标。  
StorageCapability：该存储实例实际具备的能力声明。  
DesiredCopy：Policy Planner 判断“应该存在”的目标副本。  
RemoteCopy：某 PhotoAsset 在某 StorageInstance 上实际存在的远端副本。  
TransferJob：为创建/更新 RemoteCopy 执行的一次可恢复任务。  
EncryptionArtifact：Private 场景中本地加密后产生的密文载荷。  
VerificationRecord：对某 RemoteCopy 做过的验证事实。  
ProtectionEvaluation：Actual State 与 Desired State 的比较结果。  
ProtectionState：面向用户的最终保护摘要。  
  
4\. 关键关系  
PhotoAsset  
→ ProtectionAssignment  
→ ProtectionPolicy  
→ ProtectionEvaluation  
→ DesiredCopy  
→ TransferJob  
→ RemoteCopy  
→ VerificationRecord  
→ 重新计算 ProtectionState  
  
Private 路径必须是：  
SourceCopy 明文  
→ 本地加密  
→ EncryptionArtifact 密文  
→ TransferJob  
→ StorageInstance / Provider  
→ RemoteCopy（密文）  
→ Verification  
→ ProtectionState  
  
5\. 必须严格区分  
PhotoAsset ≠ SourceCopy  
StoragePlugin ≠ StorageInstance  
ProtectionMark ≠ ProtectionPolicy  
ProtectionAssignment ≠ TransferJob  
DesiredCopy ≠ RemoteCopy  
Upload Success ≠ Verified  
TransferJob State ≠ ProtectionState  
Private ≠ Hidden  
  
6\. ProtectionState 语义级状态  
UNASSIGNED：尚无保护绑定。  
PENDING：已有保护意图，正在自动执行，尚未满足。  
PROTECTED：当前所有 Policy 必要条件均满足。  
DEGRADED：已有部分有效保护，但当前不再完全满足 Policy。  
NEEDS_ATTENTION：系统无法自动继续，需要用户动作。  
FAILED：本轮执行失败且无自动恢复路径正在运行；但 FAILED 不能抹掉已有 RemoteCopy 的真实存在。  
  
7\. StoragePlugin / StorageInstance 生命周期  
Plugin：  
AVAILABLE_IN_CATALOG → INSTALLED → UPDATE_AVAILABLE / DISABLED / REMOVED  
  
StorageInstance：  
DRAFT → CONFIGURED → TESTING → READY  
READY → UNAVAILABLE / AUTH_EXPIRED / MISCONFIGURED  
修复后可回 READY  
  
严格规则：  
PluginInstalled 不等于 StorageInstanceReady。  
  
8\. RemoteCopy 生命周期  
TRANSFERRED_UNVERIFIED  
→ VERIFIED  
→ STALE / MISSING / INVALID  
  
上传接口返回成功时，只能说明“传输阶段成功”，不能直接变成 VERIFIED。  
  
9\. Private 加密硬约束  
\- 明文只能停留在本地允许区域。  
\- Provider 只能收到密文和最少必要协议 metadata。  
\- 加密失败时禁止自动降级为明文上传。  
\- 密钥不属于 StoragePlugin，不属于 Marketplace，不属于 Provider。  
\- Provider-side encryption 不满足 Private 定义；Private 只认客户端本地加密。  
  
10\. Local-only 数据边界  
允许进入我们的 Marketplace Server：  
\- PluginCatalogEntry：插件名、版本、Publisher、描述、图标、价格/第三方费用说明、兼容信息、发布/撤回状态。  
  
必须只存在本地：  
PhotoAsset  
SourceCopy  
ProtectionMark  
ProtectionPolicy  
ProtectionAssignment  
StorageInstance  
Credential  
LocalKeyMaterial  
DesiredCopy  
RemoteCopy  
TransferJob  
VerificationRecord  
ProtectionEvaluation  
ProtectionState  
  
Provider 可得到：  
\- 普通策略：其目标副本所需 payload + 最少必要协议 metadata。  
\- Private：仅密文 payload + 最少必要协议 metadata。  
  
Provider 不应得到：  
用户 ProtectionMark、完整 Library、其他
StorageInstance、LocalKeyMaterial、ProtectionEvaluation。  
  
11\. 失败→恢复闭环验证  
案例：一张 Important 照片要求 2 个独立已验证副本。  
  
初始：  
verifiedCopies = 0  
ProtectionState = PENDING  
  
Storage A 上传并验证成功：  
RemoteCopy A = VERIFIED  
  
Storage B 上传失败：  
不能显示 PROTECTED。  
若自动重试仍在进行：PENDING。  
若无自动恢复路径：NEEDS_ATTENTION。  
  
B 后续重试并验证成功：  
verifiedCopies = 2  
所有 Policy 条件满足  
→ PROTECTED。  
  
未来 A 暂不可访问：  
不能仅凭 Credential 失效推断远端副本已丢失；  
需按 T03/T08 的验证时效规则重新评估，可能进入 DEGRADED。  
  
12\. Private 闭环验证  
选择 Private：  
→ 创建 ProtectionAssignment  
→ Policy 要求 encryptionRequired = true  
→ 本地加密  
→ 只有密文产生成功后才允许进入 Transfer  
→ 上传成功但未验证 ≠ PROTECTED  
→ Verification PASS  
→ 再检查其余 Policy 条件  
→ 全部满足才 PROTECTED  
  
若本地加密失败：  
不得产生明文传输任务；  
不得将状态显示为 Protected。  
  
13\. 禁止模糊术语  
“同步状态”禁止作为总状态，必须明确 TransferJob State 或 ProtectionState。  
“备份完成”禁止作为 Domain 状态。  
“账号”不能用于我们的产品；只能描述 Provider Identity/Credential。  
“安全等级”不进入 Domain，使用 ProtectionMark + ProtectionPolicy + ProtectionState。  
“加密存储”必须区分 Provider-side 与 Client-side。  
  
14\. 对后续任务的强约束  
T03：  
冻结 ProtectionMark 可见模型、Policy 机器规则、Policy Satisfied 条件、验证时效。  
  
T04：  
必须直接使用本 Domain，不得另造重复状态；定义 PENDING / DEGRADED / NEEDS_ATTENTION 的进出条件。  
  
T05：  
UI 文案可以用户化，但必须一一映射 Domain；不能把 Transfer Success 渲染成 Protected。  
  
T06：  
明确 StoragePlugin / InstalledPlugin / StorageInstance 三者生命周期和 Capability。  
  
T07：  
把本 Domain 转成持久化 Schema；Credential/Key 必须与普通数据隔离。  
  
T08：  
Planner 输入 = Assignment + Policy + Actual RemoteCopies；  
输出 = DesiredCopy + TransferJobs。  
  
T09：  
对 Credential、Key、Metadata、Plugin 权限做完整 Trust Boundary。  
  
15\. 验证结论  
已用“一张照片→两个 Storage→一个失败→恢复”和“Private 加密失败→禁止明文上传”两个案例走通完整 Domain。  
T01 的所有关键交互均可映射到本 Domain。  
没有核心概念歧义。  
  
闭环结论：  
T02 已足够约束后续产品、数据流、状态机和开发规格。  
  
T02：PASS。


```

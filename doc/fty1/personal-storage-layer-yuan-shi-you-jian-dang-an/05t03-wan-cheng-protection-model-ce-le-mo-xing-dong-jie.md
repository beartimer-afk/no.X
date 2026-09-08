# 05｜【T03 完成｜Protection Model / 策略模型冻结】

**邮件主题：** RE: 【项目立项】Personal Storage Layer — Mobile Photos V1｜T01–T10 主线程

**收到时间：** 2026-09-05T16:10:14Z

## 邮件正文原文

```
【T03 完成｜Protection Model / 策略模型冻结】  
  
验收结论：PASS  
  
一、任务目标  
冻结用户“打什么标”、系统如何把标记翻译为保护策略、什么时候才允许显示
Protected。核心要求：用户操作必须极简，但底层必须能表达“重要性”和“隐私性”是两个不同维度。  
  
二、唯一产品决策  
V1 不让用户直接配置“安全等级 1/2/3”，也不把 Storage 名称放进照片操作。  
用户只面对两个可独立叠加的标记：  
1\. Important：这份内容不能轻易丢。  
2\. Private：这份内容不能以明文交给 Provider。  
  
因此一张 PhotoAsset 有四种组合：  
A. 无特殊标记 = Standard  
B. Important  
C. Private  
D. Private + Important  
  
UI 可以把四种结果呈现为快捷选项，但 Domain 永远保存两个正交布尔意图：  
importance = standard | important  
privacy = standard | private  
  
原因：  
\- “私密但不重要”与“重要但不私密”现实中都存在。  
\- 将二者压成单轴 Low/Medium/High 会产生错误语义。  
\- 两维模型未来扩展 Storage Policy 时无需迁移核心概念。  
  
三、默认行为  
PhotoAsset 被发现后默认：  
importance = standard  
privacy = standard  
  
默认不要求用户逐张处理。  
用户只处理例外。  
  
Standard 的具体远端保存策略可由用户默认 Policy 决定；若用户从未配置任何 Storage，则 Standard 照片仍可正常存在于
Home，不产生强制错误。  
V1 绝不因为“未配置远端 Storage”阻止用户浏览/打标。  
  
四、Policy 组成  
ProtectionPolicy 必须由可计算约束组成：  
  
1\. copyRequirement  
\- minimumVerifiedRemoteCopies  
\- minimumIndependentStorageInstances  
  
2\. encryptionRequirement  
\- clientSideEncryptionRequired: true/false  
  
3\. verificationRequirement  
\- verificationRequired: true  
\- verificationMethodSet：T06/T08 冻结 Provider 能支持的方法  
\- unverified copy 永远不计入满足数量  
  
4\. deletionRule  
V1 默认：  
\- 删除 SourceCopy 不自动传播到 RemoteCopy。  
\- 删除 RemoteCopy 必须是独立显式动作。  
原因：本产品核心是保护，不做双向镜像删除。  
  
5\. providerEligibility  
目标 StorageInstance 必须 READY 且满足 Policy 要求的 Capabilities。  
  
五、四种组合的 V1 默认策略语义  
  
A. Standard  
目的：最低干预。  
默认：  
clientSideEncryptionRequired = false  
minimumVerifiedRemoteCopies = 0，直到用户为 Standard 配置默认远端策略。  
说明：  
V1 不把所有手机照片强制上传；否则产品又退化成“全量相册备份”。  
  
若用户主动给 Standard 配置一个 Storage：  
可生成 1 个 verified remote copy 作为默认保存策略。  
但未配置时不显示风险警告。  
  
B. Important  
目的：明确表达“不能轻易丢”。  
V1 产品规则：  
minimumVerifiedRemoteCopies >= 1  
必须存在至少一个 READY StorageInstance。  
verificationRequired = true  
clientSideEncryptionRequired = false，除非同时 Private。  
  
注意：  
长期 Vision 可以要求 2 个独立副本；但 V1 只有一个真实 GitHub Reference Plugin，为保证最小 App 可闭环，一期
Important 的最低可执行基线定为“至少 1 个经过验证的远端副本”。  
UI/文案不得把 1 个远端副本描述成“绝不会丢”。  
  
C. Private  
目的：Provider 不得到明文。  
硬规则：  
clientSideEncryptionRequired = true  
minimumVerifiedRemoteCopies >= 1  
verificationRequired = true  
Provider 只能收到密文 payload + 最少必要协议 metadata。  
若没有可用 Storage，Assignment 保留但进入 Needs Configuration。  
若加密失败，禁止明文降级上传。  
  
D. Private + Important  
同时满足两组规则：  
clientSideEncryptionRequired = true  
minimumVerifiedRemoteCopies >= 1（V1）  
verificationRequired = true  
未来用户配置多个 StorageInstance 后，可将 Important Policy 提升为 >=2 independent
copies，而无需改变用户标记。  
  
六、为什么 V1 Important 不是强制双副本  
这是刻意的 MVP 决策，不是产品长期原则。  
原因：  
\- 一期唯一真实 Reference Plugin 为 GitHub。  
\- 强制两个 Provider 会把“验证产品核心交互”变成“先实现多个 Provider”。  
\- 用户真正表达的是“重要”，具体保护强度由 Policy 决定。  
\- Domain 已支持 minimumIndependentStorageInstances，二期可以把默认 Important 提升到 2+。  
  
验证要求：  
开发 AI 不能把 minimum=1 写死到 UI/Domain；它只能作为 V1 默认 Policy 数据。  
  
七、Protection Picker 交互语义  
用户选择一张或多张 PhotoAsset 后打开 Protection Picker。  
  
唯一推荐呈现：  
\- Standard  
\- Important  
\- Private  
\- Private + Important  
  
但界面必须让用户看懂两个属性，而不是制造“四个安全等级”的错觉。  
推荐文案：  
Standard：普通保存  
Important：重要  
Private：私密  
Private + Important：私密且重要  
  
每个选项只给一句事实型说明：  
Standard：使用默认保存方式。  
Important：按重要内容策略保留已验证的远端副本。  
Private：上传前先在本地加密。  
Private + Important：本地加密，并按重要内容策略保存。  
  
禁止：  
“最高安全”“军工级”“永不丢失”“绝对私密”等不可验证承诺。  
  
八、批量打标规则  
用户选择 N 张照片后选择一个组合：  
\- 对 N 个 PhotoAsset 统一写入新的 ProtectionAssignment。  
\- 操作必须幂等；重复选择相同标记不产生重复 Assignment/Transfer。  
\- 若部分照片已是相同标记，只处理状态差异。  
\- 改标后立即重新 ProtectionEvaluation。  
  
例：  
10 张照片中 4 张 Important，用户全部改 Private+Important：  
10 张最终统一为 Private+Important；  
已有 RemoteCopy 是否可复用必须根据新 Policy 判断。  
若旧 RemoteCopy 是明文，则不能满足 Private，即使内容已上传成功。  
  
九、Policy Satisfied 精确定义  
某 PhotoAsset 只有同时满足以下条件才能 ProtectionState=PROTECTED：  
  
1\. 存在有效 ProtectionAssignment。  
2\. Assignment 对应 Policy 可成功解析。  
3\. 所有计入数量的 RemoteCopy 当前有效。  
4\. 每个计入的 RemoteCopy 已完成 Policy 要求的 Verification。  
5\. verified copy 数量 >= minimumVerifiedRemoteCopies。  
6\. 独立 StorageInstance 数量 >= minimumIndependentStorageInstances。  
7\. 若 clientSideEncryptionRequired=true，所有计入满足 Policy 的 RemoteCopy
必须明确记录为由合规客户端加密流程产生。  
8\. 所需 StorageCapability 条件全部满足。  
9\. 没有会使上述事实失效的已知状态。  
  
任何一条不成立，不得显示 Protected。  
  
十、状态判定规则  
UNASSIGNED：  
没有 Assignment。  
  
PENDING：  
Assignment 有效，系统已有自动执行路径，正在加密/传输/验证/重试。  
  
PROTECTED：  
Policy Satisfied=true。  
  
DEGRADED：  
曾经或已有部分满足结果，但当前 verified/eligible copies 少于 Policy 要求，同时至少仍有一个已知有效保护结果。  
  
NEEDS_ATTENTION：  
无法自动继续，例如：  
\- Policy 要求远端副本但没有 READY StorageInstance。  
\- Credential 需要用户重新授权。  
\- Plugin 被禁用且没有替代目标。  
\- 配置缺字段。  
\- Private 所需密钥材料不可用且需要用户处理。  
  
FAILED：  
某一轮计划执行终止且当前没有自动重试；FAILED 主要作为执行/问题事实，UI 最终仍应结合 Evaluation 告诉用户缺什么。不得用 FAILED
覆盖“已有一个有效副本”的事实。  
  
十一、验证时效原则  
V1 不承诺持续在线实时验证。  
冻结以下语义：  
\- VerificationRecord 必须带 timestamp 和 method。  
\- 最近一次验证成功是“我们最后一次确认该副本有效”的事实。  
\- Provider 暂时不可访问时，不立即把 RemoteCopy 标成 MISSING。  
\- 只有获得明确不存在/内容不匹配证据时才能标 MISSING/INVALID。  
\- 长时间无法重新验证可标 verification stale；是否导致 DEGRADED 由具体 Policy 的
freshnessRequirement 决定。  
V1 默认 Important/Private 不设置强制实时 freshness SLA，避免无法兑现的承诺。  
  
十二、Storage 缺失时的闭环  
用户打 Private：  
→ Assignment 立即保存  
→ Evaluation 发现 requiredCopies=1、eligible Storage=0  
→ ProtectionState=NEEDS_ATTENTION  
→ UI 当场引导“配置存储”  
→ 用户进入 Plugin Store  
→ 安装 GitHub Plugin  
→ 配置 StorageInstance  
→ Capability Test PASS  
→ READY  
→ 自动回到未完成 Assignment  
→ Re-evaluate  
→ 产生 DesiredCopy/TransferJob  
→ Private 先加密  
→ Upload  
→ Verify  
→ 满足 Policy  
→ PROTECTED  
  
关键：用户不需要重新选择照片、不需要重新打标。  
  
十三、改标闭环  
Private → Standard：  
不能自动删除已经存在的加密 RemoteCopy。  
只改变 Desired State。  
旧副本成为“非当前 Policy 必需的 existing copy”，删除需独立显式动作。  
  
Important → Standard：  
同样不自动删除远端副本。  
  
Standard → Private：  
任何已有明文 RemoteCopy 不得被计入 Private Policy satisfied。  
必须创建合规密文副本。  
  
Private → Private+Important：  
若现有密文 verified copy 已满足新的 copyRequirement，可直接复用；否则只补差额。  
  
十四、费用/风险文案原则  
Protection Picker 不展示具体费用数字。  
当某 Policy 首次需要远端 Storage 时，在 Storage 选择/详情中说明：  
\- 插件自身费用（若有）  
\- Provider 可能产生的存储/流量/API 等第三方费用  
\- 费用估算仅供参考，实际以 Provider 为准  
  
Protection 文案只描述“系统执行了什么”，不承诺 Provider 永久在线、不承诺零丢失。  
  
十五、三个核心案例验证  
  
案例1：书封截图  
用户不打标。  
Standard。  
没有 Storage。  
正常留在 Home，不弹错误，不强制上传。  
PASS。  
  
案例2：重要家庭照片  
标 Important。  
无 Storage → NEEDS_ATTENTION → 引导配置。  
GitHub StorageInstance READY。  
上传+验证成功。  
verifiedCopies=1，满足 V1 Important Policy。  
→ PROTECTED。  
PASS。  
  
案例3：私密照片  
标 Private+Important。  
先本地加密。  
若加密失败：Provider 收不到明文，状态不 Protected。  
加密成功→只上传密文→Verify。  
满足 copyRequirement 后→PROTECTED。  
PASS。  
  
十六、对后续任务影响  
T04：  
必须实现四组合打标、Assignment 持久化、缺 Storage 暂停并恢复、改标重新 Evaluation。  
  
T05：  
Protection Picker 必须解释四种组合但不包装成四档“安全等级”。  
  
T06：  
StorageCapability 必须支持 Policy eligibility 判断；GitHub 只是 Reference。  
  
T07：  
ProtectionAssignment 必须保存两个正交属性或可无损表示它们；Policy 不能写死在 UI enum。  
  
T08：  
Planner 必须做“差额执行”，不能每次改标都重传所有内容。  
  
T09：  
Private 必须验证 plaintext boundary、metadata leakage、key boundary。  
  
十七、闭环结论  
V1 用户只需要回答两个问题：  
“这张照片重要吗？”  
“这张照片需要在交给 Provider 前保持私密吗？”  
  
系统负责把答案翻译为机器 Policy。  
Protected 只代表“当前 Policy 的可验证条件已满足”，不代表绝对安全或永不丢失。  
  
T03：PASS。


```

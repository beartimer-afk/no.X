# 06｜【T04 完成｜完整 UX / System State Machine】

**邮件主题：** RE: 【项目立项】Personal Storage Layer — Mobile Photos V1｜T01–T10 主线程

**收到时间：** 2026-09-05T16:11:13Z

## 邮件正文原文

```
【T04 完成｜完整 UX / System State Machine】  
  
验收结论：PASS  
  
一、目标  
把 V1 从首次启动到 Protected 的全部状态和恢复路径冻结。开发 AI 不得用“弹个错误”“重新试一下”替代明确状态转换。  
  
二、总原则  
1\. Home 永远是产品主锚点。  
2\. 用户意图先落地，再检查依赖；缺 Storage 时不得丢失已选择照片和 ProtectionAssignment。  
3\. 所有异步动作必须可恢复。  
4\. 任何失败都不能把已有有效 RemoteCopy 抹掉。  
5\. Protected 只能由 ProtectionEvaluation 计算得到，UI/TransferJob 不能自行设置。  
  
三、顶层 App 状态  
  
APP_READY：  
本地 Core 可工作，进入 Home。  
  
APP_LOCAL_DATA_ERROR：  
本地核心数据无法正常读取。不得假装空图库。显示阻断型错误和恢复/诊断入口。  
具体平台存储修复后置，但产品语义必须区分“真的没有数据”和“数据读取失败”。  
  
Marketplace 不可用不影响 Home/Core；只影响浏览/安装新插件。  
  
四、Home 数据可见状态  
  
PHOTO_ACCESS_UNKNOWN  
→ Home 空状态：说明需要访问照片才能显示内容。  
动作：Allow Photos。  
  
PHOTO_ACCESS_AVAILABLE  
→ 加载可见 PhotoAsset。  
  
PHOTO_ACCESS_PARTIAL  
→ Home 显示当前可见照片，并显示非阻断提示“仅显示已授权内容/部分内容”。  
产品不能声称这是完整图库。  
  
PHOTO_ACCESS_DENIED  
→ Home 不跳走。  
显示权限说明和“重新授权/管理访问”动作。  
  
PHOTO_LIBRARY_EMPTY  
→ 已有访问能力但没有可见 PhotoAsset。  
显示真正空图库状态，不显示权限错误。  
  
PHOTO_LIBRARY_LOADING  
→ 首次/增量建立本地 PhotoAsset 索引。  
已有内容时优先显示已有照片并局部加载，不用全屏阻断。  
  
PHOTO_LIBRARY_ERROR  
→ 明确“无法读取照片”，不能显示成 Empty。  
  
五、Home 浏览状态  
  
BROWSE  
默认照片瀑布/网格流。  
照片按时间组织。  
普通照片不常驻复杂状态徽标。  
  
SELECTION_ACTIVE  
进入条件：用户对一张 PhotoAsset 发起选择动作。  
状态保存：  
selectedAssetIds = set  
动作：  
\- add/remove selection  
\- select range（产品语义）  
\- cancel  
\- open Protection Picker  
  
选择数量=0：  
自动退出 Selection → BROWSE。  
  
六、Protection Picker 状态  
  
PROTECTION_PICKER_OPEN  
输入：selectedAssetIds 非空。  
展示：  
Standard  
Important  
Private  
Private + Important  
  
用户 Cancel：  
关闭 Picker，回 SELECTION_ACTIVE；选择保留。  
原因：避免误触导致重新选择。  
  
用户选择某 Mark：  
对所有 selectedAssetIds 幂等写入/更新 ProtectionAssignment。  
立即触发 Evaluation。  
然后：  
\- 若所有 Assignment 当前不需要动作/已有满足：结束选择→BROWSE，刷新状态。  
\- 若存在可自动执行路径：进入任务执行，结束选择→BROWSE；后台状态为 PENDING。  
\- 若缺必要 Storage：进入 STORAGE_REQUIRED_PROMPT。  
  
七、Storage Required 状态  
  
STORAGE_REQUIRED_PROMPT  
必须明确：  
“这些照片已经标记完成，但当前没有可用于此保护方式的存储。”  
  
动作：  
A. Configure Storage  
→ 保存 ResumeContext  
→ Plugin Store  
  
B. Not Now  
→ Assignment 保留  
→ Home  
→ 对应 PhotoAsset = NEEDS_ATTENTION  
→ 后续可从问题入口继续。  
  
ResumeContext 至少逻辑保存：  
\- originatingAssignmentIds  
\- reason = noEligibleStorage  
\- requiredCapabilities / policy requirement  
  
不得保存成纯 UI 页面栈；Core 应能重新 Evaluation。  
  
八、Plugin Store 状态  
  
MARKETPLACE_LOADING  
从 Marketplace Plane 获取 PluginCatalogEntry。  
  
MARKETPLACE_READY  
支持 browse/search。  
  
MARKETPLACE_OFFLINE / ERROR  
已有 InstalledPlugin/StorageInstance 不受影响。  
用户可 Retry。  
不得阻断 Home 或现有 Protection。  
  
PLUGIN_DETAIL  
展示 T01 规定的信息。  
状态按钮：  
INSTALL  
INSTALLING  
INSTALLED / CONFIGURE  
UPDATE（若适用）  
UNAVAILABLE（撤回/不兼容）  
  
九、Plugin Install 状态  
  
PLUGIN_INSTALL_REQUESTED  
→ 验证 Catalog Entry/包信息（具体技术后置）  
→ INSTALLING  
  
成功：  
INSTALLED  
→ 自动进入 Activation/Configuration（若来自 Storage Required ResumeContext）。  
  
失败：  
INSTALL_FAILED  
→ 显示事实型原因类别  
→ Retry / Back  
→ ResumeContext 保留。  
  
插件安装成功不能使任何 StorageInstance READY。  
  
十、Storage Activation / Configuration 状态  
  
INSTANCE_DRAFT  
基于 InstalledPlugin 新建配置草稿。  
  
CONFIGURING  
用户填写/完成 Provider 所需认证与参数。  
  
CONFIG_INVALID  
缺字段/格式不满足 schema。  
不能 Test。  
  
CONFIGURED  
结构完整，可 Test。  
  
用户 Back：  
若有未保存配置，明确 Save Draft / Discard（具体 UI T05）。  
ResumeContext 仍保留。  
  
十一、Capability Test 状态  
  
TESTING  
测试至少逻辑包含：  
AUTH  
ACCESS/READ（如 Provider 模型需要）  
WRITE  
VERIFY  
以及 T06 Policy 所需 Capability。  
  
TEST_PASS  
→ StorageInstance = READY  
→ 记录 Capability snapshot  
→ 自动 Re-evaluate 所有因该 Instance 可满足的待处理 Assignment，优先 ResumeContext。  
  
TEST_PARTIAL  
能连接但缺 Policy 必要 Capability。  
→ StorageInstance 不得作为该 Policy eligible target。  
→ UI 明确“连接成功，但缺少 X 能力”。  
是否允许保存为非 Ready/limited Instance 由 T06 定义。  
  
TEST_FAIL_AUTH  
→ AUTH_EXPIRED/MISCONFIGURED  
→ Edit Credentials。  
  
TEST_FAIL_NETWORK/PROVIDER  
→ UNAVAILABLE  
→ Retry；不要求重输配置。  
  
TEST_FAIL_WRITE / VERIFY  
→ MISCONFIGURED 或 CAPABILITY_INSUFFICIENT  
→ 显示对应修复动作。  
  
十二、恢复原 Protection Intent  
  
StorageInstance READY 后：  
不返回空白 Home 让用户重新操作。  
Core：  
→ 找到 pending Assignment/ResumeContext  
→ Re-evaluate  
→ 生成 DesiredCopy  
→ 进入执行  
  
UI：  
可回 Home 并显示“正在保护 N 项”。  
ResumeContext 只负责体验连续性；真正是否需要执行由 Evaluation 决定，避免重复任务。  
  
十三、执行状态机（产品级）  
  
PLANNING  
根据 Assignment + Policy + Actual State 算差额。  
  
WAITING_FOR_SOURCE  
源暂不可读但可自动等待/重试。  
  
ENCRYPTING  
仅 Private 路径。  
  
ENCRYPTION_FAILED_RETRYABLE  
自动重试仍有意义 → PENDING。  
  
ENCRYPTION_NEEDS_ATTENTION  
密钥/本地材料需要用户处理。  
绝不明文降级。  
  
QUEUED  
任务等待执行。  
  
TRANSFERRING  
  
TRANSFER_RETRY_WAIT  
网络/Provider 临时失败，系统仍有自动恢复计划。  
  
UPLOADED_UNVERIFIED  
  
VERIFYING  
  
VERIFY_RETRY_WAIT  
验证暂时无法完成，但可自动恢复。  
  
VERIFIED  
该 RemoteCopy 可按 Policy 计数。  
  
JOB_FAILED_TERMINAL  
当前 Job 无自动恢复路径；触发 Evaluation 决定整体 NEEDS_ATTENTION / DEGRADED 等。  
  
十四、ProtectionState 转换  
  
UNASSIGNED  
→ 用户打标  
→ Evaluation  
  
若 Policy 当前 minimumRemoteCopies=0 且条件满足：  
→ PROTECTED 或产品可选择不强调 Protected；T05 定文案。  
  
若缺 Storage：  
→ NEEDS_ATTENTION  
  
若有自动路径：  
→ PENDING  
  
PENDING  
→ 全条件满足 → PROTECTED  
→ 自动路径耗尽且 0/不足有效结果 → NEEDS_ATTENTION  
→ 已有部分有效保护但不满足 → DEGRADED  
  
PROTECTED  
→ 用户改 Policy，现状不足 → PENDING/NEEDS_ATTENTION  
→ 明确发现 RemoteCopy invalid/missing → DEGRADED/NEEDS_ATTENTION  
→ Provider 暂时不可达但没有证据副本丢失：保持最近已知事实，标 verification stale（不立即判丢失）  
  
DEGRADED  
→ 自动补齐中 → PENDING（UI 可继续提示当前部分保护）  
→ 补齐成功 → PROTECTED  
→ 无自动路径 → NEEDS_ATTENTION  
  
NEEDS_ATTENTION  
→ 用户完成所需动作  
→ Re-evaluate  
→ PENDING 或直接 PROTECTED  
  
十五、关键异常  
  
1\. Marketplace 断网  
只影响安装/更新插件。  
已安装插件和本地 StorageInstance 继续工作。  
  
2\. Provider 临时离线  
Transfer 进入 retry wait。  
已有 verified RemoteCopy 不被删除。  
  
3\. Credential 失效  
StorageInstance → AUTH_EXPIRED。  
所有依赖它的新任务停止创建/执行。  
已有 RemoteCopy 记录保留。  
用户重新认证→Test→READY→Re-evaluate。  
  
4\. 插件撤回  
Catalog 标 REMOVED。  
本地已安装版本默认不自动删除。  
是否允许继续运行取决于撤回原因/本地安全策略，T06/T09 冻结。  
绝不能因为 Marketplace 撤回直接删除用户 RemoteCopy 记录或 Credential。  
  
5\. App/Core 中断  
所有非终态 Job 必须能从持久化事实恢复。  
不能依赖页面仍然存在。  
T07/T08 定幂等恢复。  
  
6\. 用户改标  
立即保存新 Assignment。  
取消/补充 Desired State。  
已存在 RemoteCopy 不自动删除。  
  
7\. 用户取消当前 Transfer  
取消 Job ≠ 删除 Assignment。  
Evaluation 后若 Policy 仍不满足 → NEEDS_ATTENTION 或可再次执行。  
如果用户真正想取消保护意图，应修改 ProtectionMark/Assignment。  
  
8\. 部分成功  
绝不能用批任务整体 success 掩盖。  
每个 PhotoAsset 独立 Evaluation。  
  
9\. Private 加密失败  
停止在本地。  
Provider 不收到明文。  
状态 PENDING/NEEDS_ATTENTION。  
  
10\. Upload 成功、Verify 失败  
RemoteCopy=UNVERIFIED/INVALID。  
不计入 Policy。  
可重试验证或重传。  
  
十六、问题恢复入口  
Home 不增加永久复杂 Tab。  
当存在 NEEDS_ATTENTION / DEGRADED 时，Home 出现聚合的非侵入问题入口，例如：  
“3 项需要处理”。  
  
点击进入 Problem List（T05 详细设计）。  
问题按“用户能执行的修复动作”聚合，而不是按底层错误码：  
\- Configure storage  
\- Reconnect provider  
\- Fix storage configuration  
\- Retry protection  
\- Restore encryption access  
等。  
  
十七、状态覆盖验证  
  
场景 A：首次用户  
Launch→Access Unknown→Allow→Loading→Browse。  
PASS。  
  
场景 B：拒绝权限  
Unknown→Denied→Home 保持→重新授权→Available→Browse。  
PASS。  
  
场景 C：选 Private、无 Storage  
Browse→Selection→Picker→Assignment saved→Needs Attention→Storage
Prompt→Store→Install→Configure→Test PASS→READY→自动恢复
Assignment→Encrypt→Transfer→Verify→Protected。  
PASS。  
  
场景 D：配置到一半退出  
ResumeContext + Assignment 均保留。  
回 Home 后仍显示 Needs Attention；再次进入继续配置。  
PASS。  
  
场景 E：上传后断网  
Uploaded Unverified→Verify Retry Wait；不显示 Protected。  
网络恢复→Verify→Protected。  
PASS。  
  
场景 F：Provider Credential 失效  
Ready→Auth Expired→Needs Attention→Reconnect→Test→Ready→Re-evaluate→恢复。  
PASS。  
  
场景 G：两个资产批量处理，一个成功一个失败  
Asset 1 独立 Protected。  
Asset 2 Pending/Needs Attention。  
批量 UI 不得统一显示“全部成功”。  
PASS。  
  
十八、对后续任务的约束  
T05：必须给以上每个用户可见状态明确页面/Sheet/反馈。  
T06：必须定义 TEST_PARTIAL、Capability snapshot、撤回/升级语义。  
T07：ResumeContext 不能成为唯一事实源；Assignment/Job 状态必须持久化。  
T08：中断恢复、幂等、Retry 分类必须落实。  
T09：插件撤回、Credential、Key、日志边界必须进入 Threat/Trust Model。  
  
十九、闭环结论  
所有关键状态均有进入条件、退出条件和恢复路径。  
不存在“配置失败后从头来”“杀掉 App 后意图消失”“上传成功就误报 Protected”“Private 加密失败降级明文”等悬空状态。  
  
T04：PASS。


```

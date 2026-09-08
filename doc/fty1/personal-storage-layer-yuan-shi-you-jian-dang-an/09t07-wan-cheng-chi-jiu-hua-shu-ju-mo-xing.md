# 09｜【T07 完成｜持久化数据模型】

**邮件主题：** RE: 【项目立项】Personal Storage Layer — Mobile Photos V1｜T01–T10 主线程

**收到时间：** 2026-09-05T17:10:54Z

## 邮件正文原文

```
【T07 完成｜持久化数据模型】  
  
验收结论：PASS  
  
一、目标  
把 T02–T06 的 Domain 转成可持久化、可恢复、可幂等执行的数据结构。这里定义产品级 Schema 和约束，不绑定
SQLite/CoreData/Room 等平台数据库。  
  
二、唯一决策  
Core 使用“本地事实表 + 可重算派生状态”的模型。ProtectionState、问题聚合等可以缓存，但永远可从
Assignment、Policy、RemoteCopy、VerificationRecord、Job 等事实重新计算。不得只保存一个
isProtected=true。  
  
三、核心表/集合  
1\. photo_asset  
asset_id：稳定本地
ID；source_ref；media_kind；created_at；discovered_at；source_state；source_fingerprint；metadata_ref；updated_at。  
禁止把 Provider remote path 当 asset_id。  
  
2\. protection_assignment  
assignment_id；asset_id UNIQUE；importance standard|important；privacy
standard|private；policy_version；created_at；updated_at。  
同一 Asset 当前最多一个 Assignment。批量打标是多条独立幂等更新。  
  
3\. protection_policy  
policy_id；policy_version；minimum_verified_remote_copies；minimum_independent_storage_instances；client_side_encryption_required；verification_required；freshness_requirement；required_capabilities；deletion_rule。  
V1 默认 Policy 是本地产品配置，不由 Marketplace 远程静默修改。  
  
4\. installed_plugin  
plugin_id；installed_version；protocol_version；manifest_snapshot；install_state；installed_at；updated_at。  
  
5\. storage_instance  
instance_id；plugin_id；display_name；config_schema_version；non_secret_config；credential_ref；state；capability_snapshot_id；config_revision；created_at；updated_at。  
secret 不得进入 non_secret_config。  
  
6\. capability_snapshot  
snapshot_id；instance_id；plugin_version；config_revision；tested_at；capabilities；limits；verification_methods；result；warnings。  
只对完全相同 pluginVersion + configRevision 有效；任一变化必须 needsRetest。  
  
7\. remote_copy  
remote_copy_id；asset_id；instance_id；remote_locator；payload_kind
plaintext|client_encrypted；encryption_artifact_id
nullable；provider_version；expected_fingerprint；observed_fingerprint；size；state
transferred_unverified|verified|stale|missing|invalid；created_at；last_verified_at；updated_at。  
唯一约束至少包含 asset + instance + 当前 logical object identity，避免重复副本记录。  
  
8\. transfer_job  
job_id；operation_id
UNIQUE；asset_id；desired_copy_key；instance_id；assignment_revision；job_type
encrypt|upload|verify|delete_explicit
等；state；attempt_count；next_retry_at；last_error_category；created_at；started_at；finished_at；updated_at。  
operation_id 是跨重启幂等键。  
  
9\. verification_record  
verification_id；remote_copy_id；operation_id；method；result
pass|fail|inconclusive；expected_fingerprint；observed_facts；verified_at；error_category。  
历史 append-only；不得用新失败覆盖旧事实。  
  
10\. encryption_artifact  
artifact_id；asset_id；source_fingerprint；encryption_format_version；key_ref；ciphertext_fingerprint；ciphertext_size；state；created_at。  
这里不保存 key bytes，只保存 key_ref。  
  
11\. local_key_record  
key_ref；key_scope；key_version；key_state；created_at；必要恢复元数据。  
实际 key material 必须与普通数据库逻辑隔离；T09 冻结恢复模型。  
  
12\. resume_context  
resume_id；reason；assignment_ids；required_capabilities；created_at；state。  
仅用于 UX 连续性。即使它丢失，Core 也必须通过 NEEDS_ATTENTION Assignment 重新发现未完成工作。  
  
13\. problem_projection  
可选派生缓存：problem_type；affected_asset_ids/instance_id；recommended_action；computed_at。  
不是事实源，可全部删除重建。  
  
四、Revision / 并发规则  
每个 Assignment、StorageInstance config 都有 revision。  
Planner 生成 Job 时记录 assignment_revision/config_revision。  
Job 真正执行或完成写回前必须确认目标 revision 仍匹配；若用户期间改标/改配置，旧结果可作为 RemoteCopy
事实保存，但不能自动宣称满足新 Policy。  
  
五、幂等规则  
1\. 用户重复选择同样 Mark：只更新时间/保持同一语义，不创建重复任务。  
2\. Planner 对同一 asset + desiredCopy + assignmentRevision 生成稳定
desired_copy_key。  
3\. Transfer operation_id 唯一；重试复用同一逻辑 operation，不创建无限重复远端对象。  
4\. Provider 返回超时但实际已写入：恢复时先 stat/verify，确认已有正确对象则复用，不盲目再次上传。  
5\. Verification 可多次执行，每次产生新 Record；RemoteCopy 当前摘要取最新可信事实。  
  
六、事务边界  
必须原子完成的产品操作：  
\- 修改 ProtectionAssignment + 增加待重新 Evaluation 标记。  
\- Storage config 更新 + config_revision 增长 + CapabilitySnapshot 失效。  
\- Job 状态转移 + 对应 attempt/error 信息。  
\- Verify PASS 写 VerificationRecord + 更新 RemoteCopy verified 摘要。  
  
不得要求“Assignment 写入”和远端上传在同一个事务；远端是不可事务化外部系统，因此必须采用可恢复任务语义。  
  
七、Credential/Key 隔离  
普通可查询/导出的 Core DB 只保存 credential_ref、key_ref。  
日志、Problem、Analytics（当前无服务端 analytics）不得包含 secret/key。  
插件配置导出不得把 secret 混进 JSON。  
删除 StorageInstance 时先检查 credential_ref 是否被其他 Instance 引用。  
  
八、Local-only 约束  
以上全部用户数据默认只在本地。  
Marketplace 服务端只能拥有 Catalog 数据，不能同步这些表。  
未来即使增加多设备能力，也不能在没有新产品决策的情况下把本 Schema 上传我们的后台。  
  
九、杀进程/重启恢复验证  
启动时执行 Recovery Scan：  
\- 找出非终态 Job。  
\- 将 executing 类状态恢复为 resumable/reconcile，而不是直接 success/fail。  
\- 对可能已经到 Provider 的 upload 先 stat/verify。  
\- 重新计算所有受影响 Assignment。  
结果：页面栈丢失不影响意图和任务。  
PASS。  
  
十、重复提交验证  
同一 Asset 连续三次标 Private：  
assignment 仍只有一个当前记录；revision 仅在语义变化时增长或按实现策略更新，但 Planner 的 desired key
不产生三份重复目标；已有正确密文 verified copy 可复用。  
PASS。  
  
十一、部分成功恢复  
Asset Important，需要一个副本：upload 成功后 Core 崩溃，Provider 已有对象但本地 Job 仍 transferring。  
重启→reconcile→stat/verify→确认对象正确→写 RemoteCopy/Verification→Job
completed→Evaluation→Protected。  
不重复上传。  
PASS。  
  
十二、Provider 重复响应  
同一 operation 收到重复 success callback/结果：operation_id UNIQUE + RemoteCopy logical
uniqueness 保证写入幂等；不会多加 verifiedCopies。  
PASS。  
  
十三、改标场景  
Private 已有 verified encrypted RemoteCopy，用户改 Standard：旧 RemoteCopy 保留；新
Evaluation 判断是否需要新动作，不自动删除。  
Standard 明文 RemoteCopy 后改 Private：payload_kind=plaintext，因此不能计入
Private；Planner 新建 encrypted desired copy。  
PASS。  
  
十四、迁移原则  
所有持久化实体必须带 schema/version 迁移能力。  
迁移规则：先备份/可回滚本地事实；不可静默丢 RemoteCopy/Verification/Assignment；无法迁移 Credential
时必须进入 Needs Attention，不得假装 Ready。  
  
十五、索引/查询要求  
至少优化：  
asset_id；ProtectionState 派生所需 asset→assignment/copies；instance_id→remote
copies/jobs；job state + next_retry_at；problem aggregation；plugin_id→instances。  
具体数据库索引语法后置。  
  
十六、验证方法  
已覆盖：杀死/重启、重复打标、重复 Provider success、上传成功但本地未落完成、部分成功、改标、Credential 修改、Plugin
更新。  
所有场景都可从事实恢复，不依赖 UI 页面或单一布尔状态。  
  
十七、闭环条件  
任意 Transfer/Verification 结果都能持久化；任意时刻可重新计算
ProtectionEvaluation/ProtectionState；用户意图在 App 中断后不丢；重复执行不制造逻辑重复副本。  
  
十八、对后续任务  
T08 必须实现 Recovery Scan、Reconcile、operationId、差额 Planner。  
T09 必须冻结 key_ref/credential_ref 背后的信任与恢复边界。  
T10 Acceptance Tests 必须包含 crash-before/after-provider-success、duplicate
response、revision changed mid-flight。  
  
T07：PASS。


```

# 10｜【T08 完成｜核心数据流与任务执行语义】

**邮件主题：** RE: 【项目立项】Personal Storage Layer — Mobile Photos V1｜T01–T10 主线程

**收到时间：** 2026-09-05T17:11:30Z

## 邮件正文原文

```
【T08 完成｜核心数据流与任务执行语义】  
  
验收结论：PASS  
  
一、目标  
冻结 Select→Mark→Resolve Policy→Plan→Encrypt(if
required)→Transfer→Verify→Evaluate 的执行链，尤其解决崩溃恢复、重复执行、部分成功、网络切换、取消和 Provider
不确定响应。  
  
二、唯一执行模型  
Core 使用 Desired State Reconciliation，而不是“用户点一次就执行一条不可恢复脚本”。任何时刻都可用：Assignment
+ Policy + 当前 RemoteCopies/Verification + Ready StorageInstances → 计算缺口 →
生成最小动作。  
  
三、标准流程  
1 用户打标，先持久化 Assignment。  
2 Resolve Policy，得到机器约束。  
3 Evaluate Actual State。  
4 Planner 只生成差额 DesiredCopy。  
5 Private：先生成 EncryptionArtifact。  
6 为每个差额创建稳定 operationId 的 TransferJob。  
7 Plugin putObject。  
8 无论 Provider 返回 success 还是超时，都进入 reconcile/verify 逻辑。  
9 stat/read/verify 得到 PASS 才写 VERIFIED。  
10 Re-evaluate；所有条件满足才 PROTECTED。  
  
四、Planner 规则  
\- 已有满足新 Policy 的 verified RemoteCopy 必须复用。  
\- 不因重复打标重传。  
\- 不因 App 重启重传。  
\- StorageInstance 不 READY 不得成为新 DesiredCopy target。  
\- Private 不能复用 plaintext RemoteCopy。  
\- 改 Policy 只补差额，不自动删除多余副本。  
  
五、Private 数据流  
Source plaintext→Core encryption→ciphertext fingerprint→opaque remote
key→Plugin→Provider。  
Encryption 失败：任务停止本地，禁止创建 plaintext upload。  
加密算法/密钥恢复由 T09 冻结，但产品执行顺序不可改变。  
  
六、上传语义  
putObject success 只表示 Provider 声称接收完成。  
之后状态必须 UPLOADED_UNVERIFIED。  
Provider timeout/connection drop 属于 UNKNOWN OUTCOME：不得直接判失败并重新创建另一对象；先 stat
remote logical key。  
  
七、验证语义  
verify 只有 PASS 可计入 Policy。  
FAIL：明确不匹配/不存在，进入修复或重传。  
INCONCLUSIVE：证据不足，不计数，可重试。  
验证方法和 observed facts 写 VerificationRecord。  
  
八、Retry 分类  
自动重试：NETWORK_UNAVAILABLE、PROVIDER_UNAVAILABLE、RATE_LIMITED、部分 transient
WRITE/READ/VERIFY。  
用户动作：AUTH_EXPIRED、TARGET_NOT_WRITABLE、CONFIG_INVALID、QUOTA_EXCEEDED（若无法自动释放/等待）、缺
Storage。  
终止：明确不支持 capability、对象永久超限且无法调整等。  
Retry 使用退避/Provider retry hint；具体秒数属于实现，不在产品层写死。  
  
九、Cancel  
取消 TransferJob 只停止当前执行，不删除 Assignment、不删除已存在 RemoteCopy。  
取消后立即 Re-evaluate：若 Policy 不满足且用户没有撤销保护意图，则显示 Needs Attention/可重新执行。  
“取消保护”必须通过 Change Protection，而不是 Cancel Job。  
  
十、网络切换/Provider 离线  
任务进入 retry wait；Home 保持可用；已有 verified copy 不失效。网络恢复后 Scheduler 重新
reconcile。不能要求用户重新选择照片。  
  
十一、Crash Recovery  
启动 Recovery Scan：  
\- queued/retry_wait：继续调度。  
\- encrypting：检查 artifact 是否完整；不完整则安全重建。  
\- transferring：视为 outcome unknown，先 stat/verify。  
\- uploaded_unverified/verifying：继续 verify。  
\- completed：不重复执行。  
每一步依赖持久化 operationId/revision。  
  
十二、部分成功  
批量 100 张：每个 Asset 独立 Evaluation。97 Protected、3 Needs Attention，UI 必须真实显示
97/3，不能整个 batch 失败或整个 batch 成功。  
一个 Asset 多目标：一份 verified、一份失败时按 Policy 判断 DEGRADED/PENDING/NEEDS_ATTENTION。  
  
十三、Source 暂不可读  
Assignment 保留。若属于可自动恢复条件，WAITING_FOR_SOURCE/PENDING；若长期需要用户动作则
NEEDS_ATTENTION。绝不能把 Source 暂不可读解释为远端副本丢失。  
  
十四、Provider 明确对象丢失  
stat/verify 获得 OBJECT_NOT_FOUND，RemoteCopy→MISSING；Re-
evaluate。若有其他有效副本，Planner 可从可读 Source 或有效副本（未来支持）补齐；V1 默认从 Source 重建。  
  
十五、Provider 内容不匹配  
VERIFY FAIL + fingerprint mismatch→RemoteCopy INVALID，不计数。若 Source
可用，生成新的修复动作；不得把错误远端对象覆盖成 verified。  
  
十六、Credential 中途失效  
当前 Job→user_action；Instance→AUTH_EXPIRED；依赖它的未执行 Job 暂停。Reconnect+Capability
Test PASS 后 Re-evaluate，旧 Assignment/operation 继续或安全重建。  
  
十七、Plugin Update 中途发生  
正在执行的操作以已加载插件版本完成或安全中断，不能一半切换协议。更新后 Snapshot needsRetest；新任务等 Instance 重新
eligible。具体进程机制后置。  
  
十八、删除语义  
V1 Source 删除不传播 RemoteCopy。  
Change Protection 不删除 RemoteCopy。  
Remove Storage connection 不等于 delete remote data。  
只有用户明确“Delete remote copy/data”动作才能创建 delete job；是否纳入 V1 UI 可后置，但 Core 绝不隐式调用。  
  
十九、Given/When/Then 验证  
GIVEN Private Assignment、无 Storage WHEN 用户配置 GitHub Ready THEN
自动恢复→encrypt→upload ciphertext→verify→Protected。PASS。  
GIVEN upload 已到 Provider 但本地 crash WHEN restart THEN
stat/verify→复用，不重复对象。PASS。  
GIVEN verify inconclusive WHEN retry pending THEN 不 Protected。PASS。  
GIVEN user changes Private→Private+Important WHEN existing encrypted verified
copy 满足 THEN 复用，只补差额。PASS。  
GIVEN credential expires WHEN reconnect THEN 原 Assignment 自动继续。PASS。  
GIVEN encryption fails THEN Provider 收不到 plaintext。PASS。  
  
二十、闭环条件  
每个用户意图最终必须收敛到：PROTECTED，或一个明确可解释的 NEEDS_ATTENTION/DEGRADED
状态；不存在永远卡在页面内的隐式任务。所有执行都可从本地事实重建。  
  
二十一、对后续任务  
T09：冻结 Encryption Format/Key 生命周期/Recovery、插件供应链与 metadata trust boundary。  
T10：Acceptance Tests 必须覆盖 unknown outcome、crash、duplicate response、verify
inconclusive、cancel、revision change。  
  
T08：PASS。


```

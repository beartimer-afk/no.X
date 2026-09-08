# 01｜项目立项：Personal Storage Layer / Mobile Photos V1

**邮件主题：** 【项目立项】Personal Storage Layer — Mobile Photos V1｜T01–T10 主线程

**收到时间：** 2026-09-05T15:51:34Z

## 邮件正文原文

```
项目立项：Personal Storage Layer / Mobile Photos V1  
  
立项目标  
一期只做 iPhone，不注册、不登录我们的账号、不依赖我们的云服务。用户进入 App
即进入照片主页面；无照片权限时在主页面原位引导授权。授权后以瀑布流展示手机照片。用户单选/多选照片后，表达保护意图/安全程度；如果该策略尚无可用存储，则按需进入
Storage Plugin Store，安装插件、激活、配置、执行能力/连通性测试，再由策略驱动上传、验证并反馈保护状态。  
  
一期核心假设  
1\. 不同照片的价值、隐私和可接受存储成本不同。  
2\. 用户愿意用极低操作成本给少量“例外照片”表达不同保护意图。  
3\. App 的价值不是卖存储，而是屏蔽不同 Storage Provider，并可靠执行用户策略。  
4\. “已上传”不等于“已保护”；只有策略要求的副本/加密/验证条件全部满足，才能显示 Protected。  
5\. Private V1 必须包含客户端加密后上传；具体密码学与恢复模型需在研究任务中冻结。  
6\. V1 Reference Provider 先研究
S3-Compatible、WebDAV/NAS、一个中国主流云存储，最终实现数量以最小闭环为原则。  
  
执行方式  
本邮件作为唯一项目主线程。后续 T01–T10 每完成一个任务，都必须以“回复本邮件”的方式追加完整成果，不另起主题。邮件内容是后续低成本 AI
开发的权威上下文之一，因此不能只写摘要。  
  
统一任务完成标准（Definition of Done）  
每个任务都必须同时包含：  
A. 任务目标：解决什么问题，以及为什么 V1 必须现在解决。  
B. 输入与边界：依赖什么；明确做什么、不做什么，防止范围漂移。  
C. 市场/平台依据：涉及产品交互时参考成熟 App；涉及平台能力时优先使用官方资料。不能只凭主观设计。  
D. 决策结果：给出唯一推荐方案；备选方案只用于解释为什么没选，避免把选择题留给开发 AI。  
E. 详细规格：细到低成本 AI 无需自行补产品逻辑。  
F. 异常与边界状态：权限、断网、Provider 离线、凭证失效、任务中断、部分成功等与本任务有关的状态必须覆盖。  
G. 验证方法：明确“怎么证明这个任务定义正确/实现正确”，包含可执行检查步骤。  
H. 闭环条件：写清楚从触发→执行→结果→状态反馈→失败恢复，什么条件下才算闭环。  
I. 对后续任务的影响：新增/修改了哪些 Domain、State、UI、数据字段或约束。  
J. 验收结论：PASS / PASS WITH CONSTRAINTS / BLOCKED；BLOCKED 必须说明阻塞点，但不阻塞其他可继续任务。  
  
统一原则  
\- 避免模糊词：“安全”“同步完成”“大概”“适当”“视情况”等必须转换为可观察状态或明确规则。  
\- 避免“绝对安全/永不丢失”等无法保证的承诺。产品只陈述实际执行的保护措施和已验证状态。  
\- 费用必须区分插件费用、第三方 Provider 费用和估算；第三方实际账单以 Provider 为准。  
\- 不允许 Happy Path 即完成。失败、重试、恢复、幂等、部分完成必须进入定义。  
\- 开发 AI 不负责产品决策；规格必须给唯一推荐结果。  
\- 最小 App 优先；不影响 V1 核心闭环的能力进入二期 Backlog。  
  
T01｜V1 竞品与成熟交互机制研究  
目标：为照片瀑布流、多选/打标、Private/Protection、Storage 配置、插件市场、连接测试、同步/验证状态找到成熟范式。  
重点：Apple Photos / Google Photos / Ente；PhotoSync / Synology Photos / Mylio /
Immich；Cryptomator；成熟 Plugin/Integration Store。  
输出：Interaction Reference Matrix + 每个关键交互的唯一推荐方案。  
验证：每个 V1 核心交互至少有成熟产品/系统范式依据，并说明不能照搬的部分。  
闭环：结果必须直接约束 T03/T04/T05，而非停留在竞品介绍。  
  
T02｜核心 Domain Model 与术语冻结  
目标：统一
PhotoAsset、ProtectionMark、Policy、StoragePlugin、StorageInstance、RemoteCopy、TransferJob、VerificationRecord、ProtectionState
等概念。  
输出：实体定义、关系、生命周期、术语表、禁止混用词。  
验证：用“1 张照片→两个 Storage→一个失败→恢复”走完整 Domain，不得有概念歧义。  
闭环：任何 UI 状态可映射 Domain；核心 Domain 状态都有用户结果或后台用途。  
  
T03｜Protection Model / 策略模型  
目标：冻结用户如何表达普通、重要、私密，以及重要性与隐私二维属性如何在 V1 UX 中降维。  
输出：唯一 V1 交互模型；各策略的副本、加密、删除传播、验证要求；Policy Satisfied 精确定义。  
验证：普通截图、重要家庭照、私密且重要照片三个案例逐一推演。  
闭环：打标→Policy→Desired State→执行→验证→Satisfied/Degraded 无缺口。  
  
T04｜完整 UX / System State Machine  
目标：穷举首次启动到 Protected 的所有用户/系统状态。  
覆盖：照片权限 unknown/limited/denied/full；空图库；选择；未配置
Storage；插件安装/激活；测试；上传；后台中断；失败；重试；凭证过期；NAS 离线；部分成功等。  
输出：状态表、事件、转换条件、不可转换状态。  
验证：transition coverage；每状态必须有进入条件和退出路径。  
闭环：任何失败都有恢复动作，不得存在悬空状态。  
  
T05｜页面与交互规格  
目标：开发 AI 不再决定页面结构和交互。  
重点：首页瀑布流、选择态、Protection Picker、Storage/Plugin Store、Plugin
Detail、Activation/Configuration、Capability Test、Protection Problem/Status。  
输出：页面结构、组件、点击/手势、导航、Sheet/Push、Loading/Empty/Error/Disabled/Success、文案原则。  
验证：T04 所有用户可见状态逐一映射 UI。  
闭环：仅按 Spec 即可无猜测完成从首次打开到一张照片 Protected 的原型。  
  
T06｜Storage Plugin Spec  
目标：定义 Core 与 Provider 稳定边界，为未来开放协议留接口，但 V1 不过度平台化。  
输出：Metadata、安装/激活、认证/配置 Schema、Capabilities、费用、免责声明、Connection/Capability
Test、Core API 最小集合。  
验证：S3-Compatible、WebDAV/NAS、中国主流云三类 Provider 套模型。  
闭环：未安装→安装→配置→测试→Ready→失效→恢复完整可表达。  
  
T07｜持久化数据模型  
目标：把 Domain 转为可开发的数据结构。  
输出：实体字段、ID、关系、枚举、索引、持久化边界、Credential 隔离、幂等键、迁移考虑。  
验证：App 杀死/重启、重复提交、重复响应、部分成功恢复后状态不得矛盾。  
闭环：Transfer/Verification 结果可靠落库并可重新计算 ProtectionState。  
  
T08｜核心数据流与任务执行语义  
目标：定义 Select→Mark→Resolve Policy→Plan→Transfer→Verify→Evaluate。  
输出：Happy Path、Partial Success、Failure、Retry、Cancel、网络切换、后台恢复、删除语义；明确 Uploaded
与 Verified。  
验证：关键流 Given/When/Then；重复执行满足幂等。  
闭环：只有 Verification 达到 Policy 要求才进入 Protected。  
  
T09｜iOS 最新能力边界验证  
目标：证明 V1 承诺在当前 iPhone/iOS 能否可靠兑现。  
覆盖：PhotoKit、Limited Library、iCloud Photos 原片、HEIC/RAW/Live
Photo/Video、后台传输、App 被杀、网络/电量、局域网/NAS、Keychain、E2EE 等。  
输出：SUPPORTED / SUPPORTED WITH CONSTRAINTS / CANNOT GUARANTEE 矩阵，并反向修改产品承诺。  
验证：关键能力优先 Apple 官方资料与最小技术 PoC 条件，不以“理论可行”替代。  
闭环：所有 V1 承诺对应平台能力；不能保证的必须修改 UX/状态/文案。  
  
T10｜最终 Development Package + Acceptance Tests  
目标：汇总 T01–T09 成为可直接交给低成本 AI 实施的单一开发包。  
输出：README/产品目标、V1 Scope、User Flow、Screen Map、Interaction Spec、State
Machine、Domain Model、Data Model、Plugin Protocol、Data Flow、Error/Edge Cases、iOS
Constraints、Acceptance Tests、二期 Backlog。  
验证：“无上下文开发 AI 测试”——仅提供开发包，若仍需提出关键产品问题，则规格未完成。  
闭环：每项核心能力都有可执行 Acceptance Test，全部通过才称 V1 功能完成。  
  
最终总体验收场景  
首次安装，无账号→主页面请求照片权限→授权→照片瀑布流→多选→设置保护意图→发现缺少
Storage→插件市场→查看插件功能/第三方费用说明→安装→激活/配置→Capability Test→绑定策略→传输→按策略加密→验证→重新计算
ProtectionState→主界面显示真实结果。任一步失败都有明确状态和恢复路径。  
  
本邮件从此作为项目立项与 T01–T10 唯一任务记录主线程。


```

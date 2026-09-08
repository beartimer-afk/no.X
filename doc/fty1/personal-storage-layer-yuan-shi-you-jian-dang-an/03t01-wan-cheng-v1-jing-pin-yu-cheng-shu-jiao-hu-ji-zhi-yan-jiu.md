# 03｜【T01 完成｜V1 竞品与成熟交互机制研究】

**邮件主题：** RE: 【项目立项】Personal Storage Layer — Mobile Photos V1｜T01–T10 主线程

**收到时间：** 2026-09-05T15:59:49Z

## 邮件正文原文

```
【T01 完成｜V1 竞品与成熟交互机制研究】  
  
验收结论：PASS  
  
一、任务目标  
为 V1 的关键交互寻找成熟范式，避免开发 AI 自行发明交互。重点不是复制某个产品，而是明确：  
1\. 主页面如何展示照片。  
2\. 如何进入多选并批量打标。  
3\. 如何表达 Private / Important。  
4\. 没有可用存储时如何按需引导。  
5\. 插件市场如何浏览、搜索、查看详情、安装、激活。  
6\. Storage 配置完成后如何测试并进入 Ready。  
7\. 用户如何理解“已处理/已保护/未完成”。  
  
二、输入与边界  
本任务仅定义产品交互范式，不讨论 iOS / Android / Desktop 的平台差异。  
不做：完整相册、搜索、人脸、编辑、社交、账号体系、云端用户数据。  
一期唯一真实 Reference Plugin 为 GitHub，但交互不得写成“GitHub 专用产品”。  
  
三、成熟产品参考与可复制点  
  
A. Google Photos —— 照片浏览与批量选择  
成熟机制：  
\- 照片主视图直接展示照片。  
\- 长按第一张进入选择态。  
\- 进入选择态后可继续逐张点选。  
\- 可通过持续拖动快速选择连续照片。  
来源：  
<https://support.google.com/photos/answer/6220402>  
  
可复制：  
\- “先浏览、后长按进入多选”的低学习成本范式。  
\- 多选状态覆盖在同一个照片流上，不跳新页面。  
\- 选择后再出现动作，而不是每张照片常驻大量按钮。  
  
不复制：  
\- Google Photos 的编辑、分享、删除等动作与本产品核心无关。  
  
唯一决策：  
V1 主页面采用“瀑布/网格照片流 + 长按进入多选 + 点击继续增减选择”的范式。  
不提供独立“选择照片”页面。  
  
B. Ente Gallery Mode —— 无账号进入本地图库  
成熟机制：  
Ente 已提供“Continue without account”的 Gallery Mode；授权后直接显示设备照片，按日期组织。  
来源：  
<https://ente.com/help/photos/getting-started/gallery-mode>  
  
可复制：  
\- 不把注册/登录放在照片之前。  
\- 本地图库本身就是产品入口。  
\- 用户先看到自己的数据，再发生后续动作。  
  
唯一决策：  
V1 启动后直接进入 Home。  
如果没有照片可见权限，Home 原位显示权限空状态；不出现 Onboarding Carousel，不出现账号入口。  
  
C. Ente Hidden / App Lock —— 敏感内容的动作与“可见性”是独立概念  
成熟机制：  
\- 用户可以选中照片后执行 Hide。  
\- Hidden 内容不会出现在普通时间线、搜索、普通相册等可见位置。  
\- 需要额外认证后才能查看。  
\- Ente 的文件在设备端加密后上传，Provider/Server 不得到明文。  
来源：  
<https://ente.com/help/photos/features/albums-and-organization/hide>  
<https://ente.com/help/photos/faq/security-and-privacy>  
<https://ente.com/help/photos/features/account/app-lock>  
  
可复制：  
\- “Private”应是用户主动选择后的明确状态。  
\- Private 不能只是标签；必须对应真实数据处理规则。  
\- Private 的状态表达需要与普通照片区分，但不应在主照片流里泄露过多敏感信息。  
  
不复制：  
\- Ente 的账号、Ente 云端、账户密码派生体系不是我们的产品模型。  
\- 我们的 Core 永久纯本地，Provider 通过插件连接。  
  
唯一决策：  
V1 中 Private 作为用户保护意图之一，触发“上传前本地加密”。  
Private 是否同时隐藏主流中的缩略图，由 T03/T05 决定；T01 不提前混淆“存储隐私”和“界面隐藏”。  
  
D. PhotoSync —— “选择→目标→传输”与 Provider 配置  
成熟机制：  
PhotoSync 支持用户选择照片后选择目标；不同 WebDAV/NAS 目标可各自配置；传输过程中展示详细进度。  
来源：  
<https://www.photosync-app.com/support/nas/answers/how-to-transfer-using-
webdav>  
<https://www.photosync-app.com/support/nas>  
  
可复制：  
\- Storage 是“已配置目标实例”，不是抽象 Logo。  
\- 同一个 Provider 可以有多个配置实例。  
\- 配置成功后目标可以复用，不要求每次重新配置。  
\- 传输是后台机制，但发生问题时需要可查看详情。  
  
不复制：  
\- PhotoSync 核心动作仍偏“用户选择目标后传输”，我们的产品核心是“用户选择保护意图，Policy 决定目标”。  
\- 不让用户每次打标后再选 Storage。  
  
唯一决策：  
Storage 配置与照片打标分离。  
用户在照片上只表达 Protection Mark；Core 根据 Policy 自动寻找已配置 StorageInstance。  
  
E. Synology Photos —— 照片备份与“额外副本”的用户语义  
成熟机制：  
Synology Photos 把手机照片备份到用户自己的 NAS，并强调额外副本和备份状态。  
来源：  
<https://www.synology.com/en-global/dsm/feature/photos>  
  
可复制：  
\- 用户不需要理解“复制任务 ID”，而需要理解“此数据是否已有额外副本”。  
\- 后台细节应退居二级页面。  
  
不复制：  
\- 单一 NAS 目的地模型。  
\- 自动备份全部照片的默认假设。  
  
唯一决策：  
Home 的反馈不显示技术型 Transfer Queue 为第一信息；只显示与用户意图相关的 ProtectionState。  
详细传输状态只在异常/详情中出现。  
  
F. VS Code Extension Marketplace —— 插件商店信息架构  
成熟机制：  
\- 单独的 Extensions / Marketplace 视图。  
\- 支持搜索、分类/筛选。  
\- 卡片显示名称、简要描述、发布者、安装量/评分等基本信息。  
\- 点进详情页了解功能。  
\- 详情页可安装。  
\- 安装完成后从 Install 变为 Manage。  
\- 第三方发布者安装时明确提示信任风险。  
来源：  
<https://code.visualstudio.com/docs/configure/extensions/extension-
marketplace>  
  
可复制：  
\- Plugin Store = Browse/Search → Detail → Install → Configure/Activate →
Ready。  
\- 插件详情应先解释能力、用途、费用、发布者，再让用户安装。  
\- 插件安装与 StorageInstance 配置是两步，不混为一个状态。  
\- 第三方插件必须明确 Publisher / Trust / Data Access 能力。  
  
不复制：  
\- 下载量、评分一期不作为必要字段。  
\- 不做复杂社区评论。  
\- 不允许插件拥有无限 Core 权限；T06 必须定义最小能力授权。  
  
唯一决策：  
Plugin Store 的产品范式采用“应用/扩展商店”，而不是 Settings 中一串 Provider Logo。  
  
四、V1 Interaction Reference Matrix  
  
1\. 首次进入  
参考：Ente Gallery Mode  
决策：直接 Home；无照片权限时原位请求；无注册/登录。  
  
2\. 照片浏览  
参考：Google Photos / Ente  
决策：按时间组织的照片瀑布/网格流；主页面就是数据本身。  
  
3\. 多选  
参考：Google Photos  
决策：长按第一张进入选择态；点击增减；支持连续范围选择的产品语义（具体平台手势后置）。  
  
4\. 打标  
参考：Ente Hide 的“selection action”  
决策：用户先选照片，再调用 Protection Picker；不在每张图常驻大量控件。  
  
5\. Storage 缺失  
参考：渐进式配置原则 + PhotoSync 目标配置  
决策：用户第一次选择某 Protection Mark 且其 Policy 无可用 Storage 时，才引导配置 Storage；首次启动不预配置。  
  
6\. Plugin Store  
参考：VS Code Extension Marketplace  
决策：Browse/Search → Detail → Install → Activate/Configure。  
  
7\. Plugin Detail  
参考：Extension Marketplace  
必须展示：  
\- 名称  
\- Publisher  
\- 一句话功能  
\- 适用场景  
\- 所需访问能力  
\- 插件自身费用  
\- 第三方 Provider 费用说明  
\- 隐私/数据访问说明  
\- Install 状态  
  
8\. Storage Activation  
参考：PhotoSync Provider Configuration  
决策：安装 Plugin 后还必须建立 StorageInstance；完成 Credential/参数配置后才能 Test。  
  
9\. Capability / Connection Test  
参考：成熟 Provider 配置工具的“配置后验证”  
决策：Test 不是只返回“Connected”。  
至少逻辑上区分：  
\- Authentication  
\- Read/Access  
\- Write  
\- Verify  
具体 Capability 在 T06 冻结。  
测试通过后 StorageInstance = Ready。  
  
10\. Protection Feedback  
参考：Synology Photos 的 Backup 状态，但语义升级  
决策：用户看到的是 ProtectionState，不把“上传成功”伪装为 Protected。  
只有 Policy 条件满足才可显示 Protected。  
  
五、明确不采用的交互  
  
1\. 首次启动强制注册。  
原因：与 Local-only 核心原则冲突，且在用户看到价值前制造阻力。  
  
2\. 首次启动先配置 Storage。  
原因：用户还没有保护意图；配置成本过早。  
  
3\. 每张照片旁常驻 Normal/Important/Private 三按钮。  
原因：视觉噪音高，迫使用户整理全部照片。  
  
4\. 用户每次打标后手动选择目标 Storage。  
原因：把 Policy Engine 退化成传统传输工具。  
  
5\. “Sync Complete = Protected”。  
原因：没有验证 Policy Desired State。  
  
6\. 插件安装完成即视为可用 Storage。  
原因：PluginInstalled ≠ StorageInstanceReady。  
  
六、V1 唯一推荐主流程  
  
Launch  
→ Home  
→ 无照片可见：Home 空状态请求访问  
→ 有照片：显示照片流  
→ 用户长按照片  
→ Selection Mode  
→ 继续多选  
→ Protection Picker  
→ 选择保护意图  
→ Core 检查该意图对应 Policy  
→ 若已有 Ready StorageInstance：进入执行  
→ 若没有：展示“需要配置存储”的按需引导  
→ Plugin Store  
→ Search/Browse  
→ Plugin Detail  
→ Install  
→ Activate  
→ Configure  
→ Capability Test  
→ PASS 后 StorageInstance = Ready  
→ 返回未完成保护意图  
→ Policy Resolve  
→ Transfer / Encrypt if required / Verify  
→ Evaluate Policy  
→ 满足：Protected  
→ 不满足：Pending / Needs Attention（精确枚举由 T03/T04）  
  
七、异常与边界要求（传递给后续任务）  
  
\- 用户拒绝照片访问：Home 必须有可恢复入口。  
\- 用户只有部分照片可见：产品不能假装拥有完整图库。  
\- 用户选完照片后取消 Protection Picker：选择是否保留由 T04 冻结。  
\- 插件安装成功但配置失败：不得创建 Ready StorageInstance。  
\- Credential 无效：Test Fail；提供修改配置路径。  
\- 能认证但不能写：不能 Ready。  
\- Provider 暂时不可用：不得删除用户 Protection Assignment。  
\- Private 加密失败：绝不能上传明文作为降级方案。  
\- 上传成功但 Verification 未完成：不得显示 Protected。  
\- 只有部分目标成功：Policy 未满足时显示真实的中间/降级状态。  
\- 插件后续失效/撤回：已有本地状态不能丢失；如何处理由 T04/T06 定义。  
  
八、验证方法  
  
A. 交互覆盖验证  
拿最终 V1 主流程的 10 个关键交互逐项检查：每项都必须有成熟范式或明确产品理由。  
结果：已覆盖。  
  
B. “无账号闭环”验证  
模拟新用户：  
启动→授权→看到照片→选择→打标。  
在 Storage 需求出现之前不要求账号。  
结果：成立。  
  
C. “延迟配置闭环”验证  
模拟用户首次将照片标为需要保护、但没有 Storage：  
系统能从当前意图进入 Plugin Store，配置完成后回到原意图，而不是让用户从头再做一次。  
结果：该要求已冻结，T04 必须实现状态保留。  
  
D. “插件生命周期闭环”验证  
Browse→Detail→Install→Configure→Test→Ready。  
每一步有独立状态，任何失败有恢复路径。  
结果：模型成立，精确状态交给 T04/T06。  
  
E. “Protection 语义闭环”验证  
上传成功但未验证时：  
必须不是 Protected。  
Private 加密失败时：  
必须不上传明文。  
结果：原则已冻结。  
  
九、对后续任务的影响  
  
T02：  
必须区分
Plugin、StorageInstance、ProtectionAssignment、Transfer、Verification、ProtectionState。  
  
T03：  
必须定义 Protection Picker 的唯一选项、Policy Satisfied 条件，以及 Private 的本地加密硬约束。  
  
T04：  
必须支持“从未配置 Storage 的 Protection Intent 暂停→完成插件配置→自动恢复原 Intent”。  
  
T05：  
Home 是唯一主页面；Plugin Store 不必成为底部主 Tab，可按需进入。  
Selection Mode 和 Protection Picker 是 Home 上的交互状态，不是独立主页面。  
  
T06：  
PluginInstalled 与 StorageInstanceReady 必须分离。  
插件要声明 Publisher、Capabilities、Data Access 等信息。  
  
T08：  
必须区分 Transfer Success 与 Verification Success。  
  
T09：  
必须验证 Marketplace 与用户 Data Plane 隔离；插件权限必须最小化。  
  
十、闭环结论  
T01 已形成足够明确的 Interaction Reference，可直接约束后续 Domain、Protection、State Machine 和
UI 规格。  
不再需要开发 AI 对“主页面怎么进、多选怎么做、什么时候配置存储、插件市场怎么组织、什么时候算完成”自行做产品判断。  
  
T01：PASS。


```

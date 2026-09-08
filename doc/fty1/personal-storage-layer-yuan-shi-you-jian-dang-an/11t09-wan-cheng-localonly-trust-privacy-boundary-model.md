# 11｜【T09 完成｜Local-only Trust / Privacy / Boundary Model】

**邮件主题：** RE: 【项目立项】Personal Storage Layer — Mobile Photos V1｜T01–T10 主线程

**收到时间：** 2026-09-05T17:12:14Z

## 邮件正文原文

```
【T09 完成｜Local-only Trust / Privacy / Boundary Model】  
  
验收结论：PASS WITH CONSTRAINTS  
  
一、目标  
冻结 Core、Plugin、Marketplace、Provider 四个信任域，确保“纯本地”不是宣传语，而是可验证的数据边界；同时冻结 Private
的真实客户端加密产品语义。  
  
二、四个信任域  
A Core：最高信任。持有用户 Library 索引、Protection、Credential 引用、Key、明文读取能力、任务事实。  
B Storage Plugin：低于 Core。只在单次授权操作中得到必要 payload/credential capability，不得浏览完整
Library/Key。  
C Marketplace：不可信远端控制面。只提供 Catalog/包版本/撤回信息。  
D Provider：外部存储。普通策略可能看到明文 payload；Private 只能看到密文与最小技术 metadata。  
  
三、唯一隐私原则  
我们的后台无法回答：用户有哪些照片、哪些是 Private、用了哪个 StorageInstance、上传了什么、密钥是什么、任务是否成功。  
如果未来业务要采集这些信息，必须作为新的产品决策重新评审，不能由开发 AI 加 analytics 顺手上传。  
  
四、Data Residency Matrix  
仅 Core
本地：PhotoAsset/SourceCopy、ProtectionMark/Policy/Assignment、StorageInstance、Credential、LocalKeyMaterial、EncryptionArtifact
mapping、RemoteCopy inventory、TransferJob、VerificationRecord、Problem、日志诊断。  
Marketplace 可有：pluginId、publisher、版本、描述、图标、兼容范围、定价说明、签名/完整性信息、withdraw/block
状态。  
Plugin 临时可见：当前操作必要的目标配置、受控 Credential 能力、payload；Private 时 payload 已是密文。  
Provider：目标对象和协议必要 metadata；Private 不得收到 Key/ProtectionMark/原始语义 metadata。  
  
五、Private 加密产品格式要求  
不在产品层自行发明密码算法，但冻结必须满足的属性：  
\- authenticated encryption：能检测密文被篡改。  
\- 每个对象使用独立随机 nonce/等价安全参数，禁止固定 nonce。  
\- 文件内容加密与密钥材料分离。  
\- ciphertext 有 formatVersion，未来可迁移。  
\- 解密前必须验证完整性。  
\- 原始文件名、Private 标签、敏感 EXIF 等默认放在加密 envelope 内，不作为 Provider-side 明文 metadata。  
具体算法套件在技术实施阶段必须选择成熟、审计充分的现代 AEAD/KDF/随机数方案，不允许自创密码学。  
  
六、密钥模型唯一产品决策  
V1 采用“本地 Master Key → 派生/包裹对象密钥”的概念模型，而不是把 Provider Credential 当加密密码。  
Provider Credential 与 Encryption Key 完全独立。  
更换 GitHub token 不应导致历史 Private 数据无法解密。  
删除 StorageInstance 也不能自动删除加密 Key。  
  
七、恢复模型  
纯本地带来真实风险：如果唯一密钥随设备彻底丢失，Private 密文可能永久不可恢复。  
因此 V1 在声称 Private 可长期恢复前，必须提供“用户控制的离线 Recovery Material 导出/恢复”产品能力，且 Recovery
Material 不上传我们的服务器。  
推荐产品语义：用户可显式创建 Recovery Kit/Recovery Key，并被明确要求离线保存；导入它可恢复 Master Key 能力。  
在该能力未实现前，Private 功能只能标记 PASS WITH CONSTRAINTS，文案必须提示“丢失本地密钥可能导致无法恢复加密内容”。  
  
八、禁止的恢复方式  
\- 我们服务器托管用户 Master Key。  
\- Marketplace 账号找回 Key（产品无账号）。  
\- 用 GitHub token 直接派生长期数据密钥。  
\- 把 Recovery Key 自动上传 Provider 同目录且无需用户秘密保护。  
  
九、插件权限  
默认 deny。  
插件不能请求“整个照片库”。Core 只向 putObject 提供当前 payload。  
Private encryption 在插件调用前完成。  
Credential 通过受控 secret handle/调用能力提供，禁止普通 config JSON 明文传播。  
插件无权读取 LocalKeyMaterial。  
  
十、插件供应链  
Marketplace 下发的插件包必须可验证 publisher/完整性/版本身份；Core 安装前验证，运行时权限受 Manifest
限制。具体签名机制后置技术实现，但产品验收必须能回答“这个包是否是 Catalog 声称的 publisher/version，是否被篡改”。  
blocked 只能阻止危险插件继续新操作，不能远程擦除用户本地数据。  
  
十一、Marketplace 泄露测试  
假设 Marketplace DB、API、日志全部泄露：攻击者最多得到公开插件目录/版本/发布信息；不能得到用户 Photo、Mark、Storage
Credential、Key、RemoteCopy 清单。  
通过。  
  
十二、Provider 全可见测试  
假设 GitHub/未来 Provider 可读取收到的全部对象和 metadata：Private 对象内容为认证密文；remote key
opaque；不暴露 Private 标签/原始文件名等默认敏感语义。攻击者仍可能观察对象大小、上传时间、对象数量等流量侧信道；V1 不声称隐藏这些
side-channel。  
通过，有明确限制。  
  
十三、Plugin 恶意测试  
恶意插件理论上最危险，因为代码在本地运行。产品边界要求：权限沙箱/能力调用必须使其只能访问 Core
明确授予的当前操作数据；若未来技术平台无法实现此隔离，则第三方插件开放必须降级为“受信/审核插件”模式，不能虚假声称完全沙箱。  
这是实施阶段必须验证的约束。  
  
十四、日志/诊断  
默认本地。  
禁止记录：明文照片、secret、key、完整 auth token、Private 原始文件名、完整敏感 EXIF。  
Provider 原始错误先脱敏再进入诊断。  
如果未来用户主动导出诊断包，必须预览/说明包含内容；不自动上传我们的服务器。  
  
十五、Telemetry  
V1 默认无用户行为/照片/Storage telemetry 上报。  
Marketplace 获取 Catalog 时不可附带 Photo/Protection 数据。  
基础网络请求不可避免暴露 IP、客户端版本等服务端技术信息时，隐私政策需如实说明；不得把“服务器完全不知道用户存在”作为绝对承诺。  
  
十六、删除边界  
删除本地 App/Core 数据、删除 Provider RemoteCopy、删除 Credential、删除 Key 是四个独立动作。  
任何一个动作不得隐式代表另外三个。  
尤其删除 Key 前必须做高风险确认：仍存在依赖该 Key 的 EncryptionArtifact/RemoteCopy 时，必须阻止或明确不可恢复后果。  
  
十七、Recovery 验证场景  
1 新设备/本地数据库丢失但用户有 Recovery Kit + Provider access：产品长期目标应可恢复解密能力；Remote
inventory 重建机制可后续阶段实现。  
2 无 Recovery Kit 且 Master Key 丢失：不能承诺恢复；UI 必须诚实。  
3 Provider token 泄露：攻击者可访问 Provider 权限范围内对象，但 Private 明文仍受独立 Key 保护。  
4 Marketplace 泄露：不影响 Private key。  
  
十八、Private 的“厂商拒绝敏感内容”边界  
客户端加密可以降低 Provider 直接识别内容明文的能力，但不能承诺绕过 Provider
服务条款、合法合规要求、账户政策、文件模式/流量检测或其他审核。产品不得宣传“加密后任何内容都能上传/不会被封”。  
  
十九、成熟安全原则依据  
采用成熟行业通用原则：客户端端到端/零知识方向的密钥与 Provider
分离、最小权限、默认拒绝、认证加密、密钥恢复与数据面分离。具体密码学实现必须在开发阶段使用成熟库和正式安全评审，不由低成本 AI 自创算法。  
  
二十、闭环条件  
Marketplace 泄露不泄露用户数据；Provider 全可见仍不能得到 Private 明文；Plugin 无权读取全库/Key；Private
加密失败绝不上传明文；密钥丢失风险有明确 Recovery 产品路径和诚实文案。  
  
二十一、为什么 PASS WITH CONSTRAINTS  
产品信任模型已冻结，但真正第三方 Plugin Sandbox 强度、具体 AEAD/KDF/Key storage 与 Recovery Kit
格式属于技术实现和安全评审，当前用户要求不绑定平台，因此不能在本阶段伪造已验证结论。开发阶段必须把这些列为 Security Gate。  
  
二十二、对 T10  
最终开发包必须把以下列为不可降级验收：client-side authenticated encryption、key/provider
credential separation、no plaintext fallback、no user data to Marketplace、opaque
metadata、Recovery warning/kit、plugin least privilege、security gate。  
  
T09：PASS WITH CONSTRAINTS。


```

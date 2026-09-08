# 02｜【立项约束修订｜高于原始立项邮件】

**邮件主题：** RE: 【项目立项】Personal Storage Layer — Mobile Photos V1｜T01–T10 主线程

**收到时间：** 2026-09-05T15:58:32Z

## 邮件正文原文

```
【立项约束修订｜高于原始立项邮件】  
  
以下四条正式冻结，并覆盖原立项邮件中与之冲突的内容：  
  
1）当前阶段只定义“产品”，不定义具体运行平台  
\- 不讨论 iOS / Android / Desktop 的平台实现差异。  
\- 不把任何平台限制带入当前交互、数据流、任务拆解。  
\- 产品规格必须保持平台中立。  
\- 原 T09「iOS 最新能力边界验证」取消，替换为「Local-only Trust / Privacy / Boundary Model」。  
\- 具体平台可行性验证属于后续“实施准备阶段”，不是当前产品定义阶段。  
  
2）V1 唯一真实 Reference Plugin：GitHub  
\- 一期只要求 GitHub 插件跑通完整闭环。  
\- GitHub 仅作为真实 Provider 用于验证插件机制，不将其包装为理想照片长期存储服务。  
\- 其他 Provider 只在抽象模型评审中用于反例/兼容性检查，不要求一期实现。  
\- 插件接口不得写死 GitHub 特性，否则 T06 不通过。  
  
3）Private 必须是真正的客户端加密  
\- Private 不是 UI 标签，不依赖 Provider 服务端加密。  
\- 明文在进入 Provider 之前完成本地加密。  
\- Provider 得到的只能是密文及协议允许暴露的最少必要信息。  
\- 具体加密格式、密钥生命周期、恢复方式在后续任务中定义；未经定义不得声称“私密已闭环”。  
\- 产品理由之一：敏感内容可能被第三方存储服务拒绝/审核，因此敏感内容不能以明文依赖 Provider 的内容政策。  
  
4）Core 永久纯本地  
以下信息原则上不得上传到我们的服务端：  
\- 用户原始文件  
\- 文件内容摘要/缩略图  
\- 用户照片分类与保护标记  
\- Policy  
\- Storage Credential  
\- 加密密钥  
\- Remote Copy 清单  
\- Transfer / Verification 历史  
\- 本地索引  
\- 用户数据全貌  
  
唯一预留的服务端能力：  
Plugin Marketplace 下发，包括插件目录、插件描述、版本、兼容性、发布/撤回信息等。  
Marketplace Plane 与用户 Data/Control Plane 必须从产品边界上分离。  
  
因此产品原则更新为：  
“我们的服务端知道有哪些插件，但不知道用户有什么数据、数据放在哪里、怎么分类、怎么保护。”  
  
【任务树修订】  
  
T01 竞品与成熟交互机制研究：保持，但全部按平台中立产品范式研究。  
  
T02 Domain Model：保持。  
  
T03 Protection Model：保持，并必须将“Private = 客户端加密后才能进入 Provider”写入 Policy 判定。  
  
T04 UX / System State Machine：保持，但去掉平台特定状态。  
  
T05 页面与交互规格：保持平台中立；后续实施阶段再做平台适配。  
  
T06 Storage Plugin Spec：一期以 GitHub 为唯一真实 Reference Plugin；必须证明接口没有 GitHub 锁定。  
  
T07 持久化数据模型：保持，但以 Local-only 为不可违反的边界。  
  
T08 数据流：保持，并新增明文→本地加密→密文→Provider→验证的数据路径。  
  
T09 替换为：  
【Local-only Trust / Privacy / Boundary Model】  
目标：定义 Core、Plugin、Marketplace、Provider 四个信任域；明确每类数据允许出现在哪里；定义 Private
数据的明文边界、Metadata 暴露边界、Credential/Key 边界、日志/诊断边界。  
验证：  
\- 对每种敏感数据做 Data Residency Matrix。  
\- 假设我们的 Marketplace 服务端完全泄露，用户照片/密钥/Provider Credential 仍不可从中获得。  
\- 假设 Provider 能查看其收到的全部内容，Private 明文仍不可获得。  
\- 任意一项不能成立则 T09 不通过。  
闭环：产品页面、Domain、Plugin Spec、Data Flow 中所有涉及远端的行为必须符合该边界。  
  
T10 最终 Development Package：当前阶段先产出“平台中立开发规格”；平台技术实现包在后续单独阶段生成。  
  
后续所有 T01–T10 回复均以本修订为准。


```

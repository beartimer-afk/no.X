# 15｜Personal Storage Layer / Mobile Photos V1 的 HarmonyOS UI 已进一步细化到“逐页面视觉实施规格”。

**邮件主题：** 【HarmonyOS视觉冻结】Personal Storage Layer V1｜逐页面字体·颜色·间距·组件规格

**收到时间：** 2026-09-06T04:55:55Z

## 邮件正文原文

```
Personal Storage Layer / Mobile Photos V1 的 HarmonyOS UI 已进一步细化到“逐页面视觉实施规格”。  
  
本轮冻结了全局 Design Tokens，并逐页面定义 Home、权限/空状态、Selection、Protection Picker、Storage
Required、Storage Center、Plugin Store、Plugin Detail、Plugin Setup、Capability
Test、Problems、Protection Detail、Storage Detail、Dialog 与全局反馈。  
  
重点包括：Light Mode 色值；HarmonyOS Sans 字号/字重/行高；4/8/12/16/24/32/40/48vp
间距；圆角；Icon；48vp 热区；按钮高度；照片 Grid；Pressed/Selected/Disabled/Error；Safe
Area；sm/md/lg 响应式规则。  
  
HarmonyOS 官方最新布局基线：<600vp 为 4-column 页面栅格、16vp margin/8vp gutter；600–839vp 为
8-column、24vp/12vp；>=840vp 为 12-column、32vp/16vp。照片墙自身冻结为手机默认 3 列、2vp
gap，这是本项目业务视觉决策，不是把官方页面栅格误解成照片必须 4 列。  
  
新增 VAT-01～VAT-20。开发必须同时通过原 AT-01～AT-35、HAT-01～HAT-16 和 VAT-01～VAT-20。  
  
开发 AI 必须先实现 Token 层，再实现页面；禁止散落 magic number，禁止自行重选颜色、字号、间距、圆角或导航。  
  
完整逐页面规格见附件。


```

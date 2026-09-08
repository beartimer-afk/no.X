# 06｜HarmonyOS 复刻规格

实现顺序冻结为：**H00 Visual Acceptance Harness → H01 Foundation Tokens → H02 TodoCheckbox → H03 TodoRow → H04 TodoMetadata → H05 ExpandedTodo → H06 Checklist → H07 Organization → H08 Overlay/Pickers → H09 Global Interaction → H10 Page Composition → Final Regression**。

平台遵循 HarmonyOS 的 vp/fp、安全区、触达区域、键盘、手势与可访问性基础能力；视觉目标仍以 Things 当前 iPhone Light Mode 真机证据为准。

HarmonyOS 当前官方文档仍支持 Root 级 Dialog/Popup/Menu/Sheet/OverlayManager 等弹出体系；需要完全自定义内容/行为时可采用 OverlayManager 类能力。拖拽可通过 LongPress/Pan/GestureGroup 组合。但系统默认视觉与默认时间参数都不能自动成为 Things Token。

每个 H 任务只有在 Fixture、Geometry、Screenshot、Interaction 和 Regression 真实运行通过后，才可从 `IMPLEMENTED` 升为 `VERIFIED`。

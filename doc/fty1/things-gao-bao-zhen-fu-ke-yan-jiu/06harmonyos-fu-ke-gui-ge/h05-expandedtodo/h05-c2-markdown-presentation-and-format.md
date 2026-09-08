# H05-C2｜Markdown Presentation & Format

> HarmonyOS 26 / ArkUI｜正式开发规格｜可独立验收

## 核心原则

H05-C2 建立 Notes 的 Markdown Presentation Layer：raw Markdown 字符串始终是唯一事实源；语法字符始终可见；RichEditor 只为同一字符范围施加样式；Format 操作必须修改 rawText 中的 Markdown syntax，而不是只改 span 属性。

## 官方事实

Things 当前 Notes 支持 Heading、Italic、Bold、Highlight、Bulleted/Numbered/Task List、Code/Code Block、Link、Horizontal Rule、Quote、Strikethrough 等 Markdown。官方明确 syntax 保持显示，iPhone/iPad 可在 selection popover 中进入 Format。

## 与 Domain Checklist 分离

Notes 中的 `- [ ] 买牛奶` 只是 Markdown 文本，不是 ChecklistItem。Parser 看到 task list 不能创建 Domain Checklist；ChecklistItem 也不能自动写入 Notes rawText。

## 架构

```
NotesEditor Core
├─ rawText
├─ selection/caret
└─ RichEditor

MarkdownPresentationController
├─ MarkdownPresentationParser
├─ MarkdownFormatCommandEngine
└─ SpanReconciler
```

`MarkdownPresentationParser`：rawText → `NotesStyleSpan[]`。

`MarkdownFormatCommandEngine`：rawText + selection + command → newRawText + newSelection。

`SpanReconciler`：style spans → RichEditor styles。

## Style Span

```ts
export interface NotesStyleSpan {
  start: number
  end: number
  styleType: NotesStyleType
  level?: number
  metadata?: Record<string, string>
}
```

样式类型至少包含 heading、bold、italic、highlight、strikethrough、inline/code block、quote、bullet/number/task markers、link、horizontal rule、markdown syntax。必须支持重叠样式。

## 索引不变合同

Parser 不得删除任何 syntax。`rawText.length == RichEditor text length`。

例如 `**abc**` 的 7 个字符全部保留，只有 `abc` 得到 Bold 样式。Caret / Selection / Copy / Search 因此始终使用 rawText index，无隐藏字符映射。

## Token

Markdown 视觉全部 Token 化：syntax、heading、code、highlight、link、paragraph gap、list indent 等。无 A 级视觉测量时保持 PROVISIONAL，不套 Web Markdown 默认 CSS。

## Format Engine

Format 是 Raw Text Mutation，而不是 `editor.setBold(true)`。

```ts
interface MarkdownFormatResult {
  rawText: string
  selectionStart: number
  selectionEnd: number
  caretPosition: number
}
```

命令至少覆盖 H1/H2、Bold、Italic、Highlight、Strike、Bullet、Numbered、Task List、Quote、Inline Code、Code Block、Link、Horizontal Rule、Indent / Outdent。

同一格式再次执行需 deterministic toggle，不能无限叠加 syntax。

## Parser / Reconciliation

推荐 line tokenizer → inline tokenizer → block/inline token → StyleSpan。长文优先增量 reparse；禁止每次按键 `clear editor → add all spans → requestFocus`，避免 caret、selection、IME 和性能问题。

## IME

composition range 优先保证稳定输入。可以延迟 style reconciliation，composition commit 后再完整 reconcile。视觉样式短暂延后优先于打断输入法。

## Selection Format Menu

官方语义确认 iPhone/iPad 可选中文本后进入 Format。HarmonyOS 使用 RichEditor selection menu 扩展功能，加入“格式”入口，动作调用 MarkdownFormatCommandEngine。

具体 iPhone Format menu 的位置、行高、图标、动效目前缺 A 级录屏，因此菜单视觉标记 `PENDING-MD-04`；功能可以先 VERIFIED。

## Undo / Redo

一个 FormatCommand 应作为一次逻辑编辑事务。Bold 包裹等一次操作理想上一次 Undo 就能撤销。精确 Things Undo 粒度保持 Pending。

## Links / Lists / Task Lists

链接必须保持完整原字符；编辑态点击 vs 打开 URL 的精确手势待补证。

列表 raw marker 保留。Return 是否自动续下一项保持 Pending。

Notes task list 如果未来支持 marker tap，也必须修改 rawText marker，不能调用 Todo Checklist Domain Command。

## Fixtures

M01 plain；M02 bold；M03 italic；M04 highlight；M05 strike；M06 heading；M07 bullet；M08 numbered；M09 tasklist；M10 inline\_code；M11 code\_block；M12 quote；M13 link\_url；M14 nested\_styles；M15 format\_toggle；M16 ime\_inside\_markdown；M17 long\_markdown\_200\_lines。

## Golden

覆盖 bold、heading、highlight、code、lists、tasklist、link、nested、selection-format-entry。必须确认 syntax 可见、raw/display 字符数一致、caret/selection 不漂移、左 anchor 不变。

## 自动测试

Parser Test、Format Command pure-function Test、Index Test、IME Test、Performance Test、C1 Regression。

## Pending

* PENDING-MD-01 Return 自动续列表
* PENDING-MD-02 Task marker 是否可直接点击
* PENDING-MD-03 URL 编辑/打开边界
* PENDING-MD-04 Format menu 视觉与动效
* PENDING-MD-05 Undo 粒度
* PENDING-MD-06 富文本粘贴行为
* PENDING-MD-07 Markdown 精确 Token
* PENDING-MD-08 Heading 完整视觉层级

## 低能力模型顺序

Bold parser → Italic/Highlight/Strike → Heading → Code/Quote → Lists/Task → Link → FormatCommandEngine → Selection preservation → incremental reconciliation / IME → Format menu 功能入口 → 200 行性能 → C1/C2 Golden。

## 禁止事项

禁止隐藏 syntax；禁止 WYSIWYG 代替 raw Markdown；禁止 Format 只改 span；禁止 Notes task 创建 Domain Checklist；禁止外部 Divider 代替 HR 导致 index 消失；禁止每键重建 RichEditor；禁止 composition 期间强制 rebuild；禁止套网页 CSS；禁止无 A 级证据自创最终 Format 菜单。

## VERIFIED

M01–M17 parser/command/index/IME/performance PASS；raw/display index 一一对应；syntax 始终可见；核心 Markdown 样式正确；Format Command 可测试且保持 selection/caret；H05-C1 regression PASS；Golden 达到 H00 阈值。

Format Menu Visual 在补齐 A 级证据前只能 `FUNCTIONAL_VERIFIED / VISUAL_PENDING`。

下一子任务：**H05-D｜ExpandedActionBar**。

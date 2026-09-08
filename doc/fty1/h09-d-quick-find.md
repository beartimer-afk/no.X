# H09-D｜Quick Find

Quick Find 是全局寻址层，不等同某个 SearchPage。

## Two-tier index

`NavigationIndex`：Main Lists/system lists、Area、Project、Heading/可导航结构；优先低延迟寻址。

`ContentIndex`：Todo title、notes、Checklist、Logbook 等深内容。两层可共享 tokenizer/ranker，但不得把导航入口与全文结果混成无类型字符串。

## Result

`QuickFindResult { type, id?, title, subtitle?, destination, rankReason }`。选择结果只发 Navigation/Open intent；不直接修改 Domain。

## UI

Overlay + search field + grouped results；keyboard-first、focus restore、empty state、recent/initial suggestions（若无证据则不添加产品功能）。隐藏特殊列表 Tomorrow、Deadlines、Repeating、All Projects、Logged Projects 可以通过 navigation index 暴露。

## Fixtures

exact/prefix/content match、Area/Project同名、system list、hidden list、Todo note hit、empty、large index、keyboard open。

## Acceptance

结果类型正确、ranking deterministic、导航不丢原页面状态、deep result 进入正确实体 presentation、性能基线和 accessibility PASS。

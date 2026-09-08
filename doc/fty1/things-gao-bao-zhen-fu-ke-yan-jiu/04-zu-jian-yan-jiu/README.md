# 04｜组件研究

组件体系按依赖层级组织，而不是按页面复制实现。

```
Foundation
→ Primitive
→ Todo / Organization Components
→ Overlay / Interaction Components
→ Page Composition
```

核心原则：一个组件在 Visual Lab 和 Golden 中 VERIFIED 后，上层页面只能组合和传 Props，不能为了某一页临时改它的颜色、间距或尺寸。

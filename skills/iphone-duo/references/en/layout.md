# Layout

[한국어](../layout.md) | English | [日本語](../ja/layout.md)


## Inspect existing code

Search the project sources for the following patterns and read their calling context. A match is not itself a defect.

```sh
rg -n --glob '*.swift' 'UIScreen\.main|userInterfaceIdiom|UIDevice\.current\.orientation|interfaceOrientation|ignoresSafeArea|safeAreaInsets|GeometryReader|frame\(width:' .
```

- Container size differs from full display size. Calculate layouts from the current view's size, including in multiple windows.
- Support compact/regular transitions without hardcoding the inner display as always regular.
- Minimum readable card widths and maximum body text widths can remain. Do not remove all fixed values and breakpoints indiscriminately.
- Prefer safe area layout guides and Auto Layout in UIKit. When calculating frames directly, account for all four edges, for example with `view.bounds.inset(by: view.safeAreaInsets)`, and recalculate during layout updates.
- Apply SwiftUI's `ignoresSafeArea` to appropriate backgrounds. Do not automatically extend buttons and body content too. Check that input fields and submission actions remain visible with the keyboard present.

## Adapt to folding

The upstream reserved-region guidance distinguishes division regions from occlusion regions. Perform [SDK verification](compatibility.md) before using these APIs, and check coordinate spaces, active state, and change notifications. Do not assume safe areas represent every fold region.

Consider split when primary and secondary content must both remain visible, and overlay when content has a foreground/background relationship. Check the official session's nesting constraints before adopting `ArrangementView`. The upstream guide places navigation containers outside arrangements and advises against putting arrangements inside `List` or `ScrollView`.

Do not move a scrolling article or feed between regions on every fold. Consider even grid column counts only when symmetrical division is useful, and do not force them at narrow widths or large text sizes. Do not calculate layouts solely from hinge angles.

Build fallback layouts with existing `NavigationStack`/`NavigationSplitView` and stacks or grids available on the project's supported OS versions. Preserve stable IDs and state owners when view structure changes.

Sources: [Adaptive layouts session](https://developer.apple.com/videos/play/tech-talks/111463/) and [Design session](https://developer.apple.com/videos/play/tech-talks/111466/). See [Sources](sources.md) for upstream interpretation and additional practical guidance.

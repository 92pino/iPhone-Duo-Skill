# Navigation and bars

[한국어](../bars.md) | English | [日本語](../ja/bars.md)


Prefer navigation container toolbars in SwiftUI and navigation/tab controllers in UIKit. Do not imitate system behavior by manually rotating bars or adding fixed side margins.

## Evaluate each item

- Provide meaningful titles and icons for buttons. Let the system choose the presentation, as with SwiftUI's `Label`.
- Keep text when it conveys information itself, such as a price or progress. Simple counts can use badges, but their meaning should also be conveyed by accessible names.
- Review the order of back/close, primary, and secondary actions. Preserve the same actions across size changes and make them reachable through overflow when needed.
- Check that key actions remain available with the keyboard, long localized strings, and large text. Keep keyboard accessories attached to the keyboard.
- The inner display in portrait is a documented HIG exception: it has enough vertical space to keep standard horizontal bars. Do not force vertical bars in every pose.
- When Split View multitasking divides the inner display, each app places its controls along its own outer edge (left app on the left, right app on the right). Check safe areas so the two apps' controls don't overlap.

Apply the upstream APIs for axis control, visibility priority, overflow integration, and disabling vertical bars only after [SDK verification](compatibility.md). If replacing a custom bar with a standard one would break the product design, first improve resizing and accessibility. Decide whether to disable vertical bars, such as in a single-button sheet, after comparing actual available space.

Validate horizontal/vertical transitions, limited height, RTL, VoiceOver names and order, and Reduce Transparency. Do not manually mirror the physical position of system bars for RTL.

Source: [Raise the bar with iPhone Duo](https://developer.apple.com/videos/play/tech-talks/111462/).

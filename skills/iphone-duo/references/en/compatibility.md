# SDK and API verification

[한국어](../compatibility.md) | English | [日本語](../ja/compatibility.md)


When checked on September 15, 2026, [Apple's Duo page](https://developer.apple.com/iphone-duo/) listed Xcode 27.1 beta as coming later that month. This is a historical observation; check the official announcement and installed SDK again when performing the task.

## Verify three separate conditions

| Condition | Evidence | Action |
|---|---|---|
| Compile-time availability | Headers/Swift interfaces in the selected SDK and an actual build | Do not add a symbol to build sources if its declaration is absent |
| Runtime OS support | Declaration availability and deployment target | Use the verified version with `#available` or `@available` and a fallback |
| Current device or scene capability | Runtime capabilities and connection state | Handle changes and unsupported configurations |

`if #available(iOS 27.1, *)` alone cannot prevent compilation errors for types absent from an older SDK. `#if canImport(SwiftUI)` does not check whether a new type exists within SwiftUI. Do not substitute the Swift compiler version for the iOS SDK version.

Gather the necessary evidence from official documentation, official session code, local SDK declarations, and a minimal build, in that order. A failed search does not prove an API is absent. A name appearing in a session does not prove the installed SDK provides it. Do not guess names, modules, arguments, or availability.

## Unverified symbols

`ArrangementView`, `UIArrangementViewController`, `reservedRegions`, `onHingeChange`, `UIHingeInteraction`, `axisBehavior`, `toolbarVerticalBehavior`, `CameraCaptureAccessory`, and `AVCaptureDeviceDirectionCoordinator` are **verification candidates** discussed in the upstream material and announcements. This is not a list of declarations known to compile.

If the SDK is insufficient, first complete improvements to resizing, safe areas, and state preservation in existing containers. Leave unverified features in design notes instead of inserting unresolved symbols, fake stubs, or private APIs. Report the required SDK version and checks to repeat when an upgrade is needed.

Use the project's established build commands. For Tuist projects, edit source manifests rather than generated projects. Discover and use real schemes and destinations, and do not arbitrarily change signing or account settings.

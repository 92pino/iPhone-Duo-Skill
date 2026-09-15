# Compatibility checklist

[한국어](checklist.md) | English | [日本語](checklist.ja.md)

This is a practical inspection scope, not a claim that every item changed in iOS 27. Select checks according to the app's features and actual release notes.

| Area | Inspection and fix criteria |
|---|---|
| Builds and dependencies | SDK declarations, compiler diagnostics, app/extension targets, package compatibility, lockfiles, linking errors |
| SwiftUI and UIKit | Valid tab selection, navigation restoration, sheet/popover anchors, safe areas and keyboard, state preservation |
| Concurrency and lifecycle | Actor isolation, UI update context, cancellation/duplicate tasks, foreground resumption, observer cleanup |
| Persistence and networking | Existing data reads/writes, failure recovery, offline behavior, expired authentication, response errors |
| Permissions and media | Denied/restricted access, camera/audio interruptions, resumption, saved output on physical devices |
| Existing system integrations | Deep links, notifications, background work, widgets/extensions, purchases and restoration in test environments |
| Accessibility and older OS versions | Large text, VoiceOver, RTL/long translations, minimum-OS fallbacks |

For SwiftUI apps, search release notes for `TabView` and `selection` to verify SDK-linked selection constraints. Inspect paths that select hidden or unavailable tabs, reproduce on the actual target version, then fix. Do not introduce an unused API because of this example.

## Validation combinations

| Build and runtime | Purpose |
|---|---|
| Existing SDK + existing OS | Baseline for existing failures and working behavior |
| Existing SDK + iOS 27, when an existing artifact is available | OS-update-only impact |
| iOS 27 SDK + iOS 27 | Rebuild failures and SDK-linked behavior |
| iOS 27 SDK + previously supported OS | Older-OS regressions and availability fallbacks |

Run only available combinations and mark the others not run. A successful build does not validate accessibility, cameras, or background behavior. Remove personal information from reported logs.

## Official sources

- [iOS & iPadOS 27 release notes](https://developer.apple.com/documentation/ios-ipados-release-notes/ios-ipados-27-release-notes)
- [Xcode 27 release notes](https://developer.apple.com/documentation/xcode-release-notes/xcode-27-release-notes)
- [iOS & iPadOS release notes index](https://developer.apple.com/documentation/ios-ipados-release-notes)

Specific changes verified in official Markdown on September 15, 2026 are in the [change map](change-map.en.md). Check the body version and item status instead of beta labels in search snippets, and compare against the actual SDK/OS build when working.

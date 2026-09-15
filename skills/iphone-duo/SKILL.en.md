# iPhone Duo

[한국어](SKILL.md) | English | [日本語](SKILL.ja.md)


English translation of the single `iphone-duo` skill. Use it to inspect and implement adaptive layouts, vertical bars, hinge interactions, multiple scenes, and camera switching in SwiftUI and UIKit apps for Duo. Apply it to Duo adaptation requests or UI, state, and camera problems caused by folding and unfolding. A mention of rotation in an ordinary iOS task alone does not warrant this skill.

Read one language version of this guide, then only the relevant references in that language. Keep all three versions synchronized; this translation is not a separately registered skill.

Adapt to changes in size, pose, and display while respecting the app's architecture and supported OS versions. Respond in the user's language and preserve API identifiers.

## Start the task

1. Read project instructions and inspect existing changes. Identify SwiftUI/UIKit usage, deployment targets, build workflow, affected screens, and state owners. For review requests, report findings. For implementation requests, complete changes and validation.
2. Check the actual toolchain with `xcodebuild -version` and `xcodebuild -showsdks`. When simulator validation is needed, discover destinations with `xcrun simctl list devices available`. Do not assume a Duo runtime or destination exists.
3. Read [SDK verification](references/en/compatibility.md) before introducing new APIs. Distinguish APIs shown in announcements from those that compile with the installed SDK.
4. Read only the references relevant to the requested feature. Do not add cameras or multiple windows to apps that do not need them.

| Task | Reference |
|---|---|
| Width, safe areas, fold regions, split/overlay | [Layout](references/en/layout.md) |
| Navigation, vertical bars, overflow | [Bars](references/en/bars.md) |
| Hinge effects, state preservation, windows and displays | [Scenes](references/en/scenes.md) |
| Camera selection, switching, and preview | [Camera](references/en/camera.md) |
| Migration and completion criteria | [Validation checklist](references/en/checklist.md) |
| Upstream revision, official links, verification scope | [Sources](references/en/sources.md) |

## Implementation principles

- Determine available space from the current container's size and size class, rather than a device model or `.phone`/`.pad`. Use actual available width when size class alone cannot express content requirements.
- Replace UI sizing based on `UIScreen.main.bounds` with the relevant view or scene. If a screen object is necessary, obtain it from the connected window scene.
- Treat all four safe area and margin edges independently. Separate background expansion from placement of controls.
- Prefer standard navigation containers and system bars. Understand the purpose of existing custom designs and adjust only what is needed.
- Preserve navigation paths, selection, drafts, and playback position across size changes. Separate shared domain data from window-specific UI state.
- Do not arbitrarily raise deployment targets. Fall back to existing layouts when APIs are unavailable, and integrate new functionality after verifying it in the SDK.

## Completion report

Summarize affected screens and files, user-visible behavior changes, build and test commands and results, and device-specific behavior that remains unverified. Do not report static inspection or ordinary iPhone simulator tests as successful Duo validation. For implementation requests, finish feasible changes and explicitly identify only the parts deferred because of environmental limitations.

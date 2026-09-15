# Migration and validation

[한국어](../checklist.md) | English | [日本語](../ja/checklist.md)


Run applicable checks and record each as pass / fail / not run / not applicable. A missing SDK should not stop feasible general adaptive UI improvements.

## Before and after implementation

- [ ] Confirm project instructions, existing changes, schemes, deployment targets, and the actual SDK.
- [ ] Compare baseline and post-change builds to distinguish existing errors from regressions.
- [ ] Inspect assumptions about full-screen size and device type, and calculations involving asymmetric insets.
- [ ] Verify new API modules, signatures, availability, and older-OS fallbacks.
- [ ] Preserve navigation paths, selection, drafts, scroll position, and playback position after resizing.

## Observable behavior

| Scenario | Check |
|---|---|
| Narrow/wide widths and limited height | Clipping, overlap, excessive empty space, access to primary actions |
| Duo opening, closing, partial folding, and rotation | Obscured controls, state preservation, repeated transition stability |
| Multitasking on either side and resizing | Asymmetric safe areas, coordinates relative to the current window |
| Keyboard, sheets, and popovers | Access to input and confirm/cancel actions, valid anchors |
| Large Dynamic Type, long translations, and RTL | Reading and navigation order, truncated text |
| VoiceOver and Reduce Motion/Transparency | Focus preservation, control names, readability |
| Ordinary iPhone, supported iPad, oldest supported OS | Regressions in existing screens and fallbacks |
| Multiple windows and external displays, when applicable | Independent state, disconnection, failed requests |
| Camera, when applicable | Denial, interruption, resumption, folding, recording, saved output orientation |

Run representative user flows on available simulators or physical devices. Without Duo, mark ordinary-device resizing as partial validation. Screenshots do not replace state preservation or accessibility checks.

Add behavioral tests for state transitions or fallbacks that carry regression risk. Avoid tests that merely compare wording or implementation structure. Run relevant existing tests and builds.

## Report format

- Changes: files, behavior, and reasons
- Verified: SDK / scheme / destination / commands / results
- Not run: missing prerequisites such as devices or runtimes, and reproduction steps
- Follow-up: specific actions needed for unverified APIs or failed checks

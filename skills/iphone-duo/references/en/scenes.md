# Scenes, hinges, and state preservation

[한국어](../scenes.md) | English | [日本語](../ja/scenes.md)


Use hinge data for interactions and effects, and available size and system layout information for layout decisions. Preserve default behavior on devices without hinges, and avoid performing expensive work for every continuous event.

## State boundaries

- Separate documents, accounts, and persisted data from each window's navigation path, selection, and focus.
- Avoid losing drafts by recreating models or changing `.id` on every size transition.
- Avoid duplicate subscriptions during background/foreground transitions and scene reconnection. Clean up relevant observers and tasks when a screen disappears.
- Use the current scene when multiple windows exist. Do not assume `connectedScenes.first` is the user's current window.

Check both app configuration and dynamic availability for multiple-window support. Handle failed window requests and preserve ongoing work. Do not introduce an unnecessary window model solely for Duo adaptation.

## Scene accessories

Introduce accessories only when supplementary content is actually needed. Perform [SDK verification](compatibility.md) for `CameraCaptureAccessory` and related APIs. Review registration location, availability changes, and resource cleanup on disappearance. Work on the primary screen must remain possible if an external display disappears. Connect shared data without indiscriminately making window-specific selection global.

For screens sharing a camera, also read [Camera](camera.md). Define capture resource ownership so both screens do not independently start the same resource.

Source: [Leverage multiple displays and scenes on iPhone Duo](https://developer.apple.com/videos/play/tech-talks/111464/).

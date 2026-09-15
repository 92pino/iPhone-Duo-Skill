# Camera

[한국어](../camera.md) | English | [日本語](../ja/camera.md)


## Selection and switching

First identify the app's resolution, frame rate, depth, and simultaneous capture requirements. If the virtual front camera described upstream meets them, do not add manual switching. If physical cameras are needed, enumerate supported devices and formats at runtime. Do not guarantee resolution or depth support based on a product name alone.

Use `AVCaptureDeviceDirectionCoordinator` and inner/outer device types after [SDK verification](compatibility.md). Direction is relative to a view, so do not reuse one global direction value for two displays.

## Execution and error handling

- Check authorization and usage descriptions. Provide fallback UI for denied or restricted access and absent cameras.
- Centralize session start, stop, and reconfiguration on the existing serial executor or camera actor. Handle UI and preview changes on the main actor. An actor does not automatically place blocking work on an appropriate executor; inspect the existing concurrency design.
- Check whether an input can be added before switching. Restore the previous input or provide an explicit failure state if reconfiguration fails. Serialize operations so start/stop cannot interrupt a configuration transaction.
- Prevent stale switching results from rapid fold events from overwriting the latest selection. Check output support and recording continuity requirements separately when switching during recording.
- Handle interruptions, backgrounding, resumption, and runtime errors. Ensure the camera does not keep running after its screen is released.

## Preview and captured output

Observe rotation coordinator changes and apply preview and capture angles separately. Verify that the rotation angles are supported. Validate mirroring and saved output orientation separately. Choose aspect fill cropping or aspect fit margins according to product intent.

Do not universally apply the upstream guidance to disable sensor orientation compensation or configure dynamic aspect ratios. Check SDK declarations and output requirements, then validate actual saved images and videos before applying it.

Simulator results are not physical-device evidence for camera switching, sensors, or recording quality. Mark these checks as not run if the device is unavailable.

Source: [Build a great camera experience for iPhone Duo](https://developer.apple.com/videos/play/tech-talks/111465/).

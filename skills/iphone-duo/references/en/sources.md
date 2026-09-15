# Sources and scope of improvements

[한국어](../sources.md) | English | [日本語](../ja/sources.md)


Checked on September 15, 2026. Recheck versions and APIs when performing a task.

## Upstream repository

[d-date/iphone-duo-skill](https://github.com/d-date/iphone-duo-skill), commit `6f5374935c76d1a4be149cbded04571a9ac694b7`.
Adapted into Korean, English, and Japanese using the upstream topic organization and migration approach. The upstream copyright and MIT notice are preserved in [LICENSE.upstream](../../LICENSE.upstream).

## Apple primary sources

- [Duo developer announcement and SDK release status](https://developer.apple.com/iphone-duo/)
- [Prepare your app for iPhone Duo](https://developer.apple.com/videos/play/tech-talks/111461/)
- [Raise the bar with iPhone Duo](https://developer.apple.com/videos/play/tech-talks/111462/)
- [Strike a pose with adaptive layouts on iPhone Duo](https://developer.apple.com/videos/play/tech-talks/111463/)
- [Leverage multiple displays and scenes on iPhone Duo](https://developer.apple.com/videos/play/tech-talks/111464/)
- [Build a great camera experience for iPhone Duo](https://developer.apple.com/videos/play/tech-talks/111465/)
- [Design for iPhone Duo](https://developer.apple.com/videos/play/tech-talks/111466/)
- [Designing for iPhone Duo — HIG](https://developer.apple.com/design/human-interface-guidelines/designing-for-iphone-duo)

The official announcement and the existence and topics of session pages were checked. Only the HIG link was checked; its body was not verified. This does not mean every API signature or Duo runtime behavior was verified. Compilation and execution of new Duo features were not validated with the installed Xcode 26.4.1 / iOS SDK 26.4.

## Additional guidance in this edition

Separating SDK declarations from runtime availability, preserving state, checking accessibility and localization, handling camera errors, maintaining older-OS fallbacks, and grading validation results are practical additions. Do not cite them as mandatory Apple requirements.

Blanket upstream rules were also made conditional. Content-based breakpoints are allowed; even grid column counts and disabling sensor compensation apply only when needed and validated for the relevant screen or output.

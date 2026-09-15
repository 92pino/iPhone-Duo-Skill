# Dependency compatibility

[한국어](dependencies.md) | English | [日本語](dependencies.ja.md)

## Inventory and reproduction

1. Locate actual `Package.swift`, `Package.resolved`, `Podfile`, `Podfile.lock`, `Cartfile.resolved`, Tuist manifests, and manually integrated frameworks/XCFrameworks. Do not introduce absent package managers.
2. Record each direct/transitive dependency's pinned version, consuming targets, source/binary/macro/plugin type, required toolchain/minimum OS, and official vendor release-note URL. Keep declared support separate from local validation results.
3. Capture the first causal diagnostic from a baseline build with pinned versions. Download/authentication failures are not compilation incompatibilities. Do not change versions or lockfiles because of network failures.

## Checks by type

| Type | Inspection and fixes |
|---|---|
| SPM source | Declared tools version, platforms, product/target integration, actual resolved version. Use the existing resolve workflow without blanket updates |
| CocoaPods | Lockfile versus installed state, the app's workspace, generated xcconfig integration. Use existing Bundler configuration and restrict Pod updates to implicated packages |
| Binary XCFramework | Check device/simulator platform variants together with architecture; arm64 does not imply the same platform. Check Swift module/interface toolchain compatibility, embedding/linking, transitive frameworks, and resource bundles |
| Macro/build plugin | Separate host macOS/toolchain/execution permission problems from app target errors. Inspect actual plugin diagnostics and vendor-supported versions without globally disabling validation |
| Duplicate modules | Follow item 136303612 in the [change map](change-map.en.md) to inspect SPM/Pods/manual integration and header search path duplication |

## Minimal changes and validation

Reproduce → identify the vendor's official fix → select the smallest compatible update → review manifest/lockfile differences → build consuming targets → exercise the affected feature. Report transitive changes too. Do not infer support when it is unknown.

Do not finish with patches only in local package caches or generated Pods files. If an update raises the minimum OS or requires major API changes, consider a version or small adapter preserving older-OS support. If no solution is possible, report the required decision and impact while completing independent fixes.

Simulator build success does not prove device linking or archive success. For binary or embedding changes, also validate available device builds/archives. Preserve existing signing/account settings and record checks that cannot run. Do not start by deleting all caches; use an isolated build directory when needed to distinguish cache effects.

## Report format

`Package/old→new version | consuming targets | causal diagnostic | vendor evidence | lockfile/transitive changes | simulator/device/feature results | remaining limitations`

This is general dependency validation. Research vendor-specific iOS 27 support for the versions actually used.

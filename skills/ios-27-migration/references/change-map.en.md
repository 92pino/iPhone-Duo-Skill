# Release changes mapped to code checks

[한국어](change-map.md) | English | [日本語](change-map.ja.md)

These entries were verified in official Markdown on September 15, 2026. They are not exhaustive; recheck their status for the actual SDK/OS build. Search results are candidates. Verify calling paths and impact before changing code.

| Entry, official category, and ID | Search targets | Reproduction and minimal fix |
|---|---|---|
| SwiftUI tab selection / New Features / 164516837 | `TabView`, `selection`, `Tab`, `tag`, persisted tab IDs | With a 27 SDK build, hide the selected tab through login, permissions, or feature flags, or restore a saved selection. Normalize selection to an existing visible tab without exposing an invalid intermediate state during view and selection updates. Test deep links, relaunch, and logout |
| Foundation URL encoding / Resolved Issues / 161588649 | `URL(string:`, `URLWithString`, `addingPercentEncoding`, `removingPercentEncoding`, `%25` | Compare URLs mixing valid percent escapes with spaces/non-ASCII on old and target OS versions. Check whether a workaround for old double encoding now corrupts URLs. Validate path and query values separately; do not indiscriminately decode/encode the entire URL |
| App Intents photo schema / Known Issues / 181800016 | `AppEntity`, `.photos.asset` | Build existing conformers with the 27 SDK. Verify new required properties in SDK declarations and implement them within the applicable availability boundary. Preserve older-OS integration and do not introduce unused schemas |
| Swift dependency scanner / New Features / 136303612 | `module.modulemap`, `HEADER_SEARCH_PATHS`, multiple locations vending one module | Confirm duplicate Clang module names visible to the same scan using compiler diagnostics. Check whether SPM, Pods, and manually embedded frameworks duplicate a dependency. Minimally fix duplicate integration or search paths; do not patch SDK files or generated caches |

The first three entries come from [iOS 27 release notes](https://developer.apple.com/documentation/ios-ipados-release-notes/ios-ipados-27-release-notes); the last comes from [Xcode 27 release notes](https://developer.apple.com/documentation/xcode-release-notes/xcode-27-release-notes). New Features can affect existing code compatibility. Resolved Issues describe OS fixes, so do not add unnecessary new workarounds.

## Candidate searches

Restrict searches to actual project source directories. Exclude vendor/generated code initially, then inspect the relevant package separately when dependencies are implicated.

```sh
rg -n --glob '*.{swift,m,mm,h}' 'TabView|URL\(string:|URLWithString|addingPercentEncoding|removingPercentEncoding|AppEntity|\.photos\.asset' .
rg --files --hidden -g '!.git/**' -g '*modulemap' -g '*.xcconfig' -g 'Package.resolved' -g 'Podfile.lock'
```

Record the official item ID, applicable SDK/OS, file/symbol, and reproduction result for each candidate. Mark absent APIs not applicable and documentation-only checks not run. Proposed fixes and tests here are practical guidance, not an exhaustive list of Apple implementation requirements.

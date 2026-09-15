# iOS 27 Migration

[한국어](SKILL.md) | English | [日本語](SKILL.ja.md)

Inspect existing SwiftUI/UIKit apps for iOS 27 SDK build and runtime compatibility, fix verified API or behavioral regressions, and validate the results. Use for iOS 27 migration, post-update errors, and compatibility reviews. New feature adoption and Duo-specific folding or multidisplay design are outside this scope.

Read one language version and report in the user's language. Translations are not separate skills; update all three together.

## Scope

For reviews, report evidence and findings. For implementation requests, finish feasible changes and validation. Do not treat new features, a UI redesign, blanket dependency upgrades, or changes to the minimum OS or Swift language mode as default migration work. Preserve existing platform support, data, and user flows.

## 1. Establish a baseline

- Read project instructions and working tree changes. Identify the workspace/project, schemes, app/extension/test targets, deployment targets, dependency lockfiles, and CI build workflow.
- Record `xcodebuild -version`, `xcodebuild -showsdks`, and `xcrun swift --version`. Discover runtimes and destinations with `xcrun simctl list devices available` when needed.
- Distinguish **running an app built with the old SDK on iOS 27** from **running an app rebuilt with the iOS 27 SDK**. Separate SDK-linked behavior changes from OS changes. Record Xcode/SDK/OS builds for each result.
- Run relevant baseline builds and tests or obtain existing CI evidence. Record pre-existing failures and continue feasible inspection. A missing SDK/runtime means not run, not compatible.

## 2. Verify evidence and impact

Read the [checklist and official sources](references/checklist.en.md) and investigate only frameworks the app uses. Release notes change; do not treat a beta number in a search snippet as the current version. Record the actual page version, check date, relevant entry, and affected files/symbols.

Describe each finding as `reproduction → expected/actual behavior → affected files → evidence → minimal fix → validation`. Distinguish API changes, app defects, dependency issues, known OS/SDK issues, and environment problems. Search matches or deprecation warnings alone do not justify replacement.

## Detailed inspection procedures

| Situation | Reference |
|---|---|
| Map release changes to code and reproduction | [Change map](references/change-map.en.md) |
| Verify data and login after updating an older app | [Update installation](references/upgrade-validation.en.md) |
| Investigate package, binary, and plugin build errors | [Dependencies](references/dependencies.en.md) |

For a full migration, perform all three procedures where applicable. For a specific error, read only relevant references.

## 3. Apply necessary fixes

- Prioritize build failures and reproducible regressions. Verify names, signatures, modules, and availability against local SDK declarations and official documentation.
- `#available` branches on runtime OS; it cannot make declarations absent from an older SDK compile. `canImport` does not guarantee a symbol exists inside a module. With an older SDK, finish feasible changes without inserting unverified API code.
- For deprecated APIs, check removal timing, actual impact, and behavioral differences in replacements. Avoid broad refactors merely to eliminate warnings.
- Fix actor isolation/Sendable errors at ownership and call boundaries. Do not hide them with indiscriminate `@unchecked Sendable`, `nonisolated(unsafe)`, or disabled checks.
- Limit dependency changes to what resolves a verified problem and review lockfile changes. With generators such as Tuist, edit source manifests instead of generated files.
- Verify known OS issues on the affected build. If a workaround is needed, document its applicability and removal conditions while preserving behavior on other OS versions.
- Validate existing data and recovery when changing persistence, migrations, authentication, or payment flows. Do not erase user data or perform real purchases or external transmissions for test convenience.

## 4. Validate and report

Build affected targets, run relevant tests, and exercise changed user flows on available iOS 27 environments and previously supported OS versions. Add tests when they verify a reproduced defect or meaningful state transition.

Record **pass / fail / not run / not applicable**. Separate static inspection, builds, simulator tests, and physical-device results. Report findings and changed files, evidence links, commands/environments/results, remaining limitations, and steps to repeat validation. Without app sources or runtime access, do not present skill-document validation as app compatibility verification.

If Duo-specific folding, hinge, or multidisplay work is also requested, use the installed `iphone-duo` skill alongside this one. General compatibility checks must remain usable without another skill.

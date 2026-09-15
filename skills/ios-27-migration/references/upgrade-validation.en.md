# Update installation and data preservation

[한국어](upgrade-validation.md) | English | [日本語](upgrade-validation.ja.md)

Run clean installation and update installation separately. Use test devices, accounts, and synthetic data without deleting real user data.

## Preparation and sequence

1. Obtain the actual previous release artifact or a reproducible build of that version. If rebuilt, record that it differs from the distributed artifact. Compare version/build, bundle ID, signing team, keychain access groups, App Groups, and storage schema.
2. Create synthetic data using the old app: records and relationships, settings, drafts, test login, offline data, and test purchase state if applicable. Record expected IDs, counts, relationships, and readability rather than secret values.
3. Stop the old app and install the new version over it **without uninstalling or resetting**. Use the same bundle ID and compatible signing/entitlements. Verify that data was retained. Do not bypass installation failures by uninstalling.
4. On first launch, check store opening, migration, relationships, settings, and session restoration. Distinguish expected reauthentication after expiration from data loss. When app/extensions share App Group data, verify reads and writes from both.
5. Add, edit, and delete records, relaunch, and test offline/online recovery. Check that repeated first-launch execution does not duplicate migrations or synchronization.
6. Reproduce migration failures and interruptions in a separate disposable environment using existing test hooks or fixtures. Preserve a copy of original test data and verify recovery, retry, and error UI. Do not conceal data loss by replacing a failed store with an empty one.

## Combinations and limitations

- Record app updates separately from device OS updates. Copying data into a new simulator does not verify an actual device OS update.
- Installing the old app on an iOS 27 runtime and updating it can exercise the app-update path. Test actual OS upgrades separately on available test devices. Do not invent unsupported runtime-upgrade commands.
- Do not mix simulator builds and physical-device artifacts. Differences in keychain access between App Store and development-signed apps do not by themselves establish an OS regression.
- Without a previous artifact, use schema fixtures for partial checks and mark full update installation not run. Fixtures produced only by new code do not prove old-version compatibility.
- Perform clean installation on a separate device/container. Test downgrades separately only when the product supports them; do not assume old binaries can read new schemas.

## Completion evidence

Report old/new artifact identifiers, OS builds, installation sequence, before/after test-data comparisons, and first-launch, relaunch, and recovery results. An opened store alone does not prove preservation of data, relationships, or accounts. This is a general app-update procedure, not a new iOS 27 requirement.

# iPhone Duo Skill

[한국어](README.md) | English | [日本語](README.ja.md)

![Concept render of a foldable iPhone Duo showing a Mail draft alongside the Photos library in split-screen](assets/iphone-duo-hero.jpg)

An original concept render illustrating side-by-side Mail and Photos use; it is not an actual product photo or precise hardware drawing.

An iOS development skill available in Korean, English, and Japanese. It guides inspection, implementation, and validation of iPhone Duo adaptations in SwiftUI and UIKit apps.

## What actually matters on this device

![Diagram connecting closed, partially folded, and open poses to available space, fold avoidance, expanded layout, and preserved user state](assets/what-matters.svg)

As the pose changes, so does the space available to the app. Lay out content from the current view or scene size and safe areas. Use reserved-region APIs only when needed and after confirming them in the installed SDK. Keep drafts, selection, and playback position across transitions. This conceptual diagram summarizes [Apple's Duo developer material](https://developer.apple.com/iphone-duo/).

## Included skills

| Skill | Scope |
|---|---|
| [iphone-duo](skills/iphone-duo/SKILL.en.md) | Duo folding, layouts, multiple displays, and cameras |
| [ios-27-migration](skills/ios-27-migration/SKILL.en.md) | iOS 27 change mapping, update installation, dependency checks, fixes, and validation |

## Improvements

- Verify APIs against the installed SDK and preserve fallbacks for older OS versions.
- Preserve navigation and editing state across folding and resizing.
- Apply guidance for safe areas, vertical bars, multiple scenes, and camera switching according to the app's needs.
- Check accessibility, localization, and error recovery, distinguishing completed validation from untested behavior.
- Include Codex UI metadata, official sources, and upstream license attribution.

Read the [English skill guide](skills/iphone-duo/SKILL.en.md). The single discovery entrypoint is [SKILL.md](skills/iphone-duo/SKILL.md); it links to the English translation. Read one language version and only the references needed for the task. All three versions describe the same workflow and should be updated together.

## Installation

Use `npx skills` to list or install the skills in this repository.

```sh
# List available skills
npx skills add DamDamStudio/iPhone-Duo-Skill --list

# Install a specific skill
npx skills add DamDamStudio/iPhone-Duo-Skill --skill iphone-duo
npx skills add DamDamStudio/iPhone-Duo-Skill --skill ios-27-migration
```

To install both skills globally for Codex:

```sh
npx skills add DamDamStudio/iPhone-Duo-Skill \
  --skill iphone-duo \
  --skill ios-27-migration \
  --agent codex \
  --global
```

### Manual installation

If you have downloaded this repository, run these commands from its root. If a skill with the same name exists, the commands stop without overwriting it.

```sh
skill_root="${CODEX_HOME:-$HOME/.codex}/skills"
mkdir -p "$skill_root"
if [ -e "$skill_root/iphone-duo" ] || [ -L "$skill_root/iphone-duo" ]; then
  echo "iphone-duo already exists. Review the existing files before merging."
else
  cp -R skills/iphone-duo "$skill_root/iphone-duo"
fi
```

For the other skill, replace `iphone-duo` with `ios-27-migration`. For Claude Code, copy the selected skill directory into the project's `.claude/skills/` directory. Keep its references and translations when installing.

The iOS 27 skill focuses on preserving existing behavior without adding new features. Install either skill independently.

## Example requests

```text
$ios-27-migration Inspect this app for iOS 27 compatibility, fix verified problems, and validate the changes.
$iphone-duo Adapt this app for Duo. Implement what the installed SDK supports and verify the changes.
$iphone-duo Check that the detail screen and draft input survive folding and unfolding.
$iphone-duo Fix camera switching, mirroring, and recording continuity in this camera screen.
```

As checked on September 15, 2026, [Apple's announcement](https://developer.apple.com/iphone-duo/) listed Xcode 27.1 beta as coming later that month. If the SDK is unavailable, the skill directs the agent to make general adaptive UI improvements and record Duo-specific validation separately.

## License and sources

See this repository's [MIT license](LICENSE) and the [upstream MIT notice](skills/iphone-duo/LICENSE.upstream). Official references and the scope of verification are documented in [Sources](skills/iphone-duo/references/en/sources.md).

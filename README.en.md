# iPhone Duo Skill

[한국어](README.md) | English | [日本語](README.ja.md)


An iOS development skill available in Korean, English, and Japanese. It guides inspection, implementation, and validation of iPhone Duo adaptations in SwiftUI and UIKit apps.

## Improvements

- Verify APIs against the installed SDK and preserve fallbacks for older OS versions.
- Preserve navigation and editing state across folding and resizing.
- Apply guidance for safe areas, vertical bars, multiple scenes, and camera switching according to the app's needs.
- Check accessibility, localization, and error recovery, distinguishing completed validation from untested behavior.
- Include Codex UI metadata, official sources, and upstream license attribution.

Read the [English skill guide](skills/iphone-duo/SKILL.en.md). The single discovery entrypoint is [SKILL.md](skills/iphone-duo/SKILL.md); it links to the English translation. Read one language version and only the references needed for the task. All three versions describe the same workflow and should be updated together.

## Installation

Run these commands from the repository root. If a skill with the same name exists, the commands stop without overwriting it.

```sh
skill_root="${CODEX_HOME:-$HOME/.codex}/skills"
mkdir -p "$skill_root"
if [ -e "$skill_root/iphone-duo" ] || [ -L "$skill_root/iphone-duo" ]; then
  echo "iphone-duo already exists. Review the existing files before merging."
else
  cp -R skills/iphone-duo "$skill_root/iphone-duo"
fi
```

For Claude Code, copy the same `skills/iphone-duo` directory into the project's `.claude/skills/` directory. Keep all references, translations, and `LICENSE.upstream` when installing.

## Example requests

```text
$iphone-duo Adapt this app for Duo. Implement what the installed SDK supports and verify the changes.
$iphone-duo Check that the detail screen and draft input survive folding and unfolding.
$iphone-duo Fix camera switching, mirroring, and recording continuity in this camera screen.
```

As checked on September 15, 2026, [Apple's announcement](https://developer.apple.com/iphone-duo/) listed Xcode 27.1 beta as coming later that month. If the SDK is unavailable, the skill directs the agent to make general adaptive UI improvements and record Duo-specific validation separately.

## License and sources

See this repository's [MIT license](LICENSE) and the [upstream MIT notice](skills/iphone-duo/LICENSE.upstream). Official references and the scope of verification are documented in [Sources](skills/iphone-duo/references/en/sources.md).

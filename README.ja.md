# iPhone Duo Skill

[한국어](README.md) | [English](README.en.md) | 日本語

韓国語・英語・日本語で提供する iOS 開発スキルです。SwiftUI と UIKit アプリの Duo 対応について、調査・実装・検証を支援します。

## 改善点

- インストール済み SDK で API を確認し、旧 OS の代替動作を維持
- 開閉・リサイズ時のナビゲーション状態と編集中の内容を保持
- safe area、垂直バー、複数 scene、カメラ切り替えを用途に応じて判断
- アクセシビリティ・ローカライズ・エラー復旧を確認し、実施済みと未検証を区別
- Codex 用 UI メタデータ、公式出典、原著作物のライセンスを収録

[日本語スキルガイド](skills/iphone-duo/SKILL.ja.md)を参照してください。登録用の入口は [SKILL.md](skills/iphone-duo/SKILL.md) の1つで、各言語版にリンクしています。ガイドは1言語だけ読み、必要な参照のみ追加で読みます。各言語版は同じ手順を説明しており、変更時は一緒に更新します。

## インストール

リポジトリのルートで実行します。同名のスキルがある場合は上書きせずに停止します。

```sh
skill_root="${CODEX_HOME:-$HOME/.codex}/skills"
mkdir -p "$skill_root"
if [ -e "$skill_root/iphone-duo" ] || [ -L "$skill_root/iphone-duo" ]; then
  echo "iphone-duo は既に存在します。既存ファイルを確認してから統合してください。"
else
  cp -R skills/iphone-duo "$skill_root/iphone-duo"
fi
```

Claude Code では同じ `skills/iphone-duo` フォルダをプロジェクトの `.claude/skills/` にコピーできます。参照資料、翻訳、`LICENSE.upstream` も一緒に保持してください。

## 使用例

```text
$iphone-duo このアプリを Duo に対応させて。現在の SDK で可能な修正から実装して検証して。
$iphone-duo 開閉時に詳細画面と入力中の内容が保持されるか確認して。
$iphone-duo カメラ画面の切り替え・ミラーリング・録画継続の問題を修正して。
```

2026-09-15 の確認時点で、[Apple の案内](https://developer.apple.com/iphone-duo/)は Xcode 27.1 beta を同月後半に提供予定としていました。SDK が不足する場合は一般的な適応型 UI の改善を進め、Duo 固有の検証は別途記録する設計です。

## ライセンスと出典

本リポジトリの [MIT ライセンス](LICENSE)と原著作物の [MIT 表記](skills/iphone-duo/LICENSE.upstream)を参照してください。公式資料と確認範囲は[出典](skills/iphone-duo/references/ja/sources.md)に記載しています。

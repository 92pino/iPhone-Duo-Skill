# 依存関係の互換性

[한국어](dependencies.md) | [English](dependencies.en.md) | 日本語

## 一覧と再現

1. 実際の `Package.swift`、`Package.resolved`、`Podfile`、`Podfile.lock`、`Cartfile.resolved`、Tuist manifest、手動 framework/XCFramework 接続を探します。未使用の管理ツールを導入しません。
2. 直接・間接依存ごとに固定バージョン、利用 target、source/binary/macro/plugin の種別、必要 toolchain・最低 OS、供給元の公式 release note URL を記録します。対応宣言とローカル検証は別欄にします。
3. 固定バージョンで基準ビルドし、最初の原因 diagnostic を確保します。ダウンロード・認証失敗はコンパイル非互換ではありません。ネットワーク失敗でバージョンや lockfile を変えません。

## 種別ごとの確認

| 種別 | 調査・修正 |
|---|---|
| SPM source | tools version、platform 条件、product/target 接続、実際の resolved 版。既存 resolve 手順を使い一括 update しない |
| CocoaPods | lockfile とインストール状態、使用 workspace、生成 xcconfig 接続。既存 Bundler 環境を使い原因パッケージに限定して更新 |
| Binary XCFramework | device/simulator の platform variant と architecture を両方確認。同じ arm64 でも platform は異なり得る。Swift module/interface の toolchain 互換性、embed/link、間接 framework、resource bundle を確認 |
| macro/build plugin | ホスト macOS・toolchain・実行権限とアプリ target エラーを区別。実際の diagnostic と供給元の対応版を確認し、検証を一律無効化しない |
| 重複モジュール | [変更マッピング](change-map.ja.md)の 136303612 に従い SPM/Pods/手動接続と header search path の重複を確認 |

## 最小変更と検証

再現 → 供給元の公式修正根拠 → 最小互換バージョン選択 → manifest/lockfile 差分確認 → 利用 target ビルド → 対象機能実行の順に進めます。間接依存の変更も報告し、不明な対応状況を推測で「対応」としません。

ローカル package cache や Pods 生成物だけの修正で終わらせません。更新で最低 OS が上がるか大きな API 変更が必要なら、旧 OS を維持できる版や小さい adapter を検討します。解決不能なら必要な選択と影響を報告し、独立した修正は完了します。

simulator ビルド成功を実機 link・archive 成功とみなしません。binary/embed の変更では可能な device build/archive も確認します。署名・アカウント設定を維持し、実行できない項目を記録します。全キャッシュ削除を初手にせず、必要なら隔離 build directory でキャッシュの影響を区別します。

## 報告形式

`パッケージ/旧→新版 | 利用 target | 原因 diagnostic | 供給元の根拠 | lockfile/間接変更 | simulator/device/機能結果 | 残る制約`

これは一般的な依存検証です。iOS 27 に対する供給元ごとの対応状況は実際に使う版で調査します。

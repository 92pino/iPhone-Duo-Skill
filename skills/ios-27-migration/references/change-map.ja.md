# 実際の変更とコード点検

[한국어](change-map.md) | [English](change-map.en.md) | 日本語

2026-09-15 に公式 Markdown 本文で確認した項目です。全変更の一覧ではなく、実行時に対象 SDK/OS build の状態を再確認します。検索結果は候補です。修正前に呼び出し経路と実際の影響を確認します。

| 項目・公式分類・番号 | 検索対象 | 再現と最小修正の判断 |
|---|---|---|
| SwiftUI タブ選択 / New Features / 164516837 | `TabView`、`selection`、`Tab`、`tag`、保存したタブ ID | 27 SDK ビルドで選択タブをログイン・権限・feature flag により隠すか、保存した選択を復元します。存在し表示可能なタブに選択を正規化し、画面と選択の更新途中も不正状態を露出させません。deep link・再起動・ログアウトをテスト |
| Foundation URL エンコード / Resolved Issues / 161588649 | `URL(string:`、`URLWithString`、`addingPercentEncoding`、`removingPercentEncoding`、`%25` | 有効な percent escape と空白・非 ASCII が混在する URL を旧/対象 OS で比較します。旧二重エンコードの補正が URL を壊さないか確認します。path と query 値を別々に検証し、URL 全体を無条件に decode/encode しません |
| App Intents 写真スキーマ / Known Issues / 181800016 | `AppEntity`、`.photos.asset` | 既存 conformer を 27 SDK でビルドします。新しい必須プロパティは SDK 宣言で確認し、適切な availability 境界内で実装します。旧 OS の連携を維持し、未使用のスキーマを追加しません |
| Swift dependency scanner / New Features / 136303612 | `module.modulemap`、`HEADER_SEARCH_PATHS`、同じモジュールの複数提供経路 | 同じ scan に見える Clang モジュール名の重複を compiler diagnostic で確認します。SPM・Pods・手動 framework で依存が重複していないか調査します。重複接続や検索経路を最小修正し、SDK ファイルや生成キャッシュを直接変更しません |

最初の3項目は [iOS 27 リリースノート](https://developer.apple.com/documentation/ios-ipados-release-notes/ios-ipados-27-release-notes)、最後は [Xcode 27 リリースノート](https://developer.apple.com/documentation/xcode-release-notes/xcode-27-release-notes)で確認しました。New Features でも既存コードの互換性に影響します。Resolved Issues は OS 側の修正なので不要な workaround を追加しません。

## 候補検索

実際のソースディレクトリに範囲を絞ります。最初は vendor/generated を除外し、依存が原因なら該当パッケージを別途確認します。

```sh
rg -n --glob '*.{swift,m,mm,h}' 'TabView|URL\(string:|URLWithString|addingPercentEncoding|removingPercentEncoding|AppEntity|\.photos\.asset' .
rg --files --hidden -g '!.git/**' -g '*modulemap' -g '*.xcconfig' -g 'Package.resolved' -g 'Podfile.lock'
```

各候補に公式番号、適用 SDK/OS、ファイル・シンボル、再現結果を記録します。未使用 API は対象外、文書確認だけなら未実施です。本書の修正・テスト案は実務上の判断であり、Apple の必須要件の全てを示すものではありません。

# 互換性チェックリスト

[한국어](checklist.md) | [English](checklist.en.md) | 日本語

実務用の調査範囲であり、全項目が iOS 27 で変更されたという意味ではありません。使用機能と実際のリリースノートに合わせて選択します。

| 領域 | 調査・修正基準 |
|---|---|
| ビルド・依存関係 | SDK 宣言、compiler diagnostic、app/extension target、パッケージ対応、lockfile、リンクエラー |
| SwiftUI・UIKit | タブ選択の有効性、navigation 復元、sheet/popover anchor、safe area・キーボード、状態保持 |
| 並行処理・ライフサイクル | actor 隔離、UI 更新場所、キャンセル・重複 task、foreground 復帰、observer 解放 |
| 保存・ネットワーク | 既存データの読み書き、失敗時の復旧、オフライン、認証期限切れ、応答エラー |
| 権限・メディア | 拒否・制限、カメラ/音声 interruption、復帰、実機での保存結果 |
| 既存のシステム連携 | deep link、通知、background 作業、widget/extension、テスト環境での購入・復元 |
| アクセシビリティ・旧 OS | 大きい文字、VoiceOver、RTL/長い翻訳、最低対応 OS の代替動作 |

SwiftUI アプリではリリースノートの `TabView` と `selection` を検索し、SDK に連動する選択制約を確認します。非表示や使用不可のタブを選ぶ経路を調べ、実際の対象版で再現してから修正します。この例のために未使用 API を導入しません。

## 検証の組み合わせ

| ビルドと実行 | 目的 |
|---|---|
| 既存 SDK + 既存 OS | 既存の失敗と正常動作の基準 |
| 既存 SDK + iOS 27、既存 artifact がある場合 | OS 更新だけの影響 |
| iOS 27 SDK + iOS 27 | 再ビルド後の問題と SDK 連動動作 |
| iOS 27 SDK + 既存対応 OS | 旧 OS の回帰と availability による代替動作 |

利用できる組み合わせだけ実行し、残りは未実施と記録します。ビルド成功だけではアクセシビリティ・カメラ・background 動作は確認できません。報告するログから個人情報を除きます。

## 公式出典

- [iOS & iPadOS 27 リリースノート](https://developer.apple.com/documentation/ios-ipados-release-notes/ios-ipados-27-release-notes)
- [Xcode 27 リリースノート](https://developer.apple.com/documentation/xcode-release-notes/xcode-27-release-notes)
- [iOS & iPadOS リリースノート一覧](https://developer.apple.com/documentation/ios-ipados-release-notes)

2026-09-15 に公式 Markdown 本文で確認した具体的な変更は[変更マッピング](change-map.ja.md)に記載しています。検索要約の beta 表記より本文の版と項目状態を確認し、作業時の SDK/OS build と照合します。

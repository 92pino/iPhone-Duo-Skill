# SDK と API の確認

[한국어](../compatibility.md) | [English](../en/compatibility.md) | 日本語

2026-09-15 の確認時点で、[Apple の Duo 案内](https://developer.apple.com/iphone-duo/)は Xcode 27.1 beta を同月後半に提供予定としていました。これは確認時点の記録です。実行時に公式案内とインストール済み SDK を再確認します。

## 3つを個別に確認

| 確認対象 | 根拠 | 対応 |
|---|---|---|
| コンパイル可否 | 選択した SDK の header/Swift interface と実際のビルド | 宣言がなければビルド対象のソースに追加しない |
| 実行 OS の対応 | 宣言の availability とデプロイ対象 | 確認したバージョンで `#available` または `@available` と代替動作を構成 |
| 現在の機器・scene の機能 | runtime capability と接続状態 | 変化や非対応の場合を処理 |

`if #available(iOS 27.1, *)` だけでは、旧 SDK にない型のコンパイルエラーを防げません。`#if canImport(SwiftUI)` も SwiftUI 内部の新しい型の存在を確認しません。Swift コンパイラのバージョンを iOS SDK のバージョンの代わりに使いません。

公式ドキュメント、公式セッションコード、ローカル SDK の宣言、最小構成のビルドの順に必要な根拠を集めます。検索で見つからないことは API が存在しない証拠ではありません。一方、セッションに名前があってもインストール済み SDK にあるとは限りません。名前・モジュール・引数・availability を推測しません。

## 未確認のシンボル

`ArrangementView`、`UIArrangementViewController`、`reservedRegions`、`onHingeChange`、`UIHingeInteraction`、`axisBehavior`、`toolbarVerticalBehavior`、`CameraCaptureAccessory`、`AVCaptureDeviceDirectionCoordinator` は、原版や発表資料で扱われる**確認対象**です。コンパイル可能な宣言の一覧として扱いません。

SDK が不足する場合は既存コンテナのリサイズ、safe area、状態保持の改善を先に完了します。未確認の新機能は設計メモに残し、未解決シンボル、仮の stub、private API で埋めません。SDK 更新が必要ならバージョンと再検証項目を報告します。

ビルドはプロジェクトの既存コマンドに従います。Tuist では生成済みプロジェクトではなく元の manifest を編集します。実際の scheme と destination を調べて使用し、署名・アカウント設定を独断で変更しません。

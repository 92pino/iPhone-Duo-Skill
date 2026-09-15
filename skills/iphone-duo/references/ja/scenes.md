# Scene、ヒンジ、状態保持

[한국어](../scenes.md) | [English](../en/scenes.md) | 日本語

ヒンジは操作や効果に使い、レイアウトは利用可能なサイズとシステムのレイアウト情報で決めます。ヒンジのない機器の基本動作を維持し、連続イベントのたびに重い処理を実行しません。

## 状態の境界

- 文書・アカウント・保存データと、ウインドウごとの navigation path・選択・focus を分けます。
- サイズ変更のたびにモデルを作り直したり `.id` を変えたりして編集中の内容を失わないようにします。
- background/foreground や scene の再接続で購読を重複登録しません。画面が消えたら関連する observer/task を片付けます。
- 複数ウインドウでは現在の scene を使います。`connectedScenes.first` をユーザーの現在のウインドウと仮定しません。

複数ウインドウの対応は既存のアプリ設定と動的な利用可否を両方確認します。新規ウインドウ要求は失敗する可能性があるため、失敗理由を処理し、進行中の作業を保持します。Duo 対応だけのために不要なウインドウモデルを追加しません。

## Scene accessories

補助コンテンツが必要な場合だけ導入します。`CameraCaptureAccessory` などは [SDK 確認](compatibility.md)を行います。登録位置、availability の変化、消えた際のリソース解放を確認します。外部画面を失っても主画面の作業を続けられるようにします。共有データに接続しつつ、ウインドウごとの選択を無条件にグローバル化しません。

カメラを共有する画面では[カメラ参照](camera.md)も読みます。両画面が同じキャプチャ資源を独立して開始しないよう、所有権を決めます。

出典: [Leverage multiple displays and scenes on iPhone Duo](https://developer.apple.com/videos/play/tech-talks/111464/)。

# カメラ

[한국어](../camera.md) | [English](../en/camera.md) | 日本語

## 選択と切り替え

既存アプリの解像度・フレームレート・深度・同時キャプチャの要件を先に確認します。原版の virtual front camera で満たせるなら手動切り替えを追加しません。物理カメラが必要なら対応 device と format を実行時に列挙します。製品名だけで解像度や depth 対応を保証しません。

`AVCaptureDeviceDirectionCoordinator` と inner/outer device type は [SDK 確認](compatibility.md)後に使います。direction は view 基準なので、2画面で1つのグローバルな方向値を共有しません。

## 実行とエラー処理

- 権限と usage description を確認し、拒否・制限・カメラ不在の場合に代替 UI を提供します。
- session の開始・停止・再構成は既存の serial executor またはカメラ actor に集約します。UI と preview の更新は main actor で行います。actor だけで blocking 処理が適切な executor 上に移るわけではないため、既存の並行処理設計を確認します。
- 切り替え前に入力を追加できるか確認します。再構成に失敗したら元の入力に戻すか、明示的な失敗状態を提供します。構成トランザクションに start/stop が割り込まないよう直列化します。
- 連続開閉時の古い切り替え結果が最新の選択を上書きしないようにします。録画中の切り替えでは出力の対応と録画継続の要件を別々に確認します。
- interruption、background 移行、復帰、runtime error を処理します。画面解放後にカメラが動き続けないようにします。

## プレビューと保存結果

rotation coordinator の変化を監視し、preview と capture の角度を別々に適用します。対応する回転角度か確認します。ミラーリングと保存結果の向きは別々に検証します。aspect fill の切り抜きと aspect fit の余白は製品の意図に合わせて選びます。

原版のセンサー方向補正の無効化や dynamic aspect ratio 設定を一般規則にしません。SDK 宣言と使用する出力の要件を確認し、実際の保存画像・動画まで検証してから適用します。

シミュレータの結果は、カメラ切り替え・センサー・録画品質の実機検証にはなりません。機器がなければ未実施と記録します。

出典: [Build a great camera experience for iPhone Duo](https://developer.apple.com/videos/play/tech-talks/111465/)。

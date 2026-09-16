# レイアウト

[한국어](../layout.md) | [English](../en/layout.md) | 日本語

## 既存コードの点検

プロジェクトのソース内で次のパターンを検索し、呼び出しの文脈を読みます。検索結果だけで不具合とは判定しません。

```sh
rg -n --glob '*.swift' 'UIScreen\.main|userInterfaceIdiom|UIDevice\.current\.orientation|interfaceOrientation|ignoresSafeArea|safeAreaInsets|GeometryReader|frame\(width:' .
```

- コンテナのサイズはディスプレイ全体のサイズとは異なります。複数ウインドウでも現在の view サイズで計算します。
- compact/regular の変化に対応し、内側ディスプレイは常に regular と固定しません。
- カードの最小可読幅や本文の最大幅は維持できます。固定値や breakpoint を一律に削除しません。
- UIKit は safe area layout guide/Auto Layout を優先します。frame を直接計算するなら `view.bounds.inset(by: view.safeAreaInsets)` のように四辺を考慮し、レイアウト更新時に再計算します。
- SwiftUI の `ignoresSafeArea` は目的に合う背景に適用します。ボタンや本文まで無条件に広げません。キーボード表示中も入力欄と送信操作が見えるか確認します。

## 折り目への適応

原版の reserved regions は分割領域と遮蔽領域を区別します。[SDK 確認](compatibility.md)後に使用し、座標系、有効状態、変更通知を確認します。safe area だけで全ての折り目領域を表せるとは仮定しません。

HIG が定義する reserved regions は 3 つです。外側の前面カメラ領域は常に存在し、Live Activities があると Dynamic Island に拡張します。内側の前面カメラ領域はカメラが有効なときだけ存在し、無効時は見えず、有効化した瞬間に UI が避けます。折り目領域は部分的に開いた状態でのみ条件付きで現れ、ヒンジ中央部分を除いて内側ディスプレイを分割します。alert・context menu・sheet などの標準コンポーネントはこれらの領域に自動対応するため、reserved region API の適用はカスタムコンポーネントに限って判断します。

主内容と補助内容を両方表示するなら split、前後関係があるなら overlay を検討します。`ArrangementView` 導入時は公式セッションの入れ子制約を確認します。原版では navigation container を arrangement の外に置き、arrangement を `List`/`ScrollView` の中に置かないよう案内しています。

スクロール中の記事やフィードを開閉のたびに別の領域へ移動させません。グリッドの偶数列は対称分割が有効な場合だけ検討し、狭い幅や大きい文字で強制しません。ヒンジ角度だけでレイアウトを計算しません。

代替レイアウトは既存の `NavigationStack`/`NavigationSplitView` と、対応 OS で利用できる stack/grid で構成します。ビュー構造が変わっても stable ID と状態の所有者を維持します。

出典: [適応型レイアウトのセッション](https://developer.apple.com/videos/play/tech-talks/111463/)、[デザインのセッション](https://developer.apple.com/videos/play/tech-talks/111466/)。原版の解釈と追加の実務基準は[出典記録](sources.md)を参照してください。

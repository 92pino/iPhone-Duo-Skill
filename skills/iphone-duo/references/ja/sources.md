# 出典と改善範囲

[한국어](../sources.md) | [English](../en/sources.md) | 日本語

確認日: 2026-09-16。バージョンと API は作業時点で再確認します。

## 基盤リポジトリ

[d-date/iphone-duo-skill](https://github.com/d-date/iphone-duo-skill)、commit `6f5374935c76d1a4be149cbded04571a9ac694b7`。
原版の主題構成と移行の観点をもとに、韓国語・英語・日本語で再構成しました。原著作物の著作権・MIT 表記は [LICENSE.upstream](../../LICENSE.upstream) に保存しています。

## Apple の一次資料

- [Duo 開発者案内と SDK 提供状況](https://developer.apple.com/iphone-duo/)
- [Prepare your app for iPhone Duo](https://developer.apple.com/videos/play/tech-talks/111461/)
- [Raise the bar with iPhone Duo](https://developer.apple.com/videos/play/tech-talks/111462/)
- [Strike a pose with adaptive layouts on iPhone Duo](https://developer.apple.com/videos/play/tech-talks/111463/)
- [Leverage multiple displays and scenes on iPhone Duo](https://developer.apple.com/videos/play/tech-talks/111464/)
- [Build a great camera experience for iPhone Duo](https://developer.apple.com/videos/play/tech-talks/111465/)
- [Design for iPhone Duo](https://developer.apple.com/videos/play/tech-talks/111466/)
- [Designing for iPhone Duo — HIG](https://developer.apple.com/design/human-interface-guidelines/designing-for-iphone-duo)

公式案内とセッションページの存在・主題を確認しました。2026-09-16 に HIG 本文全体を読み、reserved regions(外側/内側カメラ、折り目領域)の定義、内側ディスプレイが縦向きのときに横バーを維持する例外、Split View マルチタスキングのコントロール配置、arrangement view の split/overlay の区別を確認し、[レイアウト](layout.md)と[バー](bars.md)に反映しました。これは HIG 文書に書かれたデザインガイドラインの確認であり、全 API シグネチャや Duo runtime の動作を検証したという意味ではありません。インストール済み Xcode 26.4.1 / iOS SDK 26.4 では、Duo 新機能のコンパイル・実行検証は行っていません。

## この版で追加した基準

SDK 宣言と runtime availability の区別、状態保持、アクセシビリティ・ローカライズ、カメラのエラー処理、旧 OS の代替動作、検証結果の分類は実務用の補足です。Apple の必須要件として引用しません。

原版の一律の規則も条件付きの判断に変更しました。コンテンツに基づく breakpoint は許容し、グリッドの偶数列やセンサー補正の無効化は、対象画面・出力で必要かつ検証済みの場合のみ適用します。

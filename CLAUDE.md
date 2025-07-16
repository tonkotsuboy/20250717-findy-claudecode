# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

このリポジトリは、Findy向けの「2025年最新CSS実践テクニック」プレゼンテーション資料とCSSデモのコレクションです。Marpを使ったスライド資料と、最新CSS機能の実践デモで構成されています。

## Common Commands

### プレゼンテーションの開発

```bash
npm run dev          # Marpの開発サーバーを起動（ファイル監視+ローカルサーバー）
npm run build:pdf    # PDFファイルを生成
npm run build:html   # HTMLファイルを生成
```

### コード品質管理

```bash
npm run format       # 全ファイル（HTML, CSS, JS, MD）をPrettierでフォーマット
npm run format:md    # Markdownファイルのみフォーマット
npm run format:html  # HTMLファイルのみフォーマット
npm run format:css   # CSSファイルのみフォーマット
npm run format:js    # JavaScriptファイルのみフォーマット
npm run lint:text    # textlintでMarkdownの日本語表記をチェック
```

## Project Structure

```
/
├── index.md                 # メインプレゼンテーション（Marp形式）
├── bonus.md                 # ボーナスコンテンツ（Popover API等）
├── theme/style.css          # プレゼンテーション用カスタムテーマ
├── demo/                    # 番号順のCSS実践デモコレクション
│   ├── 1_hr-in-select/     # <hr> in <select>
│   ├── 2_align-content/    # align-content in ブロックレイアウト
│   ├── 3_webkit-no-longer-needed/  # backdrop-filter, background-clip: text
│   ├── 4_css-nesting/      # CSS Nesting
│   ├── 5_linear/           # linear() イージング関数
│   ├── 6_property/         # @property CSS変数アニメーション
│   ├── 7_starting-style/   # @starting-style
│   ├── bonus_has-email-form/  # :has() セレクター
│   └── bonus_popover/      # Popover API（ボーナス）
└── images/                 # プレゼンテーション用画像素材
```

## Architecture Overview

### スライド構成パターン

各技術セクションは以下の統一された流れで構成されています：

1. タイトルスライド（`_class: title`）
2. デモスライド（`_class: external-demo`）- URLなし
3. 従来の問題説明
4. モダンな解決法
5. 詳細解説
6. ブラウザ対応状況

### デモの命名規則

- 番号プレフィックス: スライド順序に対応（1_から7_まで）
- サブデモ: `1_basic/`, `2_advanced/`, `3_specific/`形式で分類
- ボーナス: `bonus_`プレフィックスで区別
- @propertyデモは複数バリエーション: `1_basic/`, `3_gradient-text/`, `4_color-palette/`, `5_transform-sequence/`

### Marp設定

- カスタムテーマ: `theme: kano`（`theme/style.css`で定義）
- HTML許可: `--html --allow-local-files`
- スライドクラス: `title`, `external-demo`, `message`, `chapter`, `description`等を使用
- 2列レイアウト: `column-2`クラスでコードと説明を並列表示

## Development Guidelines

### デモの実装原則

- HTMLデモは`index.html`, スタイルは`style.css`を基本とする
- 必要に応じて`script.js`を追加
- 実際のブラウザ対応状況を反映した実装
- 各機能の「従来の問題」と「モダンな解決法」を明確に対比

### 日本語コンテンツ品質

- textlintによる日本語技術文書の品質チェック
- 技術用語の統一（例: セレクター、プロパティ）
- ブラウザ対応状況は統一テーブル形式で記載（Can I Use情報を正確に反映）

### ファイル構成ルール

- メインコンテンツ: `index.md`
- 追加コンテンツ: `bonus.md`（時間に応じて使用）
- 画像: `images/`直下に配置
- デモ: 番号順で整理、複数バリエーションは子ディレクトリで管理
- 各デモは`index.html`、`style.css`、必要に応じて`script.js`で構成

### スライド構成のベストプラクティス

- 各技術セクションは「従来の問題」→「モダンな解決法」の流れ
- コードサンプルは`small-code`クラスで読みやすく
- ブラウザ対応状況は最新の:new:絵文字で強調
- AI系サイト風のモダンな色使い（Tailwind CSS風パレット）を推奨

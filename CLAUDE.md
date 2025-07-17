# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

このリポジトリは、**Claude Codeの実用的なtips集プレゼンテーション**をメインコンテンツとするFindy向け技術資料です。Marpを使ったスライド資料として作成され、既存のCSS実践テクニックプロジェクトをベースとして活用します。

### プロジェクト構成

1. **Claude Code Tips スライド**（メインコンテンツ - `.kiro/specs/`で仕様管理）
   - Hooks、Aqua Voice連携、カスタムスラッシュコマンド、Puppeteer MCP、Kiro連携の5つの事例
   - 各事例は概要・実例・デモの3部構成
   - `claude-code-tips.md`として実装予定

2. **CSS実践テクニック**（参考資料 - `index.md`, `bonus.md`）
   - 2025年最新CSS機能の実践デモとスライド
   - 既存の完成済みプロジェクト（Marpテーマとスライド構成の参考として活用）

## Common Commands

### プレゼンテーションの開発

```bash
npm run dev          # Marpの開発サーバーを起動（ファイル監視+ローカルサーバー）
npm run build:pdf    # PDFファイルを生成（実際のコマンドはbuid:pdf）
npm run build:html   # HTMLファイルを生成（実際のコマンドはbuid:html）
```

**注意**: package.jsonでは `buid:pdf` と `buid:html` とタイポがありますが、この通りに実行してください。

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
├── claude-code-tips.md      # Claude Code Tips プレゼンテーション（メインコンテンツ - 作成予定）
├── index.md                 # CSS実践テクニック（参考資料）
├── bonus.md                 # CSS実践テクニック ボーナスコンテンツ（参考資料）
├── theme/style.css          # プレゼンテーション用カスタムテーマ
├── demo/                    # CSS実践デモコレクション + Claude Code動画デモ
│   └── demo-placeholder.mp4 # Claude Code Tipsデモ用プレースホルダー動画
├── images/                 # プレゼンテーション用画像素材
└── .kiro/                  # Kiro AI用プロジェクト仕様管理
    └── specs/claude-code-tips-slides/  # Claude Code Tipsスライドの要件・設計・タスク
        ├── requirements.md  # 機能要件定義
        ├── design.md       # 設計仕様
        └── tasks.md        # 実装タスク一覧
```

## Architecture Overview

### プレゼンテーション構成アーキテクチャ

このリポジトリのメインコンテンツであるClaude Code Tipsプレゼンテーションと、参考資料として活用するCSS実践テクニックは異なる構成を持ちます：

#### Claude Code Tips（メインコンテンツ - 作成対象）
各事例は以下の3部構成：

1. **概要スライド**（`_class: title`）- 機能の基本概念と特徴
2. **実例スライド**（`_class: description`）- 具体的な使用例とコード例（2カラムレイアウト）
3. **デモスライド**（`_class: external-demo`）- プレースホルダー動画によるデモ実演

対象事例：Hooks、Aqua Voice連携、カスタムスラッシュコマンド、Puppeteer MCP、Kiro連携

#### CSS実践テクニック（参考資料）
各技術セクションは以下の統一された流れで構成：

1. タイトルスライド（`_class: title`）
2. デモスライド（`_class: external-demo`）- URLなし
3. 従来の問題説明
4. モダンな解決法
5. 詳細解説
6. ブラウザ対応状況

### CSS デモの命名規則（参考資料）

- 番号プレフィックス: スライド順序に対応（1_から7_まで）
- サブデモ: `1_basic/`, `2_advanced/`, `3_specific/`形式で分類
- ボーナス: `bonus_`プレフィックスで区別
- @propertyデモは複数バリエーション: `1_basic/`, `3_gradient-text/`, `4_color-palette/`, `5_transform-sequence/`

### Marp設定とスライドクラス

- **カスタムテーマ**: `theme: kano`（`theme/style.css`で定義）
- **HTML許可**: `--html --allow-local-files` 
- **重要なスライドクラス**:
  - `title`: タイトルスライド用
  - `description`: 説明スライド用（2カラムレイアウト対応）
  - `external-demo`: デモスライド用（フルスクリーン動画表示）
  - `message`: メッセージスライド用
  - `chapter`: チャプター区切り用
  - `finish`: 終了スライド用

### Kiro AI連携仕様

このプロジェクトでは`.kiro/specs/`ディレクトリでKiro AIによる要件定義・設計・タスク管理を行います：

- **要件管理**: 各機能をユーザーストーリー形式で管理
- **設計管理**: スライド構成とコンポーネント設計を詳細化
- **タスク管理**: 実装タスクを段階的に分解して管理
- **実装**: Claude Codeが設計に基づいて効率的に実装

## Development Guidelines

### Claude Code Tips スライド実装ガイドライン

#### 必須実装要件
- **統一構成**: 各事例は概要・実例・デモの3部構成を厳守
- **プレースホルダー動画**: `demo/demo-placeholder.mp4`を全デモスライドで使用
- **既存資産活用**: 挨拶・まとめスライドは既存のものを最大限再利用
- **Marpクラス**: 既存の`title`、`description`、`external-demo`クラスを活用

#### 事例別実装要件
1. **Hooks**: タスク前後の自動化処理（音声通知の実装例）
2. **Aqua Voice連携**: 高品質音声入力による効率化
3. **カスタムスラッシュコマンド**: PR作成・コミット分離の自動化
4. **Puppeteer MCP**: GitHubスクリーンショット自動アップロード
5. **Kiro連携**: 要件・設計・実装の役割分担ワークフロー

### CSS実践デモの実装原則（参考資料として活用）

- HTMLデモは`index.html`, スタイルは`style.css`を基本とする
- 必要に応じて`script.js`を追加
- 実際のブラウザ対応状況を反映した実装
- 各機能の「従来の問題」と「モダンな解決法」を明確に対比

### 日本語コンテンツ品質管理

- textlintによる日本語技術文書の品質チェック
- 技術用語の統一（例: セレクター、プロパティ）
- ブラウザ対応状況は統一テーブル形式で記載（Can I Use情報を正確に反映）

### ファイル構成ルール

- **Claude Code Tips**: `claude-code-tips.md`（メインコンテンツ - 作成対象）
- **CSS実践**: `index.md`、`bonus.md`（参考資料）
- **画像**: `images/`直下に配置
- **デモ**: 番号順で整理、複数バリエーションは子ディレクトリで管理
- **各デモ**: `index.html`、`style.css`、必要に応じて`script.js`で構成

## Puppeteer Settings

### ログイン状態を保持したブラウザアクセス

Puppeteer経由でGitHubなどのサイトにアクセスする際、ログイン状態を保持するには以下の設定を使用：

```javascript
// 推奨設定
launchOptions: {
  "headless": false,
  "args": ["--no-sandbox", "--disable-setuid-sandbox"],
  "userDataDir": "/Users/takeshi.kano/.puppeteer-sessions/github"
}
allowDangerous: true
```

この設定により、Claude Codeの別セッションでも同じログイン状態が維持される。

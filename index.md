---
marp: true
theme: kano
---

![bg](images/cover.png)

---

<!--
_class: message
-->

# 鹿野 壮

Claude Codeにタスク丸投げおじさん

![w:300](images/twitter.png)

@tonkotsuboy_com

---

# 本日の構成

<div class="text-note">

1. Hooks - タスク前後の自動化
2. Aqua Voice連携 - 高品質音声入力
3. カスタムスラッシュコマンド - 事前登録処理
4. Puppeteer MCP - GitHubスクリーンショット自動化
5. Kiro連携 - 要件・設計・実装の役割分担

</div>

---

<!--
_class: title
-->

# Hooks

タスク実行前後の自動化処理

---

<!--
_class: description
-->

# Hooksとは

- タスク開始時や終了時など、Claude Codeのさまざまな時点で任意のシェルコマンドを実行できる
- 例
  - タスク終了後にコードフォーマットをかける
  - タスク開始時の音声通知
  - コマンド実行中の状況把握
  - ログ記録の自動化

https://docs.anthropic.com/ja/docs/claude-code/hooks

---

# 実例:タスク終了後にオリジナル音声を鳴らす

<div class="annotation-lang">~/.claude/settings.json</div>

```json
  "hooks": {
    "Stop": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "afplay /Users/takeshi.kano/.claude/buhi.m4a"
          }
        ]
      }
    ]
  }
```

</div>

---

<!--
_class: external-demo
-->

<video controls>
<source src="demo/demo-placeholder.mp4" type="video/mp4">
</video>

---

<!--
_class: title
-->

# Aqua Voice連携

高品質音声入力による効率化

---

<!--
_class: description
-->

# Aqua Voiceとは

- 高精度な音声認識とリアルタイム処理により、自然な日本語の文字起こしができる
- 言い間違いや「あー、うー」なども補正してくれる

https://withaqua.com/

---

# 実例:音声でタスク実行

- Claude Codeに音声で指示を出す
- 文字を長々打つよりもラク
- オフィスでやるとちょっと恥ずかしい

<div class="annotation-lang">音声入力例</div>

```
「ユーザーのプロフィール編集画面を作成して。
フォームバリデーションも含めて。
テストも書いて。」
```

---

<!--
_class: external-demo
-->

<video controls>
<source src="demo/demo-placeholder.mp4" type="video/mp4">
</video>

---

<!--
_class: title
-->

# カスタムスラッシュコマンド

事前登録処理の素早い実行

---

<!--
_class: description
-->

# スラッシュコマンドとは

- あらかじめ登録した処理を実行できる
- 毎回長いプロンプトを書く必要がない
- 組み込みスラッシュコマンド
  - `/doctor` claude codeのバグをチェック
  - `/review` コードレビューをリクエスト
https://docs.anthropic.com/ja/docs/claude-code/slash-commands

---

# カスタムスラッシュコマンドとは

- ユーザーが独自に登録した処理を実行できる
- 毎回命令している長いプロンプトがあればカスタムスラッシュコマンドに登録しておく方が便利
- .claude/commands/コマンド名.mdに処理を記述
- スコープを設定できる
  - グローバルで使う
  - プロジェクトで共有する
  - プロジェクトで自分だけが使う

https://docs.anthropic.com/ja/docs/claude-code/slash-commands

---

# 実例:PR作成の自動化

毎回やっているフォーマットとPR作成処理をカスタムスラッシュコマンド化した

<div class="annotation-lang">~/.claude/commands/create-pr.md</div>

```markdown
## Description
このコマンドは以下の作業を自動で実行します：
1. `npm run format` でPrettierフォーマットを実行
2. 変更内容を適切な粒度でコミットに分割
3. GitHub PRを作成

## Implementation
prettierをかけたあと、適切な粒度でコミットし、PRを作って
**使用例:** `/create-pr` → 自動でPR作成完了
```

---

<!--
_class: external-demo
-->

<video controls>
<source src="demo/demo-placeholder.mp4" type="video/mp4">
</video>

---

<!--
_class: title
-->

# Puppeteer MCPでスクショを撮って<br>PRに貼り付ける

---

<!--
_class: description
-->

# MCPとPuppeteer

- MCP
  - Model Context Protocol（MCP）はClaude Codeを外部システムと連携させる仕組み
- Puppeteer
  - ブラウザ操作の自動化
  - スクリーンショット取得

https://docs.anthropic.com/ja/docs/claude-code/mcp

---

# 実例:GitHubスクリーンショット自動化

- AIエージェント全盛期の現代、大量のPRが作られるがレビューが大変
- コードの差分だけを追うよりも、スクリーンショットをベースに対話したほうが、レビュワー・レビューイともにわかりやすい
- Puppeteer MCPを使って自動化した

<div class="annotation-lang">~/.claude.json</div>

```markdown
"mcpServers": {
  "puppeteer": {
    "command": "npx",
    "args": [
      "@modelcontextprotocol/server-puppeteer"
    ]
  }
}
```

---

<!--
_class: external-demo
-->

<video controls>
<source src="demo/demo-placeholder.mp4" type="video/mp4">
</video>

---

<!--
_class: title
-->

# Kiro連携

---

<!--
_class: title
-->

# Kiroとは

- 2025/07/15にAmazonがリリースしたIDE
- 要件・設計・実装計画の対話形式で作成できる
- 実装の実行もできるが、Claude Codeに比べると遅い

https://kiro.dev/blog/introducing-kiro/

---

<!--
_class: description
-->

# 実例: Kiroで仕様を作成し、Claude Codeで実装する

- Kiroは対話形式で詳細な要件書・設計書を作れるが、実装速度が遅い
- Claude Codeは爆速開発ができるが、正確な指示出しが難しい
- 2つの長所を組み合わせることで、質と速度の両取りができた

https://zenn.dev/ubie_dev/articles/kiro-claude-code

---

<!--
_class: external-demo
-->

<video controls>
<source src="demo/demo-placeholder.mp4" type="video/mp4">
</video>

---

<!--
_class: message
-->

# ご清聴ありがとうございました

Claude Codeでもっと楽に開発しましょう！

---

<!--
_class: finish
-->

![w:600](images/finish.png)

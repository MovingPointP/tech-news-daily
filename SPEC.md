# tech-news-daily 仕様書

## 概要

毎日トレンドの技術記事を自動収集・要約し、GitHubリポジトリに保存するルーティン。

---

## 実行設定

| 項目 | 設定 |
|---|---|
| 実行時刻 | 毎日 午前6時 JST（cron: `0 21 * * *` UTC） |
| 実行環境 | Claude Code リモートエージェント（Anthropicクラウド・PC不要） |
| 実行モデル | claude-sonnet-4-6 |
| ルーティンID | `trig_014qYYVhpEBfaQN1sMtztD1U` |
| ルーティン管理URL | https://claude.ai/code/routines/trig_014qYYVhpEBfaQN1sMtztD1U |
| 使用ツール | Bash, Read, Write, Edit, Glob, Grep, WebFetch |

---

## 取得対象サイト

| サイト | 件数 | トレンド基準 | 取得方法 |
|---|---|---|---|
| Hacker News | 5件 | 当日のトップストーリー | Firebase REST API（IDリスト → 各記事を個別取得） |
| Zenn | 5件 | 当日のトレンド | `zenn.dev/api/articles?order=trending` |
| Qiita | 5件 | 過去7日以内に投稿された記事をストック数順 | `qiita.com/api/v2/items?query=created:>7日前&sort=stock&per_page=20`（重複除外後5件） |
| Dev.to | 5件 | 当日のトップ記事 | `dev.to/api/articles?top=1&per_page=20`（重複除外後5件） |

---

## 要約

| 項目 | 設定 |
|---|---|
| 言語 | 日本語 |
| 長さ | 10文程度 |
| 対象 | 全サイトの全記事（英語記事も日本語で要約） |

---

## 保存先

| 項目 | 設定 |
|---|---|
| サービス | GitHub リポジトリ |
| リポジトリ名 | `tech-news-daily` |
| 公開設定 | Public |
| ファイル構造 | `YYYY/MM/DD.md`（日付ごとに1ファイル） |

### ファイル構造例

```
tech-news-daily/
├── 2026/
│   ├── 05/
│   │   ├── 22.md
│   │   ├── 23.md
│   │   └── ...
│   └── 06/
└── seen_urls.txt
```

---

## 重複排除

| 項目 | 設定 |
|---|---|
| 管理ファイル | `seen_urls.txt`（リポジトリルート） |
| 保持期間 | 30日分 |
| 動作 | 取得した記事のURLが `seen_urls.txt` に存在する場合はスキップし件数に数えない。新規記事のURLは実行後に追記。30日以上前のURLは自動削除。 |

---

## GitHub連携

| 項目 | 設定 |
|---|---|
| リポジトリURL | https://github.com/MovingPointP/tech-news-daily |
| GitHub App | Claude（Anthropic製）を `tech-news-daily` リポジトリにインストール済み |
| claude.ai連携 | コネクタからGitHub連携を設定済み |
| コミット方法 | BashツールでGitコマンドを実行（`git add . && git commit && git push`） |
| seen_urls.txt記録形式 | `YYYY-MM-DD URL`（1行1件） |

---

## 出力フォーマット

```markdown
# 技術記事まとめ — YYYY-MM-DD

## Hacker News

### [記事タイトル](URL)

要約（日本語・10文程度）

---

## Zenn

### [記事タイトル](URL)

要約（日本語・10文程度）

---

## Qiita

### [記事タイトル](URL)

要約（日本語・10文程度）

---

## Dev.to

### [記事タイトル](URL)

要約（日本語・10文程度）
```

# ingraft Claude Code チーム設定プラン

## Context

ingraft inc. は2-3人（エンジニア＋非エンジニア混合）のチームで、Claude Codeチームプランを活用して以下3つの業務を自動化・効率化したい：
1. 自社サイト管理（Figma実装、デプロイ、アクセス解析）
2. 動画制作（FFmpegベースの自動編集）
3. SNS運用（X/Instagram/YouTube投稿、トレンド分析）

**基本方針: 誰が作業しても同じ結果を得られる環境を構築する。**
- 全ての設定・規約・ワークフローはGit管理下のファイルで共有
- MCP設定は `.mcp.json`（プロジェクトルート）でチーム共有、秘密情報は `${ENV_VAR}` で参照
- 権限設定は `.claude/settings.json` でチーム共有
- 新メンバーは `git clone` → 環境変数設定 → MCP認証 の3ステップで即作業開始可能
- CLAUDE.mdにオンボーディング手順を完備

## 実装状況

### Phase 1: 基盤構築 ✅ 完了

| ファイル | 状態 | 内容 |
|----------|------|------|
| `index.html` | ✅ 削除 | テスト用ファイル |
| `CLAUDE.md` | ✅ 作成 | プロジェクト規約・オンボーディングガイド・ワークフロー定義 |
| `.mcp.json` | ✅ 作成 | Slack（OAuth）+ GitHub（`${GITHUB_PAT}`）のMCP定義 |
| `.claude/settings.json` | ✅ 作成 | チーム共有の権限設定（git, ffmpeg, curl等） |
| `.claude/launch.json` | ✅ 作成 | ローカルプレビューサーバー（python3 http.server:8080） |
| `.claude/skills/deploy.md` | ✅ 作成 | サイトデプロイスキル |
| `.claude/skills/figma-to-code.md` | ✅ 作成 | Figma→コード変換スキル |
| `.claude/skills/sns-post.md` | ✅ 作成 | SNS投稿原稿作成スキル |
| `.claude/skills/trend-report.md` | ✅ 作成 | トレンドレポートスキル |
| `.claude/skills/analytics-report.md` | ✅ 作成 | アクセス解析レポートスキル |
| `.claude/skills/video-edit.md` | ✅ 作成 | 動画編集スキル |
| `.github/workflows/deploy.yml` | ✅ 作成 | GitHub Pages自動デプロイ |
| `.gitignore` | ✅ 作成 | 秘密情報・大容量ファイル除外 |
| `sns/calendar.md` | ✅ 作成 | コンテンツカレンダーテンプレート |
| `sns/templates/` | ✅ 作成 | 投稿テンプレート用ディレクトリ |
| `sns/drafts/` | ✅ 作成 | 下書き保存用ディレクトリ |
| `video/scripts/` | ✅ 作成 | 動画台本用ディレクトリ |
| `video/exports/` | ✅ 作成 | 動画書き出し用ディレクトリ |

---

### Phase 2: SNS + アナリティクス（API準備後）

| タスク | 状態 | 内容 |
|--------|------|------|
| X API準備 | 未着手 | Developer Account → `TWITTER_API_KEY` 等 |
| Instagram API準備 | 未着手 | Facebook Business Account → `INSTAGRAM_ACCESS_TOKEN` 等 |
| YouTube API準備 | 未着手 | Google Cloud Console → `YOUTUBE_API_KEY` 等 |
| GA4 API準備 | 未着手 | GA4 Data API → `GA_PROPERTY_ID` 等 |
| スケジュールタスク登録 | 未着手 | morning-summary, weekly-analytics, trend-report |

### スケジュールタスク実行環境

| タスク | 実行環境 | 理由 |
|--------|----------|------|
| morning-summary (平日8:50) | Mac mini (Claude Code) | Gmail MCPアクセスが必要 |
| weekly-analytics (月曜9:15) | GitHub Actions | APIベースでデータ取得 |
| trend-report (火・木10:05) | GitHub Actions | Web検索・API呼び出しのみ |
| SNSカレンダーリマインダー | GitHub Actions → Slack | 単純な通知 |

**Mac miniセットアップ**: 同じチームアカウントでログイン、同じリポジトリをクローン、環境変数設定、Claude Codeセッションを常時起動。スリープ無効にしておく。

---

### Phase 3: 動画制作パイプライン（FFmpegインストール後）

| タスク | 状態 | 内容 |
|--------|------|------|
| FFmpegインストール | 未着手 | `brew install ffmpeg` |
| YouTube自動アップロード | 未着手 | YouTube Data API v3 + OAuth2 |

---

## セキュリティ要件

- **`.mcp.json`（Git管理）**: サーバー定義のみ、秘密情報は `${ENV_VAR}` で参照
- **`.claude/settings.json`（Git管理）**: 権限設定のみ
- **`~/.claude/settings.json`（個人）**: 個人固有の追加MCP設定
- **現状の問題**: GitHub PATがリテラル文字列で個人settings.jsonに記載 → 環境変数に移行必要
- **`.gitignore`**: `.env`, `*.key`, `video/exports/`, `sns/drafts/` を除外済み

## ホスティング

- **推奨**: GitHub Pages（静的サイト、コスト0、pushで自動デプロイ）
- **移行手順**: GitHub Pages有効化 → CNAME設定 → さくらサーバーからDNS変更
- **CI/CD**: `.github/workflows/deploy.yml` で `main` push時に自動デプロイ
